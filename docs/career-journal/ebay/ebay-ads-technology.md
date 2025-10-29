---
title: "Ebay Ads Technology"
layout: default
parent: Ebay
nav_order: 2
---

# eBay Ads: Technical Implementation & Architecture

## 1. High-Level System Architecture

The eBay Ads platform is a large-scale, real-time distributed system designed to serve relevant ads to millions of users while processing massive volumes of data. Its primary goal is to balance **user relevance**, **seller value** (sales), and **platform revenue**.

The architecture can be broken down into four main pillars:

1.  **Campaign Management:** The seller-facing systems (UI and API) that allow sellers to create and manage their ad campaigns (e.g., set ad rates, bids, budgets).
2.  **Ad Serving (Real-Time):** The "front-end" of the ad system. This is a low-latency service that selects and returns the "best" ad in milliseconds when a user loads a page.
3.  **Data Pipeline (Near Real-Time & Batch):** The "back-end" data-processing infrastructure. It collects, processes, and aggregates impression, click, and sales data for reporting, billing, and machine learning.
4.  **Reporting & Analytics:** The services that query the processed data to show performance dashboards to sellers.

---

## 2. The Ad Serving Lifecycle (Real-Time Request)

This is the sub-second process that occurs every time a page with ad slots is loaded.

1.  **Page Request:** A user loads an eBay page (e.g., a search results page for "red running shoes").
2.  **Ad Call:** The page's frontend code makes an asynchronous request to the **Ad Server**. This request includes critical context:
    * **User Data:** User ID (if logged in), browser cookies, device type.
    * **Contextual Data:** Search query ("red running shoes"), item category being viewed (e.g., `Category 93427`), placement slot (e.g., "top-of-search").
3.  **Candidate Retrieval:** The Ad Server queries a high-speed cache or index (like an inverted index) to find all *eligible* promoted listings that match the context. This "inventory" could be thousands of listings.
4.  **Auction & Ranking:** This is the core logic where the system decides which ads to show. The logic differs by ad type:
    * **Promoted Listings Advanced (PLA):** A classic **real-time bidding (RTB)** auction.
        * `Ad Rank = Max CPC Bid * Quality Score`
        * **Quality Score** is a machine-learning model's prediction of the ad's relevance and predicted Click-Through-Rate (pCTR).
        * The winner pays using a model like GSP (Generalized Second-Price), often paying $0.01 more than the next-highest bidder.
    * **Promoted Listings Standard (PLS):** Not a simple CPC auction. It's a relevance-based ranking where the ad rate is a key factor.
        * `Ad Rank = Ad Rate % * Relevance Score * pCTR * Item Price (or other factors)`
        * The system prioritizes ads that are highly relevant, likely to be clicked, and will provide a good return (via the ad rate) *if* they sell.
5.  **Ad Selection:** The top-ranked ads from the auction are selected to fill the available ad slots.
6.  **Ad Response:** The Ad Server sends a JSON response to the client's browser containing the ad data (listing ID, title, image URL, tracking URLs).
7.  **Client-Side Render:** The browser's JavaScript renders the ads into the page.

---

## 3. Core System Components

### Campaign Management Service
* **Purpose:** Provides CRUD (Create, Read, Update, Delete) operations for ad campaigns.
* **Technology:**
    * A set of **microservices** (e.g., `CampaignService`, `BiddingService`, `BudgetService`).
    * **REST APIs** for the seller-facing UI.
    * **Persistent Database:** A relational database (e.g., PostgreSQL, MySQL) or a consistent NoSQL DB (e.g., Google Spanner) to store campaign data, budgets, ad rates, and keyword bids.

### Ad Server
* **Purpose:** To handle the real-time ad request (Steps 2-6 above) with very low latency (e.g., < 100ms).
* **Technology:**
    * **High-Performance Backend:** Written in a high-concurrency language like **Java**, **Scala**, or **Go**.
    * **In-Memory Caching:** Heavy use of **Redis** or **Memcached** to store user profiles, active campaign data, and ML model features for fast lookups.
    * **Machine Learning (ML) Serving:** Integrates with an ML model serving platform to get real-time predictions (like pCTR) for ranking.

### Data & Event Pipeline
* **Purpose:** To reliably collect and process the billions of ad-related events generated daily.
* **Technology:**
    1.  **Event Ingestion:** All frontend events (`impression`, `click`) are fired to a message bus.
        * **Stack:** **Apache Kafka** or **Google Pub/Sub** is the industry standard for this.
    2.  **Stream Processing:** Events are processed in near real-time for immediate dashboard updates.
        * **Stack:** **Apache Flink** or **Apache Spark Streaming** consumes from Kafka, aggregates data (e.g., "clicks per hour"), and writes to an analytics DB.
    3.  **Batch Processing (ETL):** Larger, more complex jobs (e.g., daily billing, attribution) run overnight.
        * **Stack:** **Apache Spark** (Batch) jobs read data from a data lake (e.g., HDFS, S3), perform complex joins and logic, and write to the final data warehouse.

### Tracking & Attribution
* **Purpose:** To accurately connect ad clicks to eventual sales.
* **Technology & Logic:**
    * **Click Tracking:** When an ad is clicked, the user is first sent to a **Tracking URL**. This service logs the `click_id`, `user_id`, and `listing_id` to the Kafka pipeline in milliseconds, then issues an HTTP 302 redirect to the actual item page.
    * **Conversion Tracking:** When a user completes a purchase, an internal "order" event is generated.
    * **Attribution Logic:** A batch (or stream) processing job joins the `click` events with the `order` events.
        * **The Rule:** `IF order.user_id == click.user_id AND order.listing_id == click.listing_id AND order.timestamp <= click.timestamp + 30_days THEN attribute_sale_to_ad`.
        * This is a **30-day last-click attribution model**.

### Analytics & Reporting Database
* **Purpose:** A specialized database optimized for fast analytical queries (OLAP) on massive datasets. This powers the seller's performance dashboard.
* **Technology:**
    * **Stack:** Not a standard relational DB. It's a columnar database like **Apache Druid**, **ClickHouse**, or a cloud data warehouse like **BigQuery** or **Snowflake**. These are built for high-speed aggregation queries (e.g., `GROUP BY day, campaign_id`).

---

## 4. Illustrative Technology Stack (Summary)

| Component | Technology | Purpose |
| :--- | :--- | :--- |
| **Backend Services** | Java, Scala, Go | High-concurrency, low-latency microservices. |
| **Event Ingestion** | Apache Kafka | Collecting billions of `impression`/`click`/`sale` events. |
| **Stream Processing**| Apache Flink, Spark Streaming | Real-time dashboards and data aggregation. |
| **Batch Processing** | Apache Spark | Nightly billing, attribution, and ML model training. |
| **Data Lake** | HDFS, Google Cloud Storage, AWS S3 | Cheap, scalable storage for raw event logs. |
| **Analytics DB** | Apache Druid, ClickHouse, BigQuery | Powering fast dashboards (OLAP). |
| **Caching** | Redis, Memcached | Storing user profiles, campaign data for ad server. |
| **ML Serving** | TensorFlow Serving, Internal Platforms | Serving real-time pCTR/Relevance models. |