# データベースサービス - 練習問題

> **⚠️ 免責事項:** これらは AWS ドキュメントに基づいて教育目的で作成した**オリジナル練習問題**です。AWS 認定試験の**実際の試験問題ではありません**。

## 試験標準問題（SAA-C03）

---

### 問題 1
ある企業は、自動スケーリング・複数 AZ にまたがる高可用性・自動フェイルオーバーを備えたリレーショナルデータベースを必要としています。データベース運用は最小限にしたい場合、どのサービスを使うべきですか？

A. Amazon RDS MySQL
B. Amazon Aurora
C. Amazon DynamoDB
D. Amazon Redshift

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- **Amazon Aurora** のポイント:
  - MySQL/PostgreSQL compatibility
  - 5x performance of MySQL, 3x of PostgreSQL
  - 自動スケーリング (storage up to 128 TB)
  - Multi-AZ by design (6 copies across 3 AZs)
  - 自動フェイルオーバー (< 30 seconds)
  - Serverless オプションあり

**Aurora vs RDS**:
- **Aurora**: Better performance, auto-scaling storage, faster replication
- **RDS**: Standard MySQL/PostgreSQL, simpler

**Aurora 機能**:
- Up to 15 read replicas
- S3 への継続バックアップ
- ポイントインタイムリカバリ
- Backtrack (rewind database)

**参照:** Amazon Aurora, RDS vs Aurora
</details>

---

### 問題 2
アプリケーションで読み書きにサブミリ秒レイテンシが必要で、毎秒数百万リクエストに対応するため自動スケーリングも必要です。どのデータベースを使うべきですか？

A. Amazon RDS
B. Amazon DynamoDB
C. Amazon Aurora
D. Amazon DocumentDB

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- **Amazon DynamoDB** はフルマネージドの NoSQL データベースです
  - 1 桁ミリ秒レイテンシ
  - 1 日 10 兆超リクエストまでスケール
  - 自動スケーリング
  - マルチ AZ レプリケーションを標準搭載
  - Serverless オプション（オンデマンド課金）

**DynamoDB 機能**:
- Key-value and document database
- DynamoDB Streams (change data capture)
- Global Tables (multi-region replication)
- DynamoDB Accelerator (DAX) for microsecond latency
- ポイントインタイムリカバリ
- On-demand and provisioned capacity modes

**利用する場面 DynamoDB**:
- Need extreme scale
- Variable workloads
- Need sub-10ms latency
- Key-value or document data model

**参照:** Amazon DynamoDB, NoSQL Databases
</details>

---

### 問題 3
企業はデータベースクエリ結果をキャッシュして RDS の読み取り負荷を下げ、応答時間を改善したいと考えています。どのサービスを使うべきですか？

A. Amazon CloudFront
B. Amazon ElastiCache
C. DynamoDB Accelerator (DAX)
D. Amazon S3

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- **Amazon ElastiCache** provides in-memory caching
  - Redis or Memcached engines
  - Microsecond latency
  - データベース負荷を軽減
  - セッションストレージ
  - リーダーボード、リアルタイム分析

**ElastiCache Engines**:

| Feature | Redis | Memcached |
|---------|-------|-----------|
| **Data Types** | Advanced (strings, sets, sorted sets) | Simple (strings) |
| **Persistence** | Yes (snapshots) | No |
| **Replication** | Yes (Multi-AZ) | No |
| **Pub/Sub** | Yes | No |
| **Sorted Sets** | Yes (leaderboards) | No |
| **Multi-threaded** | No | Yes |

**一般的なパターン**: Application → ElastiCache → RDS (cache-aside)

**参照:** Amazon ElastiCache, Caching Strategies
</details>

---

### 問題 4
データウェアハウスで、ペタバイト規模データに対する複雑な分析クエリが必要です。最も適切な AWS データベースサービスはどれですか？

A. Amazon RDS
B. Amazon DynamoDB
C. Amazon Redshift
D. Amazon Aurora

<details>
<summary>解答を表示</summary>

**解答: C**

**解説:**
- **Amazon Redshift** はデータウェアハウスサービスです
  - 列指向ストレージ
  - 超並列処理（MPP）
  - Petabyte-scale
  - SQL queries (PostgreSQL-compatible)
  - Integration with S3, EMR, Glue

**Redshift 機能**:
- 列指向ストレージ (better compression, faster queries)
- Redshift Spectrum (query S3 data directly)
- 自動バックアップ and snapshots
- Encryption at rest and in transit
- Concurrency Scaling (handle burst queries)
- Result caching

**Redshift vs RDS**:
- **Redshift**: Analytics, OLAP, data warehouse
- **RDS**: Transactional, OLTP, operational database

**ユースケース**:
- Business intelligence
- Historical data analysis
- Large-scale analytics
- Reporting

**参照:** Amazon Redshift, Data Warehousing
</details>

---

### 問題 5
MongoDB アプリケーションを最小限の変更で AWS に移行する必要があります。どのサービスを使うべきですか？

A. Amazon RDS
B. Amazon DynamoDB
C. Amazon DocumentDB
D. Amazon Neptune

<details>
<summary>解答を表示</summary>

**解答: C**

**解説:**
- **Amazon DocumentDB** は MongoDB 互換のドキュメントデータベースです
  - フルマネージド
  - MongoDB 3.6 and 4.0 compatibility
  - Scales to millions of requests/second
  - Multi-AZ replication
  - 自動バックアップ

**DocumentDB 機能**:
- Document data model (JSON)
- MongoDB drivers and tools compatible
- ストレージは 64 TB まで自動スケール
- Up to 15 read replicas
- S3 への継続バックアップ

**移行**: Use AWS Database Migration Service (DMS)

**利用する場面**:
- Existing MongoDB workloads
- Document-oriented data
- Need managed service

**参照:** Amazon DocumentDB, MongoDB on AWS
</details>

---

### 問題 6
アプリケーションでデータエンティティ間の関係（SNS のつながり、詐欺検知など）を保存する必要があります。最も適したデータベースはどれですか？

A. Amazon RDS
B. Amazon DynamoDB
C. Amazon Neptune
D. Amazon DocumentDB

<details>
<summary>解答を表示</summary>

**解答: C**

**解説:**
- **Amazon Neptune** はグラフデータベースです
  - Store and query highly connected data
  - Gremlin と SPARQL をサポート
  - 高速な関係性クエリ
  - Multi-AZ replication

**Neptune ユースケース**:
- Social networks
- Fraud detection
- Recommendation engines
- Knowledge graphs
- Network topology
- Supply chain

**Graph vs Relational**:
- **Graph**: Complex relationships, traversals
- **Relational**: Joins become expensive for deep relationships
- Graph databases optimize for relationship queries

**参照:** Amazon Neptune, Graph Databases
</details>

---

### 問題 7
企業は、別 AZ のスタンバイインスタンスへ自動フェイルオーバーする RDS を運用したいと考えています。何を設定すべきですか？

A. RDS Read Replicas
B. RDS Multi-AZ Deployment
C. RDS Snapshots
D. RDS Cross-Region Replication

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- **RDS Multi-AZ** provides 高可用性
  - スタンバイへの同期レプリケーション
  - 自動フェイルオーバー (1-2 minutes)
  - 同一リージョン内の別 AZ
  - 手動介入不要

**Multi-AZ vs Read Replicas**:

| Feature | Multi-AZ | Read Replicas |
|---------|----------|---------------|
| **目的** | High availability | Read scaling |
| **Replication** | 同期 | 非同期 |
| **Failover** | Automatic | 手動昇格 |
| **Access** | スタンバイには直接アクセス不可 | レプリカから読み取り可能 |
| **Region** | Same | Same or cross-region |

**Multi-AZ Failover Triggers**:
- AZ failure
- Instance failure
- Storage failure
- Software patching (planned)

**参照:** RDS Multi-AZ, High Availability
</details>

---

### 問題 8
アプリケーションで DynamoDB を使用しつつ、複数属性を使った複雑なクエリに対応する必要があります。どの機能を使うべきですか？

A. DynamoDB Streams
B. DynamoDB Global Secondary Index (GSI)
C. DynamoDB Auto Scaling
D. DynamoDB Accelerator (DAX)

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- **Global Secondary Index (GSI)**:
  - Query on non-primary key attributes
  - Different partition and sort keys than base table
  - Eventually consistent reads
  - Can be added after table creation

**DynamoDB Index Types**:

| Type | Keys | Projection | Creation |
|------|------|------------|----------|
| **LSI** | Same partition key | Can choose | At table creation only |
| **GSI** | Different keys | Can choose | Anytime |

**GSI ユースケース**:
- Query by different attributes
- Support multiple access patterns
- Flexible querying

**Example**:
- Table: Partition Key = UserID
- GSI: Partition Key = Email
- Can now query by email

**参照:** DynamoDB Indexes, GSI vs LSI
</details>

---

### 問題 9
企業は DynamoDB の読み取りでマイクロ秒レイテンシを実現したいと考えています。どの機能を有効化すべきですか？

A. DynamoDB Streams
B. DynamoDB Global Tables
C. DynamoDB Accelerator (DAX)
D. DynamoDB Auto Scaling

<details>
<summary>解答を表示</summary>

**解答: C**

**解説:**
- **DynamoDB Accelerator (DAX)**:
  - DynamoDB 向けインメモリキャッシュ
  - Microsecond latency (vs millisecond)
  - フルマネージド, highly available
  - アプリコード変更なし（drop-in replacement）

**DAX Benefits**:
- 10x performance improvement for read-heavy workloads
- Reduces read load on DynamoDB
- Eventually consistent reads
- Write-through cache

**DAX vs ElastiCache**:
- **DAX**: DynamoDB-specific, easier integration
- **ElastiCache**: General purpose, more control

**利用する場面 DAX**:
- Read-heavy workloads
- Need microsecond latency
- Repeated reads of same items
- Real-time bidding, gaming

**参照:** DynamoDB Accelerator (DAX), Caching
</details>

---

### 問題 10
RDS データベースで読み取りトラフィックが増加しています。書き込みトラフィックは低いです。最も費用対効果の高い解決策はどれですか？

A. Upgrade to larger instance
B. Enable RDS Multi-AZ
C. Create RDS Read Replicas
D. Use ElastiCache

<details>
<summary>解答を表示</summary>

**解答: C**

**解説:**
- **RDS Read Replicas**:
  - 非同期 replication
  - Offload read traffic from primary
  - Up to 15 replicas (Aurora)
  - Same region or cross-region
  - Can be promoted to standalone

**Read Replica ユースケース**:
- Read-heavy workloads
- Reporting/analytics (separate from production)
- Cross-region disaster recovery (promotion)
- Read scaling

**Benefits**:
- Cost-effective scaling for reads
- No impact on primary instance
- Can use different instance types

**Replication Lag**: Monitor with CloudWatch metric

**参照:** RDS Read Replicas, Read Scaling
</details>

---

### 問題 11
グローバルアプリケーションで、複数 AWS リージョンに DynamoDB テーブルをレプリケーションし、各リージョンで低レイテンシの読み書きを実現する必要があります。どの機能を使うべきですか？

A. DynamoDB Streams
B. DynamoDB Global Tables
C. DynamoDB Cross-Region Replication
D. AWS Database Migration Service

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- **DynamoDB Global Tables**:
  - Multi-region, multi-active replication
  - Bi-directional replication
  - Local read/write in each region
  - Sub-second latency
  - Conflict resolution (last writer wins)

**Global Tables Requirements**:
- DynamoDB Streams enabled
- Same table name across regions
- Same primary key structure

**ユースケース**:
- Global applications
- Disaster recovery
- Multi-region 高可用性
- Reduce latency for global users

**参照:** DynamoDB Global Tables, Multi-Region
</details>

---

### 問題 12
企業はコンプライアンスのため RDS バックアップを 90 日保持する必要があります。何を設定すべきですか？

A. Automated Backups with 90-day 保持期間
B. Manual Snapshots
C. Both automated backups (35 days) and manual snapshots
D. RDS Cross-Region Replication

<details>
<summary>解答を表示</summary>

**解答: C**

**解説:**
- **RDS Automated Backups**: Max 35 days 保持期間
- **Manual Snapshots**: Retained until explicitly deleted
- For > 35 days: Use manual snapshots

**RDS Backup Types**:

| Type | Retention | Deleted with DB | Use Case |
|------|-----------|-----------------|----------|
| **Automated** | 1-35 days | Yes | ポイントインタイムリカバリ |
| **Manual** | Until deleted | No | Long-term 保持期間 |

**ベストプラクティス for Long Retention**:
1. Enable automated backups (35 days)
2. Create manual snapshots for long-term
3. Use lifecycle policies to copy to S3/Glacier

**Automated Backup 機能**:
- Daily full snapshot
- Transaction logs every 5 minutes
- ポイントインタイムリカバリ within 保持期間 period

**参照:** RDS Backups, Backup Retention
</details>

---

### 問題 13
IoT センサーデータ向けに、自動データ保持ポリシー付きの時系列データ保存が必要です。最も適したデータベースはどれですか？

A. Amazon DynamoDB
B. Amazon RDS
C. Amazon Timestream
D. Amazon Redshift

<details>
<summary>解答を表示</summary>

**解答: C**

**解説:**
- **Amazon Timestream** is purpose-built for time-series data
  - IoT, DevOps, analytics
  - Automatic data lifecycle management
  - Built-in time-series analytics
  - 1000x faster, 1/10th cost vs relational

**Timestream 機能**:
- Automatic tiering (memory → magnetic)
- Built-in time-series functions
- Serverless, auto-scaling
- SQL queries

**ユースケース**:
- IoT sensor data
- Application monitoring
- DevOps metrics
- Industrial telemetry

**Timestream vs Alternatives**:
- **DynamoDB**: General NoSQL, manual TTL
- **Redshift**: Data warehouse, not optimized for time-series
- **Timestream**: 目的-built, best for time-series

**参照:** Amazon Timestream, Time-Series Databases
</details>

---

### 問題 14
企業はオンプレミス Oracle データベースを最小ダウンタイムで AWS へ移行したいと考えています。どのサービスを使うべきですか？

A. AWS Database Migration Service (DMS)
B. AWS DataSync
C. AWS Snowball
D. Manual export/import

<details>
<summary>解答を表示</summary>

**解答: A**

**解説:**
- **AWS DMS** (Database Migration Service):
  - Migrate databases with minimal downtime
  - Source database remains operational
  - Supports homogeneous and heterogeneous migrations
  - Continuous data replication

**DMS Migration Types**:
1. **Homogeneous**: Oracle → Oracle, MySQL → Aurora MySQL
2. **Heterogeneous**: Oracle → Aurora PostgreSQL (use SCT)

**DMS 機能**:
- Continuous replication
- Multi-AZ for 高可用性
- 自動フェイルオーバー
- Supports many database engines

**Migration Steps**:
1. Create replication instance
2. Configure source and target endpoints
3. Create migration task
4. Full load + CDC (Change Data Capture)
5. Cutover when ready

**参照:** AWS Database Migration Service, Database Migration
</details>

---

### 問題 15
アプリケーションは RDS MySQL を使用しています。既存の未暗号化データベースを暗号化したい場合、正しい手順はどれですか？

A. Enable encryption on the existing database
B. Create encrypted snapshot, restore to new encrypted instance
C. Use AWS KMS to encrypt in-place
D. Export data and re-import to encrypted instance

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- **Cannot encrypt existing RDS instance directly**
- Must create encrypted copy

**Encryption Process**:
1. Create snapshot of unencrypted DB
2. Copy snapshot with encryption enabled
3. Restore encrypted snapshot to new DB instance
4. Update application endpoint
5. Delete old instance

**RDS Encryption**:
- Uses AWS KMS for key management
- Encrypts data at rest (storage, backups, snapshots, read replicas)
- Cannot disable encryption once enabled
- Minimal performance impact

**Alternative**: Use AWS DMS to migrate with encryption enabled

**参照:** RDS Encryption, Encrypting Existing Databases
</details>

---

### 問題 16
DynamoDB テーブルのトラフィックが日中に大きく変動します。コスト最適化のため、どのキャパシティモードを使うべきですか？

A. Provisioned Capacity
B. On-Demand Capacity
C. Reserved Capacity
D. Auto Scaling

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- **On-Demand Capacity**:
  - リクエスト課金
  - キャパシティ計画不要
  - 自動スケール
  - Ideal for unpredictable/変動ワークロード

**Capacity Modes 比較**:

| Mode | Best For | Pricing | Scaling |
|------|----------|---------|---------|
| **On-Demand** | Variable traffic | Per request | Automatic |
| **Provisioned** | Predictable traffic | Per hour (RCU/WCU) | Manual or Auto Scaling |

**When to Use**:
- **On-Demand**: New tables, unpredictable, spiky traffic
- **Provisioned**: Predictable, steady traffic, cost optimization

**Cost**: On-Demand can be more expensive for consistent workloads

**Can switch between modes** once per 24 hours

**参照:** DynamoDB Capacity Modes, On-Demand vs Provisioned
</details>

---

### 問題 17
アプリケーションで複数の DynamoDB テーブルにまたがる ACID トランザクションが必要です。どの機能を使うべきですか？

A. DynamoDB Streams
B. DynamoDB Transactions
C. DynamoDB Batch Operations
D. DynamoDB Global Tables

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- **DynamoDB Transactions**:
  - ACID properties (Atomicity, Consistency, Isolation, Durability)
  - All-or-nothing operations
  - Up to 100 items or 4 MB per transaction
  - TransactWriteItems and TransactGetItems

**Transaction ユースケース**:
- Financial transactions
- Order processing
- Inventory management
- Need data consistency across items

**Transaction APIs**:
- `TransactWriteItems`: Atomic writes
- `TransactGetItems`: Snapshot reads

**Cost**: 2x the cost of standard reads/writes

**参照:** DynamoDB Transactions, ACID Compliance
</details>

---

### 問題 18
企業は DynamoDB アイテムの変更をリアルタイムで追跡し、マテリアライズドビュー更新やワークフロー起動を行いたいと考えています。どの機能を使うべきですか？

A. DynamoDB Auto Scaling
B. DynamoDB Streams
C. DynamoDB Backups
D. CloudWatch Events

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- **DynamoDB Streams**:
  - アイテム単位変更の順序付き記録
  - ほぼリアルタイム（通常 1 秒未満）
  - 24-hour 保持期間
  - Exactly-once 配信

**Stream View Types**:
- **KEYS_ONLY**: Only key attributes
- **NEW_IMAGE**: Entire item after change
- **OLD_IMAGE**: Entire item before change
- **NEW_AND_OLD_IMAGES**: Both before and after

**ユースケース**:
- Update materialized views
- Trigger Lambda functions
- Cross-region replication (Global Tables)
- Data pipelines
- Analytics

**一般的なパターン**: DynamoDB Streams → Lambda → Process changes

**参照:** DynamoDB Streams, Change Data Capture
</details>

---

### 問題 19
Aurora DB クラスターで、本番トラフィックに影響を与えず分析クエリを処理する必要があります。何を設定すべきですか？

A. Aurora Read Replicas
B. Aurora Serverless
C. Aurora Global Database
D. RDS Read Replicas

<details>
<summary>解答を表示</summary>

**解答: A**

**解説:**
- **Aurora Read Replicas**:
  - Up to 15 replicas
  - Low replication lag (< 10ms)
  - Offload read traffic
  - Can have custom endpoints

**Aurora Custom Endpoints**:
- Direct specific workloads to specific replicas
- Example: Analytics queries to larger instances

**Reader Endpoint**:
- Load balances across all read replicas
- 自動フェイルオーバー if primary fails

**Aurora Auto Scaling**:
- Automatically add/remove replicas based on load

**参照:** Aurora Read Replicas, Aurora Endpoints
</details>

---

### 問題 20
サーバーレスアプリケーションで、ワークロードに応じてキャパシティが自動スケールするデータベースが必要です。最も適した選択肢はどれですか？

A. RDS with Auto Scaling
B. Aurora Serverless
C. DynamoDB On-Demand
D. Both B and C

<details>
<summary>解答を表示</summary>

**解答: D**

**解説:**
Both are suitable for サーバーレスアプリケーション:

**Aurora Serverless**:
- Automatically scales compute capacity
- Pay per second for compute
- Ideal for intermittent/unpredictable workloads
- SQL database (PostgreSQL/MySQL)

**DynamoDB On-Demand**:
- Automatically scales throughput
- リクエスト課金
- NoSQL database
- Sub-10ms latency

**選定基準**:
- **Need SQL, relational** → Aurora Serverless
- **Need NoSQL, extreme scale** → DynamoDB On-Demand
- **Intermittent workloads** → Both work
- **Dev/test environments** → Aurora Serverless
- **Serverless apps** → Both work

**参照:** Aurora Serverless, DynamoDB On-Demand, Serverless Databases
</details>

---

### 問題 21
開発チームは、フルマネージドでスケーラブルかつ高可用な Apache Cassandra 互換データベースを必要としています。どの AWS サービスを使うべきですか？

A. Amazon Keyspaces
B. Amazon DynamoDB
C. Amazon RDS for PostgreSQL
D. Amazon Aurora

<details>
<summary>解答を表示</summary>

**解答: A**

**解説:**
- Amazon Keyspaces is a マネージドな Cassandra 互換データベース
- Supports Cassandra Query Language (CQL)
- DynamoDB is NoSQL but not Cassandra-compatible
- RDS and Aurora are relational databases

**参照:** Amazon Keyspaces, Managed Cassandra
</details>

---

### 問題 22
金融機関が、取引記録のためにフルマネージドで不変かつ暗号学的に検証可能な台帳データベースを必要としています。どの AWS サービスを使うべきですか？

A. Amazon QLDB
B. Amazon Aurora
C. Amazon RDS
D. Amazon DynamoDB

<details>
<summary>解答を表示</summary>

**解答: A**

**解説:**
- Amazon QLDB (Quantum Ledger Database) is a fully managed ledger database
- 不変かつ暗号学的に検証可能なトランザクションログを提供
- Aurora, RDS, and DynamoDB are not ledger databases

**参照:** Amazon QLDB, Ledger Database
</details>

---

## まとめ

**総問題数**: 22
**対象トピック**:
- Amazon RDS (Multi-AZ, Read Replicas, Backups, Encryption)
- Amazon Aurora (機能, Read Replicas, Serverless)
- Amazon DynamoDB (Capacity Modes, Indexes, Streams, Transactions, DAX, Global Tables)
- Amazon ElastiCache (Redis vs Memcached)
- Amazon Redshift (Data Warehousing)
- Amazon DocumentDB (MongoDB-compatible)
- Amazon Neptune (Graph Database)
- Amazon Timestream (Time-Series)
- AWS Database Migration Service (DMS)
- Amazon Keyspaces (Cassandra-compatible)
- Amazon QLDB (Ledger Database)

**試験のコツ**:

**データベース選定**:
- **Relational (OLTP)**: RDS, Aurora
- **NoSQL Key-Value**: DynamoDB
- **NoSQL Document**: DocumentDB
- **Graph**: Neptune
- **Data Warehouse (OLAP)**: Redshift
- **Time-Series**: Timestream
- **In-Memory Cache**: ElastiCache

**RDS vs Aurora**:
- **Aurora**: Better performance, auto-scaling, more expensive
- **RDS**: Standard engines, simpler

**高可用性**:
- **RDS Multi-AZ**: 自動フェイルオーバー, synchronous
- **Read Replicas**: Read scaling, asynchronous

**DynamoDB**:
- **GSI**: Query different attributes, add anytime
- **LSI**: Same partition key, create with table
- **Streams**: Track changes, trigger Lambda
- **Transactions**: ACID compliance
- **DAX**: Microsecond latency cache
- **Global Tables**: Multi-region replication

**ElastiCache**:
- **Redis**: Advanced features, persistence, replication
- **Memcached**: Simple, multi-threaded

**移行**:
- **AWS DMS**: Minimal downtime migration
- **SCT**: Convert schema for heterogeneous migrations

**キャパシティ計画**:
- **Predictable**: Provisioned capacity
- **Variable**: On-Demand or Auto Scaling
- **Intermittent**: Serverless (Aurora Serverless, DynamoDB On-Demand)

**次のステップ**:
- 各データベース種別のユースケースを理解する
- Multi-AZ と Read Replicas の使い分けを理解する
- DynamoDB の機能と使いどころを整理する
- 要件に応じたデータベース選定を練習する
