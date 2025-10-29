---
title: "Managing Elasticsearch & OpenSearch at Scale"
layout: default
parent: Ebay
---

# Learnings: Managing Elasticsearch & OpenSearch at Scale

This document captures our key learnings from using Elasticsearch (and our eventual move to OpenSearch) as a primary datasource. Our heaviest use cases led to significant operational challenges, especially around cluster balancing, disk utilization, and data management.

---

## The Core Problem: Unbalanced Clusters

While Elasticsearch and OpenSearch are powerful, their default shard allocation and rebalancing logic can struggle under specific conditions:

* **Uneven Shard Size:** Some indices grow much larger than others. The default allocator, which often balances by *shard count*, can place many large shards on one node and many small shards on another. This leads to one node having 90% disk usage while another has 30%.
* **High Shard Count:** A high number of total shards (often from a "time-based index" strategy) puts significant metadata overhead on the cluster master and makes rebalancing operations slow and complex.
* **"Hot Nodes":** The combination of uneven shard size and I/O patterns creates "hot nodes" that are bottlenecks for disk space, CPU, or I/O, even if the rest of the cluster is healthy.

Relying on default behavior led to frequent high-disk-usage alerts, which required manual intervention.

---

## Key Learnings & Best Practices

Our experience in managing these alerts led to several key best practices and platform-level learnings.

### 1. Default Allocation Isn't Enough
We learned that we cannot "set and forget" the cluster's allocation strategy. We must actively manage shard placement. This led to the realization that we needed custom tooling to supplement the built-in cluster logic.

### 2. Build Abstracted Tooling (Don't Use Raw APIs)
Our first instinct was to manually use the ES/OS `_cluster/reroute` API. This was slow, error-prone, and dangerous in a production environment.

**The Learning:** We built our own internal API service that sits *in front of* the raw cluster APIs. This service encapsulates the complex logic of:
* Finding the largest shards on the "hot" node.
* Finding the smallest shards on the "cold" nodes.
* Calculating a safe "swap" of shards.
* Executing the underlying reroute commands.

This abstraction turns a risky, multi-step manual process into a single, safe, repeatable API call.

### 3. The "Dry Run" Parameter is Non-Negotiable
The most critical feature we built into our custom tooling is a "dry run" mode (e.g., `execute=false`).

* **Before this:** Making a change was high-stakes. You'd "fire and hope."
* **After this:** Our standard procedure is to *always* run in preview mode first. The API returns a JSON payload of *exactly* what reroute commands it plans to execute.

**The Learning:** Never build a destructive or state-changing operational tool without a `dry-run` flag. The ability to preview a change is the most important safety feature you can have.

### 4. Differentiate Between Rebalancing and Scaling
Our SOPs (Standard Operating Procedures) must have two distinct branches for a "high disk" alert:

1.  **Is the *whole cluster* full?** If the average disk usage is high (e.g., >80%), no amount of rebalancing will help. The only fix is to scale the cluster (add more nodes/storage).
2.  **Is *one node* full?** If the cluster average is healthy but one node is hot (e.g., >85%), this is a *rebalancing* problem. This is when we use our custom tooling.

**The Learning:** An alert must first be triaged. Trying to rebalance a full cluster is a waste of time and can cause instability.

### 5. Proactive Monitoring is the Only Way
We cannot wait for the disk to be 95% full. By then, the cluster's built-in "low disk watermark" may have already triggered, marking indices as read-only and breaking ingest.

**The Learning:** Our alerts are set at a *proactive* threshold (e.g., 75% or 80%). This gives the SRE on-call enough time to run the `Check -> Preview -> Execute -> Verify` workflow *before* the cluster enters a critical, self-protecting (and service-impacting) state.

### 6. Use the Right Tool for Bulk Data I/O (Spring Batch)
When we needed to perform large-scale data insertion, retrieval, or re-indexing, simple scripts were not reliable.

**The Learning:** We adopted **Spring Batch** for heavy-duty data operations. A proper batch framework is essential because it provides critical features out-of-the-box that we would otherwise have to build:
* **Chunk-Based Processing:** Reading and writing in chunks (e.g., 1000 documents at a time) is far more memory-efficient and performant than document-by-document operations.
* **Job Restartability:** If a job fails after processing 8 million of 10 million documents, it can be relaunched and will restart from the point of failure, not from scratch. This is a lifesaver for long-running tasks.
* **Retry/Skip Logic:** It automatically handles transient network failures (retries) or bad data (skips) without failing the entire job.

**Takeaway:** Don't write custom scripts for large-scale data I/O. Use a robust framework like Spring Batch.

### 7. Kibana is the Go-To for Data-Level Debugging
Our monitoring dashboards (Grafana/Sherlock) are excellent for viewing *cluster health metrics* (disk, CPU, memory). However, they can't answer "What is *in* the data?"

**The Learning:** **Kibana** is the indispensable tool for *data-level* exploration and ad-hoc analysis. Its "Discover" tab is the fastest way for an SRE or developer to:
* Run ad-hoc queries to inspect raw JSON documents.
* Debug ingest problems by seeing exactly what data is arriving.
* Confirm the impact of a data change or backfill.
* Build quick, temporary visualizations to spot data anomalies (e.g., a sudden spike in a specific error code).

**Takeaway:** You need two complementary views: an **infrastructure monitoring view** (like Grafana) for cluster health and a **data exploration view** (Kibana) for ad-hoc querying and debugging.

---

## Our Generalized Operational Workflow (SOP)

This is the high-level workflow that resulted from these learnings.

**Alert:** A "High Disk Usage" alert fires from our monitoring platform for a specific node (e.g., `disk_usage > 75%`).

1.  **Check:**
    * Open the `[Our Monitoring Dashboard (e.g., Grafana/Sherlock)]` for the specific ES/OS cluster.
    * Verify the disk usage of all nodes.
    * **Triage:** Is this a "hot node" problem (go to step 2) or a "full cluster" problem (escalate to add storage)?

2.  **Preview (Dry Run):**
    * Call our internal balancing API in "dry run" mode. This API finds candidate shards to move.
    * **Endpoint:** `GET /v1/es/balance-nodes`
    * **Parameters:**
        * `threshold=[integer]`: (e.g., `75`) The % to target.
        * `node=[full-node-name]`: (Optional) Target a specific node.
        * `execute=false`: **This is the default and most important part.**
    * **Action:** Review the JSON response. It shows a list of proposed shard moves (e.g., "move large shard X from hot-node to cold-node"). Confirm this looks safe.

3.  **Execute:**
    * **Option A (Full Auto-Balance):**
        * Call the same API again, but with `execute=true`.
        * `GET /v1/es/balance-nodes?threshold=75&execute=true`
    * **Option B (Surgical Move):**
        * If we only want to execute *one* of the proposed moves, we use a different, more granular API, passing the specific move request from the "dry run" output.
        * `POST /v1/es/cluster-reroute`

4.  **Verify:**
    * Go back to the `[Our Monitoring Dashboard]`.
    * Watch the disk usage on the hot node decrease and the usage on the cold node(s) increase.
    * The goal is to see the target node's disk usage fall back below the 75% threshold.

5.  **Escalate:**
    * If balancing fails or if the cluster is generally too full, submit a request to the infrastructure team for more storage.