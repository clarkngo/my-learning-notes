---
title: "Platform Change and Metadata API Design"
layout: default
parent: Ebay
---

# Central Change Information System Learnings and Operational Overview

This document provides an extensive overview of the organization's **Centralized Change Database** (known internally as Changes Hub), focusing on its function as a critical tool for operational stability, its integration of Experimentation System (ES) data, and key learnings regarding Near Real-Time (NRT) data processing and triage procedures.

## 1. System Function and Value Proposition

The **Centralized Change Database** (Changes Hub) is a table residing on an internal platform (AirTable) that aggregates Change information from various sources. The system’s primary objective is to **provide a one-stop database to store Change information**.

This centralization is crucial for operational efficiency: when alerts are triggered, **on-call personnel can swiftly inspect the recent deployed changes and triage the root cause of alerts**. The Changes Hub provides a clear and concise user interface, often displayed in a timeline view, which is embedded within the **Control Center Metadata - Change page**.

## 2. Change Information Aggregated

The Changes Hub successfully aggregates and tracks historical change data across multiple categories.

| Type of Change | Metadata Type | Key Context |
| :--- | :--- | :--- |
| **Core Infrastructure Changes** | Code, Config, Milestone (Project) | Includes Code Rolls and configuration updates. Code Rollout and rollback are supported for specific infrastructure platforms, such as Raptor.io, Raptor, and NodeJS for Ads owned pools. |
| **Experimentation Changes** | EP (Experiment Platform), CR\_EP (Experiment from ServiceNow), PlannedEP. | **Unlaunched but planned EPs information are also available** on the Control Hub. EP changes are sometimes referred to as "touchstone" changes. Integration of ADP (Ads Platform) and PlannedEP changes into the Control Center has been achieved. |
| **Internal & Platform Changes** | DSS, Rheos. | Changes from the Ads Platform (ADP) are tracked. |
| **User Interface Changes** | MFE (Merch Front End). | |
| **General Tracking** | Launched changes. | |

The metadata schema utilizes various types, including ADP, Code, CR\_EP, DSS, MFE, and PlannedEP. The Elasticsearch index used for change metadata utilizes date properties such as `startAt`, `endAt`, `effectStart`, and `lastModifiedAt`.

## 3. Experimentation System (ES) Metadata Learnings

The effort to enhance ES metadata was specifically designed **to provide more visibility/observability when the root cause of a decrease in a business metric is related to experimental learning receiving traffic**.

### 3.1. Experiment Lifecycle and Change Request (CR) Insights

*   **Policy and Traceability:** A policy decision dictates that multiple Change Requests (CRs) should be created for EP ramp up/down operations (which may impact the site), rather than only for EP creation (which typically has no site impact). This practice was established to align with internal regulatory requirements (SEC) and enable better traceability for possible site impacts.
*   **Historical Context:** In the past, automatically generating CR tickets for all Experiment Platform changes was halted due to the extremely high volume, estimated at approximately **40,000 Experiment changes per day**. This volume made it challenging to locate the specific experiment needed during triage.
*   **Data Availability:** The system captures EP audit logs (EP history). Users can view EP state changes through these audit logs, noting that data collection begins when the **"Served Traffic Percentage != 0"** and the **"Experiment Status" is Active**.

### 3.2. Treatment Identifiers: XT and ET Tags

Two essential treatment identifiers are used in NRT monitoring:

| Tag Name | Definition and Usage | Insight/Learning |
| :--- | :--- | :--- |
| **Experiment Tag (XT)** | Originates from the application payload (`!xt`) of the entire Frontend Component (MFE) call. XT is the parent set of ET tags. | ES teams, especially for Page Optimization (PO), **prefer using the XT tag for analysis** because it focuses on traffic over all events and is considered **more accurate**. The NRT page supports retrieving data through the XT tag. |
| **Experiment Treatment (ET)** | Comes from the MFE placement level impression data (`soj impression`). It is generally on the placement level. | ET can be referenced when analyzing a specific placement executed by Page Optimization. The NRT system also supports retrieving data through the ET tag. |

## 4. Near Real-Time (NRT) Monitoring Infrastructure Learnings

Significant operational improvements were made to the Near Real-Time (NRT) monitoring pipeline, especially concerning data ingestion and quality.

### 4.1. NRT Data Quality and Metrics

*   **Confidence Level:** Verification comparing NRT data (Elasticsearch) and batch data (MADCDL) based on the XT tag revealed a high confidence level, with a **median difference ratio of 1%–2%**. This accuracy supports providing the data to PMs for real-time monitoring.
*   **Monitoring Capabilities:** NRT monitoring includes key metrics such as **Impressions, Clicks, and Click-Through Rate (CTR)**. **Purchase Through Rate (PTR)** is also an important metric. The NRT page supports **real-time time step selection** ranging from 5 minutes to 1 hour to 1 day.

### 4.2. Indexing and Data Processing Optimizations

Learnings related to managing high-volume data ingestion into Elasticsearch (ES index `ctr-xt*`):

1.  **Index Management:** Rollover policies were enabled in the Index Management Tool. These policies activate when an index reaches thresholds, such as **150 index size or 3 billion maximum documents**.
2.  **Performance Improvement:** To address abnormal write speed and huge data size, **parallelism was added** to the processing pipeline, reducing the end-to-end duration from 1 hour down to 2 seconds.
3.  **Duplicate Data Mitigation:** To resolve the issue of huge data loss caused by duplicate keys (a common problem in Flink real-time processing), a **second map-reduce aggregation round was added**. This aggregation significantly reduced the document count (from 10 million hits per hour to approximately 160k documents per hour) and reduced data size (from 1.5GB to 700MB), resulting in faster query speeds.

### 4.3. PromQL Monitoring Rules

Monitoring rules are set up in the Internal Metrics Service (ATMos) using Prometheus Query Language (PromQL).

*   A recording rule was created to track experiment treatment count changes per minute, grouped by `experiment_id`, `treatment_id`, and `request_group` (placement IDs):
    ```promql
    sum(increase(ep_treatment_total{_namespace_="reco", namespace="reco"}[1m])) by (experiment_id, treatment_id, request_group)
    ```
    This rule records the increase count by these identifiers per minute.
*   Other query examples include summing up the EP count by `experiment_id` (e.g., `sum(rr:mfe_ep_count{_namespace_="reco"}) by (experiment_id)`) or summing EP count matching a specific placement (e.g., `rr:mfe_ep_count{_namespace_="reco", request_group=~".*101110*."}`).

## 5. Change Triage and Verification Procedures (SOPs)

Standard procedures are established for verifying changes related to experiments and the Frontend Component.

### 5.1. Experiment (EP) Change Verification via Change Request System (CRS)

To find a corresponding Change Request (CR) ticket for an experiment in the Change Request System (ServiceNow):

1.  Navigate to Change Requests.
2.  Click **All** (to remove the default "Active = true" filter).
3.  Filter by typing **Experimental Learning** in the Assignment Group column.
4.  Search for the Experiment ID (`*{EPID}`) in the Short description column (e.g., `*100800`).

If the EP CR is found in ServiceNow but **NOT** visible in the Control Center, relevant support personnel should be contacted. If the EP CR is **NOT** found in ServiceNow, a troubleshooting ticket should be filed.

### 5.2. Experiment Verification via Experiment Management Tool (EMT)

If a specific known or suspected experiment or experiment change event (such as traffic ramp up/down) is not visible on the Control Center changes dashboard, the Experiment Management Tool (Touchstone) can be used.

1.  Search using the `placementId` in the upper right search box.
2.  Change the view to **All Experiments**.
3.  Verify the experiment by viewing the Design Tab for **Factor to confirm placementId**.
4.  Check the **Audit Logs** to view EP state changes, confirming that data collection starts when **"Served Traffic Percentage != 0"** and the **"Experiment Status" is Active**.

### 5.3. Frontend Component (MFE) Change Verification

A specific URL in the Change Request System (ServiceNow) is available for locating change requests related specifically to the Merch Front End (MFE).

## 6. Related APIs and Tools

Various internal systems and APIs support the centralized change tracking and metadata ingestion:

*   **Central Monitoring Dashboard (Control Center):** The main portal for viewing aggregated change metadata and NRT metrics.
*   **Change Request System (CRS/ServiceNow):** The primary source system for change tickets (e.g., CHG8147386).
*   **Experiment Management Tool (Touchstone):** Provides APIs for managing experiments, retrieving EP history, audit logs, and general experiment raw data.
*   **Internal Ads Metadata Service API:** Provides endpoints to show Experiment history (e.g., `/history/{id}`) and general EP information (e.g., `/{id}`).
*   **Internal Catalog/Config Service (Homesplice) API:** Used for retrieving configuration data such as Variants, Features, and Configs.
*   **Internal Placement ID API:** An endpoint is available to show all active production placement IDs.