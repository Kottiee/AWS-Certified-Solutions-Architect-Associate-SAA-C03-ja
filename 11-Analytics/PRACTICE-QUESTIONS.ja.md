# 分析サービス - 練習問題

> **⚠️ 免責事項:** これらはAWS公式ドキュメントをもとに学習目的で作成した**オリジナル問題**です。AWS認定試験の実問題ではありません。

## 試験レベル問題（SAA-C03）

---

### 問題 1
企業はアプリケーションログをCSV形式でAmazon S3に保存しています。データアナリストは、インフラを構築せずにこのデータへアドホックにSQLクエリを実行したいと考えています。どのAWSサービスを使うべきですか？

A. Amazon EMR
B. Amazon Athena
C. Amazon Redshift
D. AWS Glue

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**
- Athenaはサーバーレスで、S3データにSQLクエリを実行できる
- インフラ管理が不要
- クエリ課金（TBスキャン単位）
- アドホック分析に最適
- EMRはクラスター管理が必要
- RedshiftはDWH構築が必要
- GlueはETL向け

**参照:** Amazon Athena, Serverless Analytics
</details>

---

### 問題 2
ソリューションアーキテクトは、S3上の大規模データセットに対するAthenaクエリのコストを下げたいと考えています。現在データはCSV形式で、クエリはデータセット全体をスキャンしています。コスト最適化のために何をすべきですか？

A. Athenaのクエリタイムアウトを延長する
B. データをParquet形式へ変換し、よく使う条件でパーティション化する
C. データをAmazon Redshiftへ移行する
D. S3バージョニングを有効化する

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**
- AthenaはTBスキャン課金（$5/TB）
- Parquetは列指向で、スキャン量を30〜90%削減可能
- パーティション化で必要なデータのみスキャン
- この2つの最適化で大幅なコスト削減
- タイムアウト変更ではスキャン量は減らない
- Redshiftは別途インフラコストが増える
- S3バージョニングはクエリ性能に無関係

**参照:** Athena Performance Optimization, Columnar Formats
</details>

---

### 問題 3
リアルタイム分析アプリケーションで、Webサイトのクリックストリームデータを取り込み、独自ロジックで処理し、結果をDynamoDBへ保存する必要があります。データ取り込みに使うべきサービスはどれですか？

A. Amazon Kinesis Data Streams
B. Amazon Kinesis Data Firehose
C. Amazon SQS
D. AWS DataSync

<details>
<summary>回答を表示</summary>

**正解: A**

**解説:**
- Kinesis Data Streamsはリアルタイムストリーミング＋カスタム処理向け
- Lambda/EC2/KCLなどのカスタムコンシューマーに対応
- Firehoseは送信先へのロード用途（S3/Redshift等）
- SQSはキューであり、ストリーミング分析向けではない
- DataSyncはファイル転送サービス

**参照:** Kinesis Data Streams, Real-Time Processing
</details>

---

### 問題 4
企業は、ストリーミングされるIoTセンサーデータを運用負荷を最小化してAmazon S3に取り込み、分析したいと考えています。データは5分ごとに配信され、圧縮される必要があります。最も適切なサービスはどれですか？

A. Amazon Kinesis Data Streams with Lambda
B. Amazon Kinesis Data Firehose
C. Amazon SQS with Lambda
D. AWS IoT Core with custom application

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**
- Firehoseはフルマネージドでインフラ不要
- S3へ自動配信
- GZIP/Snappy等の圧縮を内蔵
- バッファ間隔60〜900秒（5分=300秒に対応）
- Data Streamsは別途コンシューマー実装が必要
- SQSは不必要な複雑化
- カスタム実装は保守負荷が高い

**参照:** Kinesis Data Firehose, Serverless Data Loading
</details>

---

### 問題 5
Athenaクエリが10TBをスキャンしていますが、必要なのは直近1か月のレコードのみです。S3バケットは年・月・日で整理されています。性能とコストを改善するにはどうすべきですか？

A. S3 Selectでデータを絞り込む
B. Athenaでパーティションテーブルを作成し、対象パーティションのみをクエリする
C. S3 Intelligent-Tieringを有効化する
D. Athenaワークグループを使う

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**
- パーティション化で必要データのみスキャン可能
- partition keyをWHEREで指定するとスキャン量を削減
- 大幅なコスト/性能改善
- S3 SelectはAthena最適化にはならない
- Intelligent-Tieringは保存コスト最適化
- ワークグループは整理/制御向けでスキャン最適化ではない

**参照:** Athena Partitioning, Query Optimization
</details>

---

### 問題 6
企業はストリーミングデータをリアルタイムでSQL分析し、しきい値超過時にアラート送信したいと考えています。どのサービスを使うべきですか？

A. Amazon Athena
B. Amazon Kinesis Data Analytics
C. Amazon Kinesis Data Firehose
D. AWS Glue

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**
- Kinesis Data Analyticsはストリームに対するSQL分析が可能
- リアルタイム分析用途に最適
- 結果をLambdaに連携してアラート可能
- Athenaは静的データ分析
- Firehoseは配信が主目的
- GlueはETLジョブ向け

**参照:** Kinesis Data Analytics, Stream Processing
</details>

---

### 問題 7
データエンジニアリングチームはApache Sparkジョブで大規模データ処理を行いたいと考えています。インフラ構築をマネージド化したい場合、どのサービスを使うべきですか？

A. Amazon Athena
B. Amazon EMR
C. AWS Glue
D. Amazon Kinesis

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**
- EMRはマネージドなビッグデータ基盤
- Spark/Hadoop/Hive等をサポート
- クラスター構築とスケーリングを管理
- 大規模分散処理に最適
- AthenaはSQLクエリ向け
- GlueもSpark対応だがEMRの方が制御性が高い
- Kinesisはストリーミング向け

**参照:** Amazon EMR, Big Data Processing
</details>

---

### 問題 8
企業はKinesis Data StreamsのストリーミングデータをAmazon Redshiftへ取り込みたいと考えています。運用効率が最も高い方法はどれですか？

A. LambdaでKinesisから読み取りRedshiftに書き込む
B. Kinesis Data FirehoseでRedshiftへ配信する
C. EC2でカスタムスクリプトを実行する
D. EMRで処理してロードする

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**
- FirehoseはData StreamsからRedshiftへ配信可能
- フルマネージドで実装負荷が低い
- 自動スケーリング
- S3経由でCOPYロード
- Lambda/EC2/EMRは運用負荷が高い

**参照:** Kinesis Data Firehose, Redshift Integration
</details>

---

### 問題 9
組織はAWS Glue CrawlerでS3データのスキーマ検出を行っています。新規データ到着時にCrawlerを自動実行したい場合、どう構成すべきですか？

A. 毎日手動実行する
B. EventBridgeでS3 PutObjectイベントをトリガーにCrawlerを起動する
C. AWS Configで変更を検出する
D. Crawlerは自動トリガーできない

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**
- EventBridge（旧CloudWatch Events）でS3イベント連携可能
- PutObjectイベントでCrawler実行
- 新規データ到着時の自動スキーマ検出を実現
- Configは設定監査向け

**参照:** AWS Glue, EventBridge Integration
</details>

---

### 問題 10
Kinesis Data Streamに4シャードあります。プロデューサーは毎秒5,000レコードを書き込んでいます。考えられる問題と解決策は？

A. シャード容量超過。シャード数を増やす
B. 保持期間が短すぎる。延長する
C. 問題なし。制限内
D. Kinesis Data Firehoseへ切り替える

<details>
<summary>回答を表示</summary>

**正解: A**

**解説:**
- 1シャードあたり書き込みは1,000レコード/秒
- 4シャード = 4,000レコード/秒
- 5,000レコード/秒は超過
- 最低5シャード（またはオンデマンド）へ拡張が必要
- ProvisionedThroughputExceededExceptionが発生しうる
- 保持期間は書き込み容量に影響しない

**計算:** 4 shards × 1,000 records/sec = 4,000 records/sec < 5,000 needed

**参照:** Kinesis Data Streams, Shard Capacity
</details>

---

### 問題 11
企業はS3、RDS、Redshiftなど複数ソースのデータを可視化し、業務ユーザー向けに対話型ダッシュボードを提供したいと考えています。どのAWSサービスを使うべきですか？

A. Amazon Athena
B. Amazon QuickSight
C. Amazon CloudWatch
D. Amazon Kinesis Data Analytics

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**
- QuickSightはBI/可視化サービス
- S3、RDS、Redshiftなど多様なデータソース接続
- 対話型ダッシュボードを作成可能
- SPICEで高速分析

**参照:** Amazon QuickSight, Business Intelligence
</details>

---

### 問題 12
ソリューションアーキテクトは、Kinesis Data Streamsのデータを再処理のために7日間保持したいと考えています。何を設定すべきですか？

A. 保持期間を168時間（7日）に設定する
B. Kinesis Data Firehoseで7日バッファを設定する
C. S3で拡張保持を有効化する
D. Kinesisは24時間以上保持できない

<details>
<summary>回答を表示</summary>

**正解: A**

**解説:**
- Kinesis Data Streamsは24時間〜365日で保持可能
- 既定は24時間
- 延長保持は追加料金あり
- 7日=168時間

**参照:** Kinesis Data Streams, Data Retention
</details>

---

### 問題 13
ETLパイプラインで複数ソースのデータ変換、メタデータ管理、定期実行が必要です。これらを提供するサービスはどれですか？

A. Amazon EMR
B. AWS Glue
C. AWS Data Pipeline
D. Amazon Athena

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**
- GlueはフルマネージドETL
- Glue Data Catalogでメタデータ管理
- Glue Crawlersでスキーマ検出
- Glue Jobsで変換処理
- スケジュール実行に対応

**参照:** AWS Glue, ETL Pipeline
</details>

---

### 問題 14
企業はCloudWatch Logsにアプリログを収集しています。これをSQLで分析したい場合、最もコスト効率の良い方法はどれですか？

A. CloudWatchに保持しLogs Insightsを使う
B. S3へエクスポートしてAthenaでクエリする
C. Kinesisへ流してData Analyticsを使う
D. Redshiftへロードする

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**
- 長期保管はCloudWatch LogsよりS3が安い
- S3 + AthenaでSQL分析可能（$5/TBスキャン）
- 低頻度分析では特に費用対効果が高い
- Logs Insightsは直近調査向け

**参照:** CloudWatch Logs Export, Athena Cost Optimization
</details>

---

### 問題 15
Kinesis Data Firehose配信ストリームで、受信JSONをLambdaで変換してからS3に保存したい。どう設定しますか？

A. Firehoseは変換に対応していない
B. データ変換を有効化し、LambdaのARNを指定する
C. Kinesis Data Analyticsで変換する
D. Firehose送信前に必ず変換する

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**
- FirehoseはLambda変換をネイティブサポート
- 配信設定で変換Lambdaを指定
- バッチごとにLambdaを呼び出し可能
- 追加サービスなしで実装できる

**参照:** Kinesis Data Firehose, Data Transformation
</details>

---

### 問題 16
AthenaでS3データを分析しています。Parquet化・パーティション化済みなのにクエリが遅い場合、さらに有効な改善策はどれですか？

A. ファイルサイズを128MB〜1GB程度に最適化する
B. CSV形式へ戻す
C. パーティションを無効化する
D. 1〜10MBの小さいファイルに分割する

<details>
<summary>回答を表示</summary>

**正解: A**

**解説:**
- 小さすぎるファイルが多いとオーバーヘッドが増える
- 128MB〜1GBが一般的な推奨サイズ
- Parquet継続＋適正ファイルサイズが有効

**参照:** Athena Performance, File Size Optimization
</details>

---

### 問題 17
AthenaとGlue Data Catalogを利用しています。チームごとに自分のデータセットのみクエリ可能にしたい場合、どのようにアクセス制御すべきですか？

A. S3バケットポリシーのみを使う
B. Glue Catalog権限を含むIAMポリシーで制御する
C. チームごとにAWSアカウントを分ける
D. Athenaワークグループのみを使う

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**
- Glue CatalogのDB/テーブル権限をIAMで細かく制御可能
- 必要に応じてS3ポリシーと併用
- ワークグループは整理・コスト制御向け

**参照:** Athena Security, Glue Catalog Permissions
</details>

---

### 問題 18
リアルタイムアプリで、ユーザーごとのイベント順序保証が必要です。ユーザー単位で到着順に処理するには、Kinesisのどの機能を使いますか？

A. すべてのレコードを同一シャードへ送る
B. ユーザーIDをパーティションキーに使う
C. Enhanced fan-outを有効化する
D. 複数シャードへラウンドロビン送信する

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**
- パーティションキーが送信先シャードを決める
- 同一キーは同一シャードに送られる
- シャード内はシーケンス番号で順序保証
- ユーザーIDをキーにすればユーザー単位で順序が保てる

**参照:** Kinesis Data Streams, Ordering Guarantees
</details>

---

### 問題 19
企業はVPC Flow LogsをAmazon S3に取り込み、Athenaで分析したいと考えています。最小構成でフルマネージドにするには何を設定すべきですか？

A. Kinesis Data Streams + Lambdaを使う
B. Flow Logsの送信先にKinesis Data Firehoseを設定する
C. CloudWatch Logsから手動エクスポートする
D. EC2でカスタム収集する

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**
- VPC Flow LogsはFirehoseへ直接配信可能
- FirehoseがS3へ自動配信
- フルマネージドでコード不要
- 圧縮やパーティション設定も可能

**参照:** VPC Flow Logs, Kinesis Data Firehose
</details>

---

### 問題 20
Athenaワークグループで「1日あたり1TB」のデータ使用制限を設定しています。ユーザーのクエリが1.5TBをスキャンする場合、どうなりますか？

A. クエリは実行され1.5TBをスキャンする
B. クエリは拒否される
C. クエリは実行され1TBだけスキャンする
D. 警告のみ表示され実行は継続する

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**
- ワークグループの使用量制限は強制適用される
- 制限超過クエリは拒否
- コスト超過防止に有効

**参照:** Athena Workgroups, Cost Controls
</details>

---

### 問題 21
金融系企業が、外部の金融データセットを社内アナリストへ安全に共有したいと考えています。サブスクリプション形式で提供され、データ更新が自動反映される必要があります。どのAWSサービスが適切ですか？

A. AWS Data Exchange
B. Amazon S3
C. AWS Glue Data Catalog
D. Amazon Redshift Data Sharing

<details>
<summary>回答を表示</summary>

**正解: A**

**解説:**
- AWS Data Exchangeは外部データの安全なサブスク配布に対応
- データ更新通知・自動更新に対応
- S3/Redshift/Lake Formationとも連携可能

**参照:** AWS Data Exchange, Data Sharing
</details>

---

### 問題 22
医療系企業が機微な患者データを保存・分析するため、AWSでセキュアなデータレイクを構築したいと考えています。行/列レベル権限、データカタログ、分析サービス連携が必要です。権限管理とデータカタログ統合に最も適したサービスはどれですか？

A. AWS Lake Formation
B. Amazon S3
C. AWS Glue
D. Amazon Athena

<details>
<summary>回答を表示</summary>

**正解: A**

**解説:**
- Lake Formationはデータレイク向けのきめ細かいアクセス制御を提供
- テーブル/列/行レベルで権限設定可能
- Glue Data CatalogやAthena/Redshift/EMRと統合
- S3は保存基盤であり権限統制の中核ではない

**参照:** AWS Lake Formation, Data Lake Security
</details>

---

### 問題 23
メディア企業が大量のログ/イベントデータをほぼリアルタイムで検索・可視化・分析したいと考えています。最適なAWSサービスはどれですか？

A. Amazon OpenSearch Service
B. Amazon Athena
C. Amazon Redshift
D. AWS Glue

<details>
<summary>回答を表示</summary>

**正解: A**

**解説:**
- OpenSearchはログ分析、検索、可視化向け
- 近リアルタイムのインデックス/クエリに適する
- ダッシュボード（Kibana/OpenSearch Dashboards）と連携
- AthenaはアドホックSQL向け
- RedshiftはDWH用途
- GlueはETL用途

**参照:** Amazon OpenSearch Service, Log Analytics
</details>
