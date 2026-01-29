# AWS Route 53 - Global Traffic Management

A comprehensive deep dive into Amazon Route 53, implementing all 8 routing policies for high availability, disaster recovery, and performance optimization across multiple AWS Regions.

---

Architecture Design
*(This section will feature 3 distinct architecture diagrams covering Global Traffic Flow, Disaster Recovery, and Traffic Shaping.)*

### 1. Global Performance Architecture (Latency & Geo)
> *[Diagram Coming Soon - Logic for US, Mumbai, Singapore]*

### 2. Disaster Recovery Architecture (Failover)
> *[Diagram Coming Soon - Active-Passive Setup]*

### 3. Traffic Logic Flow (Weighted & IP-based)
> *[Diagram Coming Soon - Flowchart]*

---

## Routing Policies Implementation

### 1️⃣ Simple Routing Policy (The Foundation)
**Scenario:** Mapping the custom domain `www.lwinlmn.com` to a single EC2 Web Server.

<details>
<summary>Click here to view Configuration & Results</summary>

| Step | Description | Screenshot |
|---|---|---|
| **1. Setup** | Created Public Hosted Zone | ![Zone](images/SimpleRoutinghosted-zone-config.png) |
| **2. Config** | Pointed A Record to EC2 IP | ![Config](images/SimpleRoutingRecord-config.png) |
| **3. Result** | Successful Connection | ![Success](images/simple-routing-config.png) |

</details>

---

### 2️⃣ Weighted Routing Policy (Traffic Shaping)
**Scenario:** Distributing traffic (e.g., 80% to Production, 20% to Beta) for A/B testing.
> *Implementation coming next week...*

---

### 3️⃣ Failover Routing Policy (Disaster Recovery)
**Scenario:** Automatic failover to a backup S3 site when the primary server fails health checks.
> *Implementation coming next week...*

---

### 4️⃣ Latency & Geolocation Routing
**Scenario:** Serving content from the nearest region (Mumbai/Singapore) and restricting content based on country.
> *Implementation coming next week...*

---

## 🛠️ Tech Stack
* **AWS Services:** Route 53, EC2, S3, VPC, CloudWatch (Health Checks)
* **Web Server:** Apache (HTTPD) on Ubuntu
* **Tools:** Draw.io (for Architecture Diagrams)
