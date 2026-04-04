# ⚡ 速習 - データベースサービス

> **完了時間**: 60-75 分 | **試験配点**: ~15-20%

## 🎯 必須理解コンセプト（5分）

### データベースサービス選択（RANDI-NERD）
```
RELATIONAL SQL? → RDS (managed RDBMS)
NOSQL KEY-VALUE? → DynamoDB
CACHE? → ElastiCache (Redis/Memcached)
DATA WAREHOUSE? → Redshift
GRAPH? → Neptune
IN-MEMORY? → ElastiCache
TIME-SERIES? → Timestream
LEDGER? → QLDB
DOCUMENT? → DocumentDB (MongoDB compatible)
```

**記憶法**: 「RDS は Relational、Dynamo は Dynamic data」

## 📊 クイックリファレンステーブル

### RDS データベースエンジン
| Engine | Type | 最大 Storage | 用途 |
|--------|------|-------------|----------|
| **MySQL** | Open source | 64 TiB | Web アプリ、汎用 |
| **PostgreSQL** | Open source | 64 TiB | 高度な機能 |
| **MariaDB** | Open source | 64 TiB | MySQL フォーク |
| **Oracle** | Commercial | 64 TiB | エンタープライズアプリ |
| **SQL Server** | Commercial | 16 TiB | Microsoft エコシステム |
| **Aurora** | AWS proprietary | 128 TiB | 高パフォーマンス |

### RDS vs Aurora vs DynamoDB
| Feature | RDS | Aurora | DynamoDB |
|---------|-----|--------|----------|
| **Type** | SQL | SQL | なしSQL |
| **AZ** | Single/Multi | Multi (デフォルト) | Multi (デフォルト) |
| **Read Replicas** | Up to 5 | Up to 15 | N/A |
| **Scaling** | Vertical | Vertical + Horizontal | Horizontal (auto) |
| **Performance** | Standard | 5x MySQL, 3x PostgreSQL | Milliseconds |
| **Maintenance** | 要対応 | 最小限 | なし |
| **Cost** | $$ | $$$ | $ (従量課金) |

## 🔥 試験頻出トピック

### 1. RDS Multi-AZ vs Read Replicas
```
MULTI-AZ (High Availability)
├── Synchronous replication
├── Automatic failover (< 2分)
├── Same region のみ
├── One DNS name (automatic)
├── Standby NOT readable
└── Use: Disaster Recovery, HA

READ REPLICAS (Read Scaling)
├── Asynchronous replication
├── なし automatic failover
├── Cross-region supported
├── Each has own DNS
├── Replicas ARE readable
└── Use: Read-heavy workloads, reporting
```

**記憶法**: Multi-AZ = 可用性、Read Replica = 読み取り性能

### 2. Aurora の機能（超重要!）
```
AURORA ADVANTAGES
├── 5x faster than MySQL
├── 3x faster than PostgreSQL
├── Up to 15 read replicas
├── Auto-scaling storage (10GB → 128TB)
├── 6 copies across 3 AZs
├── Self-healing storage
├── Automatic failover < 30 seconds
└── Backtrack (point-in-time without restore)

AURORA SERVERLESS
├── Auto-scales compute
├── Pay per second
├── Good for unpredictable workloads
└── Automatic pause when idle
```

### 3. DynamoDB 主要コンセプト
| Concept | Description | 試験のコツ |
|---------|-------------|----------|
| **Partition Key** | Primary key (hash) | 一意な識別子 |
| **Sort Key** | Optional second key | 範囲クエリ |
| **GSI** | グローバル Secondary Index | 非キー属性でクエリ |
| **LSI** | Local Secondary Index | 同じ partition key、別 sort key |
| **Streams** | Change data capture | Lambda をトリガー |
| **DAX** | In-memory cache | マイクロ秒レイテンシ |

**Capacity Modes**:
- **Provisioned**: RCU/WCU を設定（予測可能なら安価）
- **On-Demand**: リクエストごとの従量課金（予測不能トラフィック向け）

### 4. ElastiCache クイック比較
| Feature | Redis | Memcached |
|---------|-------|-----------|
| **Data Types** | Complex (lists, sets) | Simple (strings) |
| **Persistence** | Yes (snapshots) | なし |
| **Replication** | Yes (Multi-AZ) | なし |
| **Backup** | Yes | なし |
| **Multi-threaded** | なし | Yes |
| **用途** | 高機能キャッシュ | シンプルキャッシュ |

**記憶法**: 「Redis = Rich features、Memcached = Memory のみ」

## 💡 よくある試験シナリオ

### Scenario 1: RDS の高可用性
**質問**: DB を AZ 障害から最小ダウンタイムで守りたい
**✅ 正解**: RDS Multi-AZ デプロイメントを有効化（自動フェイルオーバー）

### Scenario 2: 読み取り負荷が高いワークロード
**質問**: DB の読み取りトラフィックが重く、書き込みは通常
**✅ 正解**: RDS Read Replica を追加（通常は最大 5、Aurora は最大 15）

### Scenario 3: グローバル低レイテンシ読み取り
**質問**: 世界中のユーザーが高速に読み取りできる必要がある
**✅ 正解**: DynamoDB グローバル Tables（マルチリージョン、active-active）

### Scenario 4: データベース負荷の削減
**質問**: 同じクエリが繰り返し DB に到達している
**✅ 正解**: DB の前段に ElastiCache（Redis または Memcached）

### Scenario 5: 予測しづらい DB 利用
**質問**: DB 利用量の変動が大きく、時にはアイドルになる
**✅ 正解**: Aurora Serverless（自動スケール、秒単位課金）

### Scenario 6: Point-in-Time Recovery
**質問**: 昨日の特定時刻へ DB を復旧したい
**✅ 正解**: RDS 自動バックアップ（保持 1-35 日）+ 復元

### Scenario 7: なしSQL で一貫した性能
**質問**: あらゆるスケールで 1 桁ミリ秒レイテンシが必要
**✅ 正解**: DynamoDB（フルマネージド なしSQL）

### Scenario 8: 分析用データウェアハウス
**質問**: ペタバイト級データに対して複雑クエリを実行したい
**✅ 正解**: Amazon Redshift（列指向ストレージ、MPP）

## 🎓 速習のコツ

### RDS バックアップの種類
```
AUTOMATED BACKUPS
├── Daily full backup during maintenance window
├── Transaction logs every 5 分
├── Point-in-time recovery (1-35 days retention)
├── Deleted when RDS instance deleted
└── Free storage = DB size

MANUAL SNAPSHOTS
├── User-initiated
├── Kept indefinitely
├── Can copy to another region
├── NOT deleted when RDS instance deleted
└── Charged for storage
```

### DynamoDB の整合性
```
EVENTUALLY CONSISTENT READS (デフォルト)
├── May not reflect recent write
├── Cheaper (1 RCU = 8 KB)
└── Use: Most use cases

STRONGLY CONSISTENT READS
├── Reflects all successful writes
├── More expensive (1 RCU = 4 KB)
└── Use: When latest data required
```

### RDS のスケーリングオプション
```
VERTICAL SCALING
└── Change instance type
└── ダウンタイムが必要（Multi-AZで最小化）
└── Use: More CPU/RAM needed

HORIZONTAL SCALING (Reads)
└── Add Read Replicas
└── なし downtime
└── Use: Read-heavy workloads

STORAGE SCALING
└── Increase storage size
└── なし downtime
└── Auto-scaling available
```

## 📝 ラピッドファイア事実集

### RDS 重要制限
- **最大 Read Replicas**: 5（通常の RDS）、15（Aurora）
- **バックアップ保持**: 0-35 日（0 = 無効）
- **Multi-AZ**: 自動フェイルオーバー < 2 分
- **暗号化**: 既存 DB は暗号化不可（新規作成が必要）
- **インスタンスタイプ**: db.t3、db.m5、db.r5 など

### DynamoDB パフォーマンス
- **1 桁ミリ秒** レイテンシ
- **DAX**: マイクロ秒レイテンシ（インメモリキャッシュ）
- **Partition Key**: 高カーディナリティ（一意値が多い）を選ぶ
- **Hot Partitions**: 回避 → アクセスを均等分散

### Aurora 料金要素
1. **Storage**: $0.10/GB-month（自動スケール）
2. **I/O Requests**: $0.20/million requests
3. **Backups**: DB サイズまで無料
4. **Data Transfer**: 標準 AWS 料金

### Redshift クイックファクト
- **Type**: データウェアハウス（OLAP、OLTP ではない）
- **Columnar Storage**: 分析に高速
- **Massively Parallel Processing (MPP)**
- **Petabyte scale**
- **Single-AZ**（Multi-AZ ではない）
- **Snapshots**: S3 へ保存、他リージョンへコピー可能

## 🚀 5分マスターレビュー

### データベース選択ツリー
```
1. What type of data?
   RELATIONAL (SQL) → Continue to 2
   KEY-VALUE (なしSQL) → DynamoDB
   CACHE → ElastiCache
   ANALYTICS → Redshift

2. Need AWS-optimized?
   あり → Aurora (5x MySQL, 3x PostgreSQL)
   NO → RDS (MySQL, PostgreSQL, etc.)

3. For ElastiCache, need persistence?
   あり → Redis
   NO → Memcached

4. For DynamoDB, predictable traffic?
   あり → Provisioned capacity
   NO → On-Demand capacity
```

### RDS ベストプラクティス
✅ 自動バックアップを有効化（1-35 日）
✅ 本番環境では Multi-AZ を使用
✅ 機密データを含む DB を暗号化
✅ 読み取り負荷が高い場合は Read Replica を使用
✅ CloudWatch / Performance Insights で監視
✅ 定期的なメンテナンスウィンドウを設定
✅ IAM DB 認証を使用
✅ 本番環境では削除保護を有効化

### DynamoDB ベストプラクティス
✅ 良い partition key（高カーディナリティ）を選ぶ
✅ 別のクエリパターンには GSI を使用
✅ 変更追跡のため DynamoDB Streams を有効化
✅ 読み取り中心でキャッシュ向きのワークロードには DAX を使用
✅ 重要テーブルでは point-in-time recovery を有効化
✅ 予測不能トラフィックには on-demand を使用
✅ 保存時暗号化を有効化

### 避けるべきよくあるミス
❌ 読み取りスケーリングに Multi-AZ を使う（Read Replica を使う）
❌ 自動バックアップを有効化しない
❌ 不適切な DynamoDB partition key を選ぶ（hot partition）
❌ OLTP に Redshift を使う（Redshift は OLAP 向け）
❌ Aurora が MySQL/PostgreSQL 互換のみであることを忘れる
❌ 繰り返しクエリに ElastiCache を使わない
❌ Multi-AZ の standby は読み取り可能だと思い込む（不可）
❌ 予測不能な DynamoDB トラフィックで provisioned capacity を使う

## 🎯 試験練習スピードラン

**クイック問題**（答えは下）

1. Aurora の最大 Read Replica 数は? __
2. Multi-AZ の standby は読み取り可能? __
3. DynamoDB の整合性モデルは? __
4. 永続化をサポートするキャッシュは? __
5. RDS の最大 backup retention は? __
6. Aurora storage の最大は? __
7. DynamoDB では Redis と DAX のどちらが高速? __
8. Redshift は OLTP と OLAP のどちら向け? __

---

### RDS Encryption
```
ENCRYPTION AT REST
├── KMS encryption
├── Must enable at creation
├── Cannot encrypt existing (create new from snapshot)
└── Encrypts: data, backups, snapshots, replicas

ENCRYPTION IN TRANSIT
├── SSL/TLS certificates
└── Force with rds.force_ssl parameter
```

### データベース移行オプション
| Source | Target | Tool |
|--------|--------|------|
| オンプレ DB | RDS | DMS (Database Migration Service) |
| RDS | RDS | Snapshots または DMS |
| 異なるエンジン | Any | DMS + SCT (Schema Conversion) |
| 大規模データセット | RDS | Snowball + DMS |

### Aurora グローバル Database
- **Regions**: 最大 5 つのセカンダリリージョン
- **Latency**: リージョン間レプリケーション < 1 秒
- **Recovery**: RTO < 1分ute
- **Read replicas**: リージョンごとに 16
- **用途**: グローバルアプリケーション、DR

## ⏱️ 次のステップ
- 学習時間: ~60-75分
- 演習: RDS インスタンスと DynamoDB テーブルを作成
- 準備完了: Database practice questions
- 次へ: Module 06 - Networking

---

**クイック解答**:
1) 15
2) なし（failover 時のみ）
3) Eventually consistent と Strongly consistent
4) Redis（Memcached は非対応）
5) 35 days
6) 128 TiB
7) DAX（マイクロ秒 vs ミリ秒）
8) OLAP（分析向け、トランザクション向けではない）
