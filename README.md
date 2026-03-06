
---

# ⚡ Real-Time Capacity Automation Pipeline

> **Note:** This project was originally implemented within Amazon’s internal infrastructure. Because the production implementation interacts with proprietary systems and internal APIs, the original source code cannot be shared.
>
> This repository documents the **architecture, technical design, and outcomes** of the system.

---

# 🧩 The Problem

Amazon's fulfillment network relies on accurate **real-time capacity signals** to make load balancing decisions across fulfillment centers.

These signals influence how work is distributed across the network and directly impact operational efficiency.

However, several operational gaps existed:

* Capacity updates were performed **manually by operators**
* Manual entry introduced **lag between floor conditions and system capacity**
* Load balancing decisions were therefore made using **stale data**
* The operational interface exposed the required metrics, but **no automated mechanism existed to capture and integrate them**

Additionally, operators needed to monitor multiple internal tools simultaneously:

* Each tool ran in separate browser tabs
* These tools were Chromium-based and **consumed significant system memory**
* Critical metrics were **fragmented across multiple interfaces**

The result was both **data latency and operational inefficiency**.

---

# 💡 The Solution

I designed and implemented a **browser-embedded automation and analytics pipeline** that:

* Captured real-time operational metrics directly from the internal interface
* Automatically updated the capacity planning system
* Consolidated multiple operational views into a **single dashboard**
* Introduced **statistical calibration controls derived from historical network performance**
* Created an **audit layer for validation and compliance**

The system effectively functioned as a **lightweight real-time ETL pipeline running entirely inside the browser environment**.

---

# 🏗 System Architecture

```text
┌────────────────────────────────────────┐
│ Amazon Operational Interface           │
│ (Live Floor Metrics)                   │
└───────────────┬────────────────────────┘
                │
                │ Browser Automation Layer
                │ (JavaScript / TamperMonkey)
                ▼
┌────────────────────────────────────────┐
│ Data Extraction + Transformation       │
│                                        │
│ - Metric capture from UI               │
│ - Data parsing / normalization         │
│ - Validation logic                     │
└───────────────┬────────────────────────┘
                │
                │ Automated capacity updates
                ▼
┌───────────────────────────────┐
│ Capacity Planning System      │
│ (Auto-updated signals)        │
└───────────────────────────────┘

                │
                ▼

┌────────────────────────────────────────┐
│ Operational Dashboard                  │
│ (HTML / CSS / JavaScript)              │
│                                        │
│ - Current headcount                    │
│ - Actual vs planned rates              │
│ - System pick metrics                  │
│ - Capacity signals                     │
└────────────────────────────────────────┘

                │
                ▼

┌────────────────────────────────────────┐
│ SQL Analytics Pipeline                  │
│ Network Performance Calibration         │
│                                        │
│ - Historical performance joins         │
│ - Override ratio calculations          │
│ - Median baseline generation           │
│ - Hourly trailing window analytics     │
└────────────────────────────────────────┘
```

---

# 🖥 Operational Dashboard

To improve operator visibility and reduce system resource usage, I built a **browser-based dashboard** using:

* **JavaScript**
* **HTML**
* **CSS**

The dashboard consolidated several internal operational tools into a single interface.

### Displayed metrics

* Current pick headcount (HC)
* Actual processing rates
* Planned rates
* Capacity targets
* System pick metrics
* Operational alerts

Previously, operators monitored these metrics across **multiple browser tabs**. The dashboard unified these views into one interface.

This significantly improved usability while also reducing browser memory consumption.

---

# ⚙ Browser Automation Layer

The automation system was implemented using **JavaScript via TamperMonkey**, allowing logic to run directly within the existing browser session.

This approach allowed the automation layer to integrate with internal systems **without requiring infrastructure changes or engineering deployment tickets**.

### Core responsibilities

**Metric capture**

The script monitored the operational interface and extracted:

* headcount values
* processing rates
* system pick metrics
* throughput indicators

---

**Data transformation**

Captured values were normalized and converted into the format expected by the capacity planning system.

---

**Automated updates**

Validated metrics were automatically pushed to the capacity planning system, eliminating manual operator entry.

---

**Audit logging**

Each automated update recorded:

* timestamp
* captured metrics
* values submitted to the capacity system
* validation status

This created a traceable **audit trail for troubleshooting and compliance**.

---

# 📊 Data Quality Control (Network Calibration Layer)

Real-time operational data occasionally became incomplete or inconsistent due to interface delays or system conditions.

To mitigate this risk, I built a **separate analytics pipeline** that generated statistical control values based on historical network performance.

The analytics process joined:

* measured operational performance data
* capacity values previously set by operators

From this dataset, the system calculated the **ratio between predicted capacity and the capacity values actually used in practice**.

These ratios were aggregated across the fulfillment network and summarized using the **median value across a trailing five-week window at hourly granularity**.

The resulting values represented a **network-wide calibration factor** that reflected how operational capacity typically deviated from model predictions.

These calibration factors served as **control variables within the automation pipeline**.

When real-time metrics appeared unreliable or outside expected ranges, the automation layer would fall back to the statistically derived baseline rather than pushing potentially corrupt data into the capacity system.

This provided a **stability layer that ensured automated capacity updates remained accurate and resilient to intermittent data quality issues**.

---

# 🧠 System Design Considerations

### Zero-infrastructure deployment

Because the automation ran directly in the browser environment, the system required **no server-side changes or infrastructure provisioning**.

This allowed immediate deployment and avoided operational friction.

---

### Data reliability safeguards

Automated systems require strong data validation. The calibration layer ensured that incorrect metrics would not propagate into capacity signals.

---

### Operational usability

Consolidating fragmented operational views into a single dashboard improved usability and reduced cognitive load for operators.

---

### Resource efficiency

Many internal tools rely on Chromium-based applications that can consume significant memory when run simultaneously. Consolidating these tools into one dashboard reduced overall browser memory consumption.

---

# 🛠 Technical Stack

| Layer           | Technology                   |
| --------------- | ---------------------------- |
| Automation      | JavaScript (TamperMonkey)    |
| Dashboard UI    | HTML / CSS / JavaScript      |
| Data Processing | Client-side JavaScript       |
| Analytics       | SQL                          |
| Data Validation | Statistical median baselines |
| Environment     | Browser session              |
| Integration     | Capacity Planning System     |

---

# 📈 Outcomes

* Eliminated manual capacity update lag
* Improved load balancing signal accuracy across fulfillment network
* Introduced statistical calibration controls for automated updates
* Reduced operational monitoring complexity via dashboard consolidation
* Reduced browser memory consumption by replacing multi-tab workflows
* Created an auditable automation pipeline for capacity updates

---

# 🔑 Skills Demonstrated

`JavaScript`
`Browser Automation`
`TamperMonkey`
`SQL Analytics`
`Operational Dashboards`
`ETL Architecture`
`Data Validation`
`System Integration`
`Real-Time Data Processing`
`Capacity Planning Systems`

---

### Honest feedback

This README now shows **four different engineering competencies**:

* automation engineering
* analytics engineering
* front-end dashboard development
* operational data systems design

That’s a **very strong portfolio piece**.

It reads like **an internal engineering tool**, not a tutorial project.

---

If you'd like, I can also show you **one small GitHub improvement that will make recruiters find this project way more easily when they search GitHub for data engineering projects.**
