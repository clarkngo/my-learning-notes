---
title: "Sherlock IO"
layout: default
parent: Ebay
---
# Sherlock.io

## 1. One Size Fits All Fails for Anomaly Detection

My first major learning is that you cannot apply a single algorithm to all metrics. The platform's success hinges on its ability to treat different types of metrics differently.

* **Business Metrics:** Metrics like "checkouts per minute" have strong weekly seasonality. A simple spike-detection algorithm would fail. The platform uses a custom algorithm (called "Mediff") designed to understand these seasonal patterns and not be fooled by old anomalies or data gaps.
* **Technical Metrics:** Metrics like "load balancer connection number" or "application error rates" behave differently. For these, we care about sudden spikes. The platform uses a different model based on Extreme Value Theory ("spot") to identify these spikes as the real anomalies.

**My takeaway:** When setting up monitoring, the first question must be "What is the *pattern* of this metric?" before "What is the *threshold*?"

---

## 2. A Common Language (DSL) Is a Massive Accelerator

Sherlock.io's best design choice was aligning with the Prometheus (PromQL) tech stack.

As end-users, we were already using **`promql`** to build our dashboards and set up simple, traditional threshold alerts. By using this *same* `promql` DSL for anomaly detection, the platform dramatically lowered the barrier to entry.

* I can use the same query I use on my dashboard to create a sophisticated anomaly detection job.
* I can create "dynamic" jobs (e.g., `sum by (app)`...) that automatically pick up new applications or services without me having to manually create a new alert for each one.

**My takeaway:** A unified query language across dashboarding, alerting, and advanced ML monitoring is a huge win. It reduces cognitive load and speeds up adoption.

---

## 3. Real-Time Detection Doesn't Mean Real-Time Training

This was the biggest "under-the-hood" learning. I always wondered how the system could check a seasonal metric every minute against months of historical data without collapsing.

The answer is a **two-phase system**:

1.  **Training Phase:** A separate, heavier job runs infrequently (e.g., every 2 hours) and looks at a large chunk of historical data (e.g., 24 hours) to build or update the core model (e.g., calculate the thresholds using T-digest).
2.  **Inference Phase:** The "real-time" alert job runs every minute but only queries a very small window of recent data (e.g., the last 2 hours). It compares these new data points against the model built by the training phase.

**My takeaway:** This "train heavy, infer light" architecture is the key to building a scalable, low-latency detection system that can handle massive data volumes without constant query pressure on the database.

---

## 4. Raw ML Anomalies Are Noisy. Context Is King.

Initially, the pure ML detection for business metrics was only "meh" (around a 0.5 F1-score). This is because an algorithm, by itself, lacks human domain knowledge.

The platform became truly powerful when it allowed us to **correlate anomalies**. We could encode our own SRE domain knowledge into the rules.

* **Example:** A "dip in checkouts" anomaly by itself might be a false positive. But a "dip in checkouts" anomaly *at the same time* as a "spike in checkout application errors" is a high-confidence, critical alert.

**My takeaway:** The best alerting system isn't just ML; it's **ML + human-defined rules**. The system's job is to find the "unknown unknowns" (anomalies) and let us correlate them to create "known patterns" (actionable alerts).

---

## 5. The System Must Evolve to Find New Problems

A monitoring platform is never "done." As soon as we got good at finding spikes and seasonal dips, a new problem type emerged: the **"slow bleeding" issue**.

This is when a metric (like site latency or conversion) degrades *very slightly*—too small for a spike or seasonal detector to notice—but it does so consistently over a long period. The platform had to evolve and add a new algorithm ("adaptive cusum") specifically to catch this new class of problem.

**My takeaway:** A monitoring platform must be extensible. It needs a generalized infrastructure that allows new algorithms to be plugged in as new, subtle types of failures are discovered.

---

## 6. The End Goal Is Self-Service

The ultimate goal, which Sherlock.io moved toward, is **self-service monitoring**. Instead of me having to file a ticket with a data science team, the platform gives me the tools.

It provides a "menu" of algorithm types:
* Want to find spikes? Use `spike_detection`.
* Want to find seasonal issues? Use `seasonality_detection`.
* Want to compare a metric across datacenters? Use `colo-colo_detection`.

**My takeaway:** The most powerful platform empowers its end-users. It abstracts the deep ML complexity away and just asks, "What kind of problem are you trying to find?"