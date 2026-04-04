# ⚡ Fast Learning - 分析サービス

> **完了時間**: 45〜60分 | **試験配点**: 約8〜12%

## 🎯 必須コンセプト（5分）

### 分析サービス選択（AREKGLQ）
```
S3データをクエリ？ → Athena（サーバーレスSQL）
データウェアハウス？ → Redshift（ペタバイト級）
ETL？ → Glue（サーバーレスETL）
Elasticsearch系？ → OpenSearch（検索・分析）
ビッグデータ？ → EMR（Hadoop, Spark）
リアルタイム？ → Kinesis（ストリーミング分析）
データレイク？ → Lake Formation（データレイク）
可視化を素早く？ → QuickSight（BIダッシュボード）
```

**暗記フレーズ**: 「Athenaで聞く、Redshiftで集約、EMRで実行、Kinesisで流す、Glueでつなぐ、Lakeで守る、QuickSightで見せる」

## 📊 クイック比較表

### 分析サービス早見表
| サービス | 種別 | 主な用途 | クエリ言語 | サーバーレス |
|---------|------|----------|-----------|-------------|
| **Athena** | 対話型クエリ | S3へのアドホックSQL | SQL | ✅ |
| **Redshift** | DWH | OLAP/BI | SQL | ❌ |
| **EMR** | ビッグデータ基盤 | Hadoop/Spark処理 | 各種 | ❌ |
| **Glue** | ETL | データ変換 | Python/Scala | ✅ |
| **OpenSearch** | 検索エンジン | 検索/ログ分析 | Query DSL | ❌ |
| **QuickSight** | BIツール | ダッシュボード可視化 | N/A | ✅ |

### Redshift vs Athena vs RDS
| 項目 | Redshift | Athena | RDS |
|------|----------|--------|-----|
| **目的** | データウェアハウス（OLAP） | S3クエリ | トランザクション（OLTP） |
| **性能** | 非常に高速 | 高速 | 高速 |
| **スケール** | ペタバイト | エクサバイト | テラバイト |
| **コスト** | $$$（プロビジョンド） | $（クエリ課金） | $$ |
| **事前ロード** | 必要 | 不要（その場クエリ） | N/A |
| **クエリ** | SQL | SQL | SQL |

## 🔥 試験頻出トピック

### 1. Amazon Athena
```
概要: S3に対するサーバーレスSQL
料金: スキャンしたTBあたり$5
形式: Parquet, ORC, JSON, CSV, Avro

主要機能:
├── インフラ管理不要
├── クエリごとの従量課金（TBスキャン）
├── Glue Data Catalog連携
├── JDBC/ODBCドライバ対応
├── パーティション対応
└── VPC Flow Logs, CloudTrail, ALBログを直接分析可能

最適化:
├── 列指向形式（Parquet/ORC）を使う（最大90%安く）
├── 圧縮（GZIP, Snappy）
├── パーティション（日時/リージョン等）
├── 1ファイルを大きめに（128MB超）
└── 必要列のみ取得（SELECT *を避ける）

代表パターン:
S3 → Glue Crawler → Glue Catalog → Athena → QuickSight
```

**試験のコツ**: Athenaはスキャン量課金。圧縮＋列指向で大幅節約。

### 2. Amazon Redshift
```
アーキテクチャ:
├── Leader Node（クエリ計画/集約）
└── Compute Nodes（実行/保存）

ノードタイプ:
├── Dense Compute（dc2）: 計算重視
└── Dense Storage（ds2）: 容量重視

主要機能:
├── 列指向ストレージ（OLAP向け）
├── MPP（超並列処理）
├── 自動圧縮
├── S3へのスナップショット（増分）
├── Enhanced VPC Routing
├── Redshift Spectrum（S3を直接クエリ）
└── Concurrency Scaling（自動拡張）

Redshift Spectrum:
├── ロードなしでS3を直接クエリ
├── 既存Redshiftクラスターを利用
├── 独立してスケール
└── TBスキャン課金

災害対策:
├── スナップショットはS3保存（耐久性11ナイン）
├── 自動スナップショット（保持1〜35日）
├── 手動スナップショット（削除まで保持）
└── 別リージョンコピー可能
```

### 3. AWS Glue
```
構成要素:
├── Glue Crawler: スキーマ検出
├── Glue Catalog: メタデータ管理
├── Glue ETL: 変換処理
└── Glue Jobs: 定期/トリガー実行

ETL手順:
1. ソースをクロール → Catalog登録
2. 変換ロジック定義（Python/Scala）
3. ジョブ実行（変換）
4. ターゲットへロード（S3/Redshift等）

トリガー:
├── スケジュール（cron）
├── オンデマンド
└── イベント駆動（JobRun完了など）

機能:
├── サーバーレス（自動スケール）
├── 秒課金
├── Data Catalog（共有メタデータ）
├── Spark/Python対応
└── Bookmark機能（処理済み追跡）
```

### 4. Amazon EMR（Elastic MapReduce）
```
概要: マネージドHadoop基盤

対応フレームワーク:
├── Hadoop（MapReduce）
├── Spark（インメモリ）
├── HBase（NoSQL）
├── Presto（対話型クエリ）
├── Flink（ストリーム処理）
├── Hive（Hadoop上SQL）
└── 20種類以上

クラスター種別:
├── Transient（一時利用・低コスト）
└── Long-running（恒常運用）

ノード種別:
├── Master Node（管理）
├── Core Nodes（実行＋HDFS保存）
└── Task Nodes（実行のみ）

ストレージ:
├── HDFS（クラスタ内）
├── EMRFS（S3をHDFS的に利用）
└── インスタンスストア

用途:
├── 大規模データ処理
├── 機械学習
├── ログ分析
└── 大規模ETL
```

## 💡 よくある出題シナリオ

### シナリオ1: S3ログをSQLで分析
**Q**: S3のCloudTrailログを、インフラ構築なしでSQL分析したい
**✅ 答え**: Amazon Athena

### シナリオ2: ペタバイト分析
**Q**: 構造化データのペタバイト級BI
**✅ 答え**: Amazon Redshift（DWH）

### シナリオ3: Athenaコスト削減
**Q**: Athenaのクエリ費用が高い
**✅ 答え**: Parquet/ORC化、圧縮、パーティション分割

### シナリオ4: ETLパイプライン
**Q**: S3のCSVを毎晩変換してRedshiftへ
**✅ 答え**: AWS Glue（スケジューラ付きサーバーレスETL）

### シナリオ5: リアルタイムログ検索
**Q**: 全文検索を伴うリアルタイムログ検索
**✅ 答え**: Amazon OpenSearch（旧Elasticsearch）

### シナリオ6: 対話型ダッシュボード
**Q**: Athena/RedshiftのデータをBI可視化したい
**✅ 答え**: Amazon QuickSight

### シナリオ7: Spark処理
**Q**: 大規模データにApache Sparkジョブを実行
**✅ 答え**: Amazon EMR（Spark）

### シナリオ8: S3とRedshiftを横断クエリ
**Q**: S3データとRedshiftデータを結合したい
**✅ 答え**: Redshift Spectrum

## 🎓 速習ポイント

### Athena パーティション
```
パーティションなし:
s3://bucket/data/year2024month01day01.csv
s3://bucket/data/year2024month01day02.csv
└── 全ファイルをスキャン（高コスト）

パーティションあり:
s3://bucket/data/year=2024/month=01/day=01/data.csv
s3://bucket/data/year=2024/month=01/day=02/data.csv
└── 例: WHERE year=2024 AND month=01
└── 必要パーティションのみスキャン（低コスト）

パーティションプロジェクション:
├── MSCK REPAIR不要
├── Athenaが自動認識
└── 時系列データで有効
```

### Redshift ベストプラクティス
✅ 列指向ストレージを活用（自動）
✅ 適切なDistribution Keyを選ぶ
✅ Sort Keyでクエリ最適化
✅ 圧縮を有効化（自動）
✅ S3データはRedshift Spectrumを使う
✅ 定期的にVACUUM/ANALYZE
✅ バースト時はConcurrency Scaling
✅ バックアップ/DRはスナップショット

### Glue vs EMR vs Lambda
```
Glueを使うべきとき:
✅ サーバーレスETLが必要
✅ 比較的シンプルな変換
✅ メタデータカタログが必要
✅ Python/Scalaジョブ
✅ インフラ運用を避けたい

EMRを使うべきとき:
✅ 複雑なビッグデータ処理
✅ 特定Hadoopエコシステムが必要
✅ カスタム設定が必要
✅ Spotでコスト最適化したい
✅ 大規模ML処理

Lambdaを使うべきとき:
✅ 軽量な変換
✅ 実行15分未満
✅ イベント駆動処理
✅ データ量が小さい
```

## 📝 重要暗記ポイント

### Athena クエリ最適化
```
コスト比較（1TBデータ）:
├── CSV 非圧縮: $5.00
├── CSV 圧縮（GZIP）: $1.25（75%削減）
├── Parquet 非圧縮: $0.50（90%削減）
└── Parquet 圧縮: $0.15（97%削減）

性能:
├── 列指向（Parquet）= 約10倍高速
├── 圧縮 = 読み取りデータ減で高速化
└── パーティション = 必要データのみスキャン
```

### Redshift Distribution Style
| Style | 使う場面 | 例 |
|-------|----------|----|
| **EVEN** | 明確な結合キーがない | ファクト表（既定） |
| **KEY** | 結合が多い | customer_idで結合 |
| **ALL** | 小さな次元表 | 全ノード複製 |
| **AUTO** | Redshiftに任せる | 多くのケース |

### QuickSight 要点
```
概要: BIサービス
料金: ユーザー/セッション課金
ユーザー: Standard($9), Enterprise($18)

データソース:
├── Athena, Redshift, RDS
├── S3
├── SaaS（Salesforce等）
└── オンプレDB

機能:
├── ML Insights（異常検知）
├── 埋め込み分析
├── SPICE（インメモリ、ユーザーごと10GB無料）
├── 自動更新
└── モバイルアプリ

SPICE:
└── データ取り込みで高速クエリ
```

### OpenSearch（Elasticsearch）の用途
```
主用途:
├── ログ分析（ELK）
├── 全文検索
├── アプリ監視
├── セキュリティ分析
└── クリックストリーム分析

機能:
├── Multi-AZ
├── S3への自動スナップショット
├── 保存時/転送時暗号化
├── きめ細かいアクセス制御
└── Kibana連携

代替ではないもの:
❌ リレーショナルDB（RDSを使う）
❌ データウェアハウス（Redshiftを使う）
```

## 🚀 5分総復習

### 分析サービス意思決定ツリー
```
1. データソースは？
   S3 → Athena（その場でクエリ）
   DB → Redshift（DWH）

2. 用途は？
   アドホッククエリ → Athena
   BI/レポーティング → Redshift + QuickSight
   ETL → Glue
   ビッグデータ処理 → EMR
   検索 → OpenSearch
   ストリーミング分析 → Kinesis Analytics

3. サーバーレスが必要？
   YES → Athena, Glue, QuickSight
   NO  → Redshift, EMR, OpenSearch

4. データ規模は？
   <1TB → Athena
   TB〜PB → Redshift
   PB超 → EMR or Redshift
```

### よくある分析アーキテクチャ
```
1. DATA LAKE
   S3（レイク）→ Glue（カタログ）→ Athena（クエリ）→ QuickSight（可視化）

2. DATA WAREHOUSE
   各種ソース → Glue ETL → Redshift → QuickSight

3. LOG ANALYSIS
   Logs → Kinesis Firehose → S3 → Athena → OpenSearch

4. BIG DATA PROCESSING
   S3 → EMR（Spark）→ 処理 → S3/Redshift

5. HYBRID QUERY
   Redshift + Redshift Spectrum → S3とRedshiftを同時クエリ
```

### データ形式比較
| 形式 | 種別 | 圧縮効率 | 分割読込 | Athena性能 |
|------|------|----------|----------|------------|
| **CSV** | 行指向 | 普通 | 可 | 遅い・高コスト |
| **JSON** | 行指向 | 普通 | 可 | 遅い・高コスト |
| **Parquet** | 列指向 | 高い | 可 | ⭐ 最良 |
| **ORC** | 列指向 | 高い | 可 | ⭐ 最良 |
| **Avro** | 行指向 | 良い | 可 | 良い |

### よくあるミス
❌ 非圧縮CSVをAthenaでそのまま分析する（高コスト）
❌ Athena用S3データをパーティション化しない
❌ OLTP用途にRedshiftを使う（RDSを使う）
❌ AthenaでSELECT *を多用する
❌ S3データにSpectrumを使わない
❌ 単純ETLに長期EMR運用をする（Glueを検討）
❌ EMRクラスター停止忘れで課金増
❌ QuickSightでSPICEを使わず低速化

## 🎯 試験直前スピードチェック

**クイック問題**（答えは末尾）

1. S3データを最安でクエリする方法は？ __
2. Athenaに最適なデータ形式は？ __
3. Redshiftはどの種類のDB？ __
4. サーバーレスETLサービスは？ __
5. Redshift Spectrumとは？ __
6. Athenaは圧縮データをクエリ可能？ __
7. Hadoop/Spark向けサービスは？ __
8. QuickSightのインメモリエンジン名は？ __

---

### AWS Lake Formation
```
概要: セキュアなデータレイク構築
簡素化できること:
├── 複数ソースからの取り込み
├── データカタログ化（Glue Catalog利用）
├── データ変換
├── きめ細かいアクセス制御
└── 大規模セキュリティ運用

機能:
├── 代表的ソース向けBlueprint
├── 列/行レベルセキュリティ
├── 権限の一元管理
├── データ重複排除
└── ML変換

連携:
├── Athena, Redshift, EMR
├── QuickSight
└── SageMaker
```

### Kinesis Data Analytics
```
概要: ストリーミングデータのリアルタイム分析
入力: Kinesis Data Streams, Kinesis Firehose
クエリ: SQL または Apache Flink

用途:
├── リアルタイムダッシュボード
├── リアルタイム指標
├── 異常検知
└── 時系列分析

出力:
├── Kinesis Data Streams
├── Kinesis Data Firehose
├── Lambda
└── S3（Firehose経由）
```

## ⏱️ 次のアクション
- 学習時間: 約45〜60分
- 実践: Athenaを実際に実行、Redshiftの特性を確認
- 次に解く: AnalyticsのPractice Questions
- 次モジュール: Module 12 - Architecture Patterns

---

**クイック問題の解答**:
1) Athena（クエリ従量・サーバーレス）
2) Parquet または ORC（列指向・圧縮）
3) OLAP / データウェアハウス（OLTPではない）
4) AWS Glue
5) Redshiftからロード不要でS3をクエリ
6) はい（コスト削減に有効）
7) Amazon EMR（Elastic MapReduce）
8) SPICE
