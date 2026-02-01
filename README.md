# AWS Route 53 - Global Traffic Management

A comprehensive deep dive into Amazon Route 53, implementing all 7 routing policies for high availability, disaster recovery, and performance optimization across multiple AWS Regions.

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

### 1.Simple Routing Policy (The Foundation)
**Scenario:** Mapping the custom domain `www.lwinlmn.com` to a single EC2 Web Server.

<details>
<summary>Click here to view Configuration & Results</summary>

| Step | Description | Screenshot |
|---|---|---|
| **1. Setup** | Created Public Hosted Zone | ![Zone](images/simple-routing-hosted-zone-config.png) |
| **2. Config** | Pointed A Record to EC2 IP | ![Config](images/simple-routing-record-config.png) |
| **3. Result** | Successful Connection | ![Success](images/simple-routing-config.png) |

</details>

---

 ### 2.Weighted Routing Policy (Traffic Shaping)
Scenario: Implementing a Canary Deployment or A/B Testing by distributing traffic to different servers based on assigned weights (percentages).

シナリオ: 特定の重み（パーセンテージ）に基づいてトラフィックを異なるサーバーに分散させ、カナリアリリースやA/Bテストを実装します。

<details>
<summary>Click here to view Configuration & Results / 設定と結果を表示するにはここをクリック</summary>

| Step / ステップ | Description / 説明 | Screenshot / スクリーンショット |
|:---:|---|:---:|
| **1. Config** | Assigned weights (100, 50, 20) to three different server IPs in the Route 53 console. / Route 53コンソールで、3つの異なるサーバーIPに重み（100, 50, 20）を割り当てました。 | ![Config](images/weighted-routing-record.png) |
| **2. Result** | Verified via `nslookup` that the domain resolves to the IP with the highest weight (`18.223.171.99`). / `nslookup` を使用して、最も重い値（100）を持つIPアドレスにドメインが解決されることを確認しました。 | ![Success](images/weighted-routing-dns-verification-nslookup.png) |

</details>
---

### 3.Failover Routing Policy (Disaster Recovery) / フェイルオーバールーティングポリシー

**Scenario:** Implementing an **Active-Passive Disaster Recovery** setup. Traffic is directed to the **Primary Server (Ohio)** while it is healthy. If the primary region fails, Route 53 automatically detects the failure and redirects users to the **Secondary Server (Mumbai)**.

**シナリオ:** **アクティブ/パッシブ構成のディザスタリカバリ**を実装します。通常、トラフィックは**メインサーバー（オハイオ）**に送られますが、メインリージョンに障害が発生した場合、Route 53が自動的に検知し、**バックアップサーバー（ムンバイ）**にリダイレクトします。

<details>
<summary>Click here to view Configuration & Results / 設定と結果を表示するにはここをクリック</summary>

| Step / ステップ | Description / 説明 | Screenshot / スクリーンショット |
|:---:|---|:---:|
| **1. Primary Setup** | Primary instances are running in the Ohio region. / オハイオリージョンでメインインスタンスが稼働中。 | ![Primary Setup](images/Failover_Primary_Running.png) |
| **2. Health Check** | Monitoring the primary endpoint. Failure is detected when the server stops. / メインエンドポイントを監視。停止時に「Unhealthy（異常）」を検知。 | ![Health Check](images/Failover_Health_Check_Unhealthy.png) |
| **3. Failover Action** | Traffic is automatically routed to the **Secondary Server in Mumbai**. / トラフィックが自動的に**ムンバイのバックアップサーバー**へ転送。 | ![Secondary Result](images/Failover_Secondary_Display.png) |
| **4. Recovery** | Once the primary server is restored, traffic returns to the **Primary Server in Ohio**. / メインサーバー復旧後、トラフィックは自動的に**オハイオ**に戻ります。 | ![Recovery Result](images/Failover_Primary_Restored.png) |

</details>
---

### 4.Latency Routing Policy / レイテンシーベースルーティングポリシー

**Scenario:** Optimizing global performance by directing traffic to the AWS region that provides the lowest network latency for the user.

**シナリオ:** ユーザーに対して最もネットワーク遅延（レイテンシー）が少ないAWSリージョンにトラフィックをルーティングし、グローバルなパフォーマンスを最適化します。

<details>
<summary>Click here to view Configuration & Results / 設定と結果を表示するにはここをクリック</summary>

| Step / ステップ | Description / 説明 | Screenshot / スクリーンショット |
|:---:|---|:---:|
| **1. Overview** | Configured Latency records for Mumbai and North Virginia in the Hosted Zone. / ホストゾーンでムンバイとバージニア北部用のレイテンシーレコードを設定しました。 | [![Hosted Zone Overview](images/latency-hosted-zone-overview.png) |
| **2. Mumbai Config** | Routing traffic to the Asia Pacific (Mumbai) region for users in Asia. / アジアのユーザー向けに、アジアパシフィック（ムンバイ）リージョンへトラフィックを誘導します。 | [![Mumbai Config](images/latency-config-mumbai.png) |
| **3. Virginia Config** | Routing traffic to the US East (N. Virginia) region for North American users. / 北米のユーザー向けに、米国東部（バージニア北部）リージョンへトラフィックを誘導します。 | [![Virginia Config](images/latency-config-virginia.png) |

</details>
---

### 5.Multivalue Answer Routing Policy / 複数値回答ルーティングポリシー

**Scenario:** Improving availability by returning up to eight healthy records in response to DNS queries. This allows the client to choose an alternative IP if one becomes unreachable.

**シナリオ:** DNSクエリに対して最大8つの正常なレコードを返すことで可用性を向上させます。これにより、1つのIPが利用不能になった場合にクライアントが別のIPを選択できるようになります。

<details>
<summary>Click here to view Configuration & Results / 設定と結果を表示するにはここをクリック</summary>

| Step / ステップ | Description / 説明 | Screenshot / スクリーンショット |
|:---:|---|:---:|
| **1. Config** | Three A records configured with Multivalue Answer routing and associated health checks. / 複数値回答ルーティングと関連するヘルスチェックを設定した3つのAレコード。 | [![Config](images/multivalue-hosted-zone.png) |
| **2. Verification** | `nslookup` returns multiple IP addresses (`18.223.171.99`, `52.66.248.11`, `13.229.237.120`) in a single response. / `nslookup` により、1回のレスポンスで複数のIPアドレスが返されることを確認。 | [![Verification](images/multivalue-nslookup-results.png) |
| **3. Health Checks** | Route 53 monitors all endpoints to ensure only healthy records are returned to users. / Route 53が全エンドポイントを監視し、正常なレコードのみをユーザーに返すようにします。 | [![Health Checks](images/multivalue-health-checks.png) |

</details>

---

### 6.IP-based Routing Policy / IPベースルーティングポリシー

**Scenario:** Routing traffic based on the specific CIDR block of the user. This allows fine-grained control over which server a user reaches based on their source IP address.
**シナリオ:** ユーザーの特定のCIDRブロックに基づいてトラフィックをルーティングします。これにより、送信元IPアドレスに基づいてユーザーがアクセスするサーバーを詳細に制御できます。



<details>
<summary>Click here to view Configuration & Results / 設定と結果を表示するにはここをクリック</summary>

| Step / ステップ | Description / 説明 | Screenshot / スクリーンショット |
|:---:|---|:---:|
| **1. CIDR Setup** | Defined CIDR collections for Location 1, 2, and 3. / ロケーション1、2、3のCIDRコレクションを定義しました。 | [![CIDR Locations](images/ip-based-cidr-locations.png) |
| **2. Config** | Assigned A records to the defined CIDR locations in Route 53. / Route 53で定義されたCIDRロケーションにAレコードを割り当てました。 | [![Hosted Zone](images/ip-based-hosted-zone.png)|
| **3. Verification (Region A)** | Testing from Ohio (Location 1) resolves to server `1.1.1.1`. / オハイオ（ロケーション1）からのテストにより、サーバー`1.1.1.1`に解決。 | [![Ohio Result](images/ip-based-verification-ohio.png) |
| **4. Verification (Region C)** | Testing from Mumbai (Location 2) resolves to server `2.2.2.2`. / ムンバイ（ロケーション2）からのテストにより、サーバー`2.2.2.2`に解決。 | [![Mumbai Result](images/ip-based-verification-mumbai.png) |
| **5. Verification (Region B)** | Testing from Singapore (Location 3) resolves to server `3.3.3.3`. / シンガポール（ロケーション3）からのテストにより、サーバー`3.3.3.3`に解決。 | [![Singapore Result](images/ip-based-verification-singapore.png) |

</details>

---



