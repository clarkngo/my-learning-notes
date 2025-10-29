---
title: "Ads Shepherd Control Center"
layout: default
parent: Ebay
---

# Ads Control Center (Ads Shepherd)

The **Ads Control Center**, also known as **Ads Shepherd**, is a tool designed to help users monitor and optimize the performance of eBay ads.

## Core Functionalities

With the Control Center, users can perform several key actions:
*   **Monitor the health** of the ads infrastructure systems globally.
*   **Check the performance** of PLS, PLA, PLX, and other ads programs using various metrics.
*   **Create and track custom performance metrics**.
*   **Define alert rules** to detect potential threats and inconsistencies.
*   **Create and manage incidents** to resolve issues identified by alert rules.
*   **Evaluate the impact** of experiments and recent code changes on ads performance.

***

## Key Control Center Pages

### Home Page

The home page dashboard provides a quick overview of ads infrastructure health and core metrics data across major eBay sites.

#### Global Health
The Global Health map helps assess the health status of eBay sites globally. A site's status ranges from **Healthy (green) to Fatal (red)**, depending on the number of unresolved **P1 and P2 incidents** raised for that site. This status refreshes every 24 seconds.

The map also displays real-time business metrics compared with last week's data. Metrics included are:
*   PL Revenue (Overall revenue from Promoted Listings).
*   PLS Revenue (Promoted Listings Standard).
*   PLA Revenue (Promoted Listings Advanced).
*   PLX Revenue (Promoted Listings Express).

#### Program Metrics (PLS, PLA, PLX)
Tabs for PLS, PLA, and PLX offer additional performance metrics specific to those programs over the last 24 hours. The metrics data refreshes every 5 minutes.

Key metrics available include:
*   **PLS Sold Adrate (%)**: Equals PLS revenue divided by PLS GMV.
*   **PLS Attribution Ratio (%)**: Equals PLS GMV divided by PLS-enabled GMV.
*   **PLA CPC**: Average cost per click for PLA.
*   **PLX Impressions** and **PLX Clicks**.

#### Changes and Incidents
The home page also lists the most recent changes released for different ads programs and displays high-priority incidents that are ongoing and active.

### Core Metrics

The Core Metrics page uses an interactive chart to track metrics data for all major ads programs. The chart can be used to analyze trends, detect abnormal performance, and predict stable values based on past performance.

By default, the timeline uses **Mountain Standard Time (MST, -07:00)** for data consistency, though the time zone can be changed.

#### Data Sources
The chart data can originate from several sources:
*   **Realtime**: Data fetched from Oracle tables.
*   **Offline**: Data dumped to Elasticsearch regularly.
*   **Nous**: Business metrics data.
*   **NRT Pipeline**: Optimized near real-time data.
*   **NuMonitor**: Real-time data from eBay transactions (Seller Hub) and merch recommendation data (ClickHouse). This source supports additional filtering options, such as filtering by Algo Family.

#### Example Metrics
Available metrics include Revenue, Impression, Clicks, Sales, and GMV (Gross merchandise value). Calculated metrics include:
*   **ROAS** (Return on advertising spent): GMV divided by revenue.
*   **CPC** (Cost per click): Revenue divided by clicks (available for PLA and others).
*   **CTR** (Click through rate).

#### Filters
Users can adjust the chart using filters such as:
*   **Site** (one or more eBay sites).
*   **Page Family** (SRP, Merch, Off eBay).
*   **Platform** (dWeb, mWeb, Others).
*   **Algo Family**.
*   **Sale Type**: Direct Sale (clicked promoted item purchased within 30 days) or Halo Item Sale (another promoted item purchased within 30 days).

#### Tools
Tools add extra dimensions to the chart:
*   **Compare History**: Compares data with the previous week and up to three weeks ago.
*   **Anomaly Detection**: Shows periods when abnormal behavior was detected in the previous three hours.
*   **Flat Currency**: Uses a flat currency exchange rate to ensure monetary data is presented in the same currency (enabled by default).
*   **Predict Data Lag**: Allows the chart to predict stable values for the last three days based on historical data lag metrics.
*   **Show Changes**: Adds timeline markers showing when a change occurred.

### Metrics Manager and Explorer

The **Metrics Manager** is a self-service tool used to define custom **metrics collection rules**.

#### Custom Metrics Collection Rules
Metrics can be collected from four types of sources:
1.  **Spark SQL** (HDFS or Hive tables).
2.  **ClickHouse SQL**.
3.  **ElasticSearch SQL**.
4.  **Prometheus QL**.

When defining a rule, users specify parameters such as Name, Data Source, Schedule (using cron format in MST timezone), and Target (Pronto is recommended, but HDFS is preferred for large results—50,000 rows or more). The rule can also define a **Backfill Time** and **Delta Time** to manage data collection time ranges.

The collected data can be used to monitor custom metrics in the **Metrics Explorer**.

#### Metrics Explorer
The Metrics Explorer enables monitoring the performance of custom metrics defined in the Metrics Manager. When viewing a graph, users select the rule name/ES Index, the Metric Name, and an **Aggregator** (e.g., AVG, COUNT, SUM). Data can also be split based on a specific dimension selected in the **Bucket Name** field.

### Alerts and Incident Management

#### Alerts
Alerts are triggered when specific conditions defined in an underlying **alert rule** are met. High priority alerts (P1 and P2) are published in the **#ads\_p1p2\_alerts Slack channel**.

Alert statuses include: **TRIGGERED** (new), **ACKNOWLEDGED** (being worked on), and **RESOLVED**. Alerts can be managed (acknowledged or resolved) in the Control Center or through the Slack channel.

#### Alert Rule Manager
Alert rules are defined with mandatory parameters including Name, Type, Priority (P1 highest to P4 lowest), Team, Domain, and **Revenue Impact**. Alert rules can be based on data derived from metrics or events.

#### Incident Management
An incident is created when an alert or unusual behavior requires additional action from the Ads Infra team. Incidents are tickets used to track the issue until resolution.

The incident schema includes fields defining:
*   `severity` (Enum: P1, P2, P3, P4).
*   `business_impact` (1: yes, 0: no).
*   `revenue_loss` and `revenue_recovered`.
*   `status` (Enum: Active, Resolved).
*   `root_cause` category.
*   `tags` (e.g., PLA, PLX).
*   Links for `slack`, `snow` (ServiceNow), and `rca` (Root Cause Analysis).

### Ads Scheduler
The Ads Scheduler page allows users to define ads workflows. Workflows can be configured manually or by uploading a YAML file declared in the **Airflow-style directed acyclic graph (DAG) format**.

Workflow components define attributes like `dag_id`, `email` recipients, `timeout`, and `retries`. Tasks within the workflow are defined by `task_type` (e.g., bash, python, spark, pyspark), `settings`, and `depends_on` relationships. Workflows can be triggered and deployed in either the pre-production or production environment.
