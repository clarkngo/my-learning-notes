---
title: Confluent Apache Flink
layout: default
parent: Workshops
---

## 🏛️ Modern Data Architecture & Stream Processing

### Data Lakehouse: Data Warehouse + Data Lake

---

### 📉 Inefficiencies of Traditional ELT Pipelines

ELT pipelines are described as **brittle, slow, and inefficient**.

* **Operational Data:** Ingestion occurs in large, infrequent batches (e.g., 5 / 30 / 60 batch ingestion).
* **Data Warehouse/Data Lake:** Requires complex remodeling and reprocessing, leading to high costs ($$$).
* **Dashboards/Reports/ML/AI:** Results in poor decision-making due to stale data.

#### Traditional Data Flow Comparison

| Flow Type | Path |
| :--- | :--- |
| **ETL** | Operational Systems $\to$ ELT/ETL $\to$ Analytical System |
| **Reverse ETL** | Analytical System $\to$ Operational Systems $\to$ ELT/ETL |

### ⬅️ Shifting Data Governance to the Left (Stream Processing)

Shifting the governance of data to the left means moving towards processing data as soon as it's created.

**How Shift Left Works:**
* Write Your Data Once, Read It as a Stream or Table.

---

## ⚡ Real-time Stream Processing Applications

### Data Pipelines
* Real-time search index building
* ML pipelines

### Real-time Analytics
* Ad/campaign performance
* Content performance

### Event Driven Applications
These applications analyze data streams over a time window:
* Fraud detection
* Anomaly detection
* Alerting/notifications
* Routing
* Business process monitoring
* Bad experience detection

---

## 💻 Stream vs. Batch Compute Models

Real-time services rely on stream processing.

| Component | Data at Rest (Traditional) | Data in Motion (Streaming) |
| :--- | :--- | :--- |
| **App Layer** | Web App | Streaming Applications |
| **Compute Layer** | Traditional Databases | Apache Flink |
| **Storage Layer** | File systems | Apache Kafka |

### The Streaming Stack

1.  **Basic Streaming:** Databases/Custom apps/3rd Party apps $\to$ **Kafka** $\to$ Downstream apps
2.  **Stream Processing (Enhanced):** Databases/Custom apps/3rd Party apps $\to$ **Flink** (Stream processing) $\to$ **Kafka** (Storage) $\to$ Downstream apps (DB $\to$ Queries)

---

## ⚙️ Apache Flink & ksqlDB

### Apache Flink

* **Definition:** Real-time Stream Processing.
* **Key Features (Why Flink):**
    * Fault tolerance
    * Language flexibility
    * Unified processing (supports both stream and batch)
* **Real-world Use Cases:**
    * **Netflix:** Personalized recommendations
    * **Stripe:** Real-time fraud detection

### ksqlDB

* **Definition:** SQL built on top of Kafka Streams.
* **Context:** Used for defining streams and tables and running continuous queries on Kafka data using a familiar SQL interface.

---

## 🛠️ Hands-on Workshop Focus

### Goal
* Create data products for a third-party reseller.

### Activities
* Explore various data sources.
* Build ad-hoc queries to aggregate data.
* Use different joins to create data products.

### Scenario Examples
* Identify unique orders
* Filter valid orders
* Generate customer product topic
* Promotion calculation for electronics

### Hands-on Architecture Pipeline


`Data sources` $\to$ `Dedepulcation` $\to$ `Data filtering` $\to$ `Data Enrichment` $\to$ (`Promotion Calculation` **AND** `Loyalty Levels`)

### Tools Used
* Cloud Console Workspace
* Flink Shell
* Flink Monitoring
* Flink SQL (used to create Kafka topics as tables)

### Operations (Key Benefits)
* **Autoscale:** Autoscale within CFUs (Confluent Flink Units).
* **Increase without downtime:** Increase CFUs without downtime.
* **Manageability:** Delete Pools.

### Monitoring
* Visually track how data is transformed and processed by Flink.
* Visually track how data flows to and from Flink.
* Inspect a Flink query.

### Elasticity
* Scale elastically to meet changing business needs.
* 100% elasticity during the workshop (CFUs grow based on workload).

---

## 📖 Advanced Flink SQL Capabilities

Flink SQL is multi-tenant and capable of elastic scaling.

* **User-Defined Functions (UDFs):** Extend Flink SQL functionality with custom UDFs.
* **Serverless Flink Accessibility:** With Table API support.

### Advanced SQL Streaming Operations

#### Time Windows


* **Time-based windows:** Windows defined by wall-clock time.
* **Event-density windows:** Windows defined by the rate of incoming events.
* **Event-based windows:** Windows where every single event can trigger a new window (e.g., sessions).

#### Pattern Matching
* Complex Event Processing (CEP).

#### Streaming Joins
* **Stream-to-stream joins:** Joining two continuous streams.
* **Temporal joins:** Joining a stream with a table based on time (e.g., price of an object at a particular date).
* **Lookup joins:** Joining a stream with an external database/store for enrichment.
* **Versioned joins:** Joining a stream with a versioned table (e.g., customer record history).

# Key differences between Flink, KStreams, and ksqlDB
| Attribute | CP Flink | CC Flink | Kafka Streams | ksqlDB |
| :--- | :--- | :--- | :--- | :--- |
| **Description** | Stream processing framework developed independent of Apache Kafka | | Embeddable client library for Java applications that is part of the Apache Kafka project | Stream processing framework that exposes Kafka Streams functionality through SQL |
| **Processing modes** | \* Unified stream and batch processing <br> \* Supports reads from multiple Kafka clusters | | \* Stream processing only <br> \* Supports reads from single Kafka cluster | \* Stream processing only <br> \* Supports reads from single Kafka cluster |
| **Pricing** | Restore state after failure from most recent incremental snapshot | | Restore state after failure by replaying all messages | Restore state after failure by replaying all messages |
| **CFLT deployment model** | \* Self-managed offering with Confluent Platform | \* Fully managed <br> \* No cluster deployment, scales to zero | \* Self-managed <br> \* Embeddable client library with no cluster | \* Fully managed and self-managed <br> \* Separate cluster deployment |
| **Language flexibility** | \* Full support of all Flink APIs (SQL, Table API, DataStream, ProcessFunction) | \* ANSI-compliant SQL <br> \* Java UDFs EA <br> \* Table API Open preview | Java (more flexible than SQL, but more complex) | SQL syntax inspired by ANSI SQL |

<br>

AI Model Inference in Confluent Cloud

Integrate remote AI models seamlessly into your data pipeline
- implement RAG to enrich LLMs models with real-time updates by using AI models

Here is the extracted information, which shows the syntax for referencing a hosted model and performing a model prediction, likely within a ksqlDB or similar stream processing environment:

## 🤖 Reference a Hosted Model

```sql
CREATE MODEL `env`.`db`.`model_name`
INPUT(prompt STRING)
OUTPUT(llm_response STRING)
WITH(
  'provider' = 'confluent',
  'confluent.model' = 'meta-llama/Llama-3.1-8B-Instruct'
);
```

### Key Elements:

  * **`CREATE MODEL`**: The command used to define the model reference.
  * **`model_name`**: The name given to the referenced model (e.g., `my_llama_model` in the prediction example).
  * **`INPUT(prompt STRING)`**: Specifies the input schema for the model, expecting a single string named `prompt`.
  * **`OUTPUT(llm_response STRING)`**: Specifies the output schema, expecting a single string named `llm_response`.
  * **`WITH(...)`**: Configuration for the model provider and the specific model.
      * `'provider' = 'confluent'`
      * `'confluent.model' = 'meta-llama/Llama-3.1-8B-Instruct'`

## 🧠 Model Prediction

```sql
SELECT * from text_input, LATERAL
TABLE(ML_PREDICT('my_llama_model', input));
```

### Key Elements:

  * **`SELECT * from text_input`**: Selects data from an input source (likely a stream or table named `text_input`).
  * **`LATERAL TABLE(...)`**: Used to apply a table function to each row of the input stream.
  * **`ML_PREDICT('my_llama_model', input)`**: The function that performs the model prediction, taking the name of the created model (`'my_llama_model'`) and the input column (`input`) as arguments.

-----

# Certification
- [Data Streaming Engineer Certification](https://developer.confluent.io/certification/)
 - [Apache Kafka® 101](https://developer.confluent.io/courses/apache-kafka/events/)
 - [Apache Flink® 101](https://developer.confluent.io/courses/apache-flink/intro/)
 - [Kafka® Connect 101](https://developer.confluent.io/courses/kafka-connect/intro/)
 - [Schema Registry 101](https://developer.confluent.io/courses/schema-registry/key-concepts/)
 - [Kafka® Streams 101](https://developer.confluent.io/courses/kafka-streams/get-started/)
