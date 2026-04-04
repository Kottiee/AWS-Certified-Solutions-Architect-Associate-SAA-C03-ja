
-----

# アプリケーション統合 - 練習問題

> **⚠️ 免責事項:** これらはAWSドキュメントに基づき、学習目的で作成された**オリジナルの練習問題**です。AWS認定試験の**実際の試験問題ではありません。**

## 試験標準問題 (SAA-C03)

-----

### 問題 1

マイクロサービスアーキテクチャにおいて、複数のサービスが同じメッセージを独立して処理する必要がある非同期通信が必要です。最も適切なAWSサービスはどれですか？

A. Amazon SQS 標準キュー  
B. Amazon SNS  
C. Amazon Kinesis Data Streams  
D. AWS Step Functions

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**

  - **Amazon SNS** (Simple Notification Service) は、パブ/サブ（出版/購読）メッセージングパターンを提供します。
  - 1つのメッセージが発行される → 複数のサブスクライバーがコピーを受信する。
  - プッシュ型配信（プロアクティブ）。
  - ファンアウト（扇状に広がる）シナリオに最適。

**SNS アーキテクチャ**:

```
パブリッシャー (マイクロサービス A)
      ↓
   SNS トピック
      ↓
   ┌──┴──┬──────┬──────┐
   ↓     ↓      ↓      ↓
サービス B  C      D    Lambda
```

**SNS の機能**:

  - **メッセージフィルタリング**: サブスクライバーは関連するメッセージのみを受信。
  - **マルチプロトコル**: SQS, Lambda, HTTP/HTTPS, Eメール, SMS, モバイルプッシュに対応。
  - **メッセージ属性**: フィルタリング用のメタデータ。
  - **ファンアウト**: 1対多の配信。
  - **FIFO トピック**: 順序保証、正確に1回の配信。

**SNS vs SQS**:

| 機能 | SNS | SQS |
|---------|-----|-----|
| **パターン** | パブ/サブ (1対多) | キュー (1対1) |
| **配信** | プッシュ | プル |
| **サブスクライバー** | 複数 (ファンアウト) | メッセージごとに単一のコンシューマー |
| **保持** | 保持なし | 最大14日間 |
| **ユースケース** | 通知、ファンアウト | 切り離し、バッファリング |

**ユースケース例**:

```
注文確定イベント → SNS トピック
  ├→ 在庫サービス (在庫更新)
  ├→ 決済サービス (顧客への請求)
  ├→ 配送サービス (出荷作成)
  ├→ Eメールサービス (確認送信)
  └→ 分析サービス (メトリクス追跡)
```

**メッセージフィルタリング例**:

```json
{
  "FilterPolicy": {
    "order_type": ["premium", "express"],
    "amount": [{"numeric": [">=", 100]}]
  }
}
```

**参照:** Amazon SNS, Pub/Sub Messaging, Fan-out Pattern

</details>

-----

### 問題 2

アプリケーションにおいて、メッセージバッファリングを使用してコンポーネントを切り離す（デカップリング）必要があります。メッセージは厳格な順序で、正確に1回だけ処理される必要があります。どのサービスを使用すべきですか？

A. Amazon SQS 標準キュー  
B. Amazon SQS FIFO キュー  
C. Amazon SNS  
D. Amazon Kinesis Data Streams

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**

  - **Amazon SQS FIFO キュー**は、厳格な順序付けを伴う「正確に1回（exactly-once）」の処理を提供します。
  - メッセージは受信した順序で処理されます。
  - コンテンツベースの重複排除により、重複を防止します。
  - メッセージグループにより、並列処理が可能です。

**SQS 標準 vs FIFO**:

| 機能 | 標準 | FIFO |
|---------|----------|------|
| **順序付け** | ベストエフォート | 保証 (FIFO) |
| **配信** | 少なくとも1回 | 正確に1回 |
| **スループット** | 無制限 | 300 件/秒 (バッチで 3000) |
| **重複排除** | なし | あり (5分間のウィンドウ) |
| **ユースケース** | 高スループット | 順序が重要 |

**FIFO キューの機能**:

**1. メッセージ順序付け**:

```
送信: A → B → C
受信: A → B → C (常に)
```

**2. メッセージグループ**:

  - 並列処理のための複数のグループ。
  - 各グループ内での順序は維持されます。
  - 異なるグループは同時に処理できます。

<!-- end list -->

```python
# メッセージグループを指定して送信
sqs.send_message(
    QueueUrl='https://sqs.../MyQueue.fifo',
    MessageBody='Order payment processed',
    MessageGroupId='order-12345',  # 注文IDでグループ化
    MessageDeduplicationId='payment-abc-123'
)
```

**3. コンテンツベースの重複排除**:

  - メッセージ本文の SHA-256 ハッシュ。
  - 自動的な重複検出 (5分間)。
  - または MessageDeduplicationId を使用。

**4. 高スループットモード**:

  - バッチ処理で毎秒 3,000 メッセージ。
  - グループあたり毎秒 9,000 TPS (バッチ処理時)。

**FIFO キューの命名**:

  - 末尾が `.fifo` サフィックスである必要があります。
  - 例: `OrderQueue.fifo`

**ユースケース**:

  - **金融取引**: 二重請求の防止。
  - **注文処理**: 正しい順序での実行。
  - **コマンド実行**: 逐次操作。
  - **イベントソーシング**: 順序が重要な場合。

**メッセージグループの例**:

```
注文処理キュー.fifo
├─ グループ: order-001 → 決済 → 配送 → 確認
├─ グループ: order-002 → 決済 → 配送 → 確認
└─ グループ: order-003 → 決済 → 配送 → 確認

各グループは順序通りに処理され、グループ間は並列に処理される
```

**ベストプラクティス**:

1.  並列性のためにメッセージグループを使用する。
2.  コンテンツベースの重複排除を有効にする。
3.  適切な可視性タイムアウトを設定する。
4.  バッチ操作を使用する (1バッチ10メッセージ)。
5.  `ApproximateAgeOfOldestMessage` を監視する。

**参照:** Amazon SQS FIFO, Exactly-Once Processing, Message Ordering

</details>

-----

### 問題 3

サーバーレスアプリケーションにおいて、数千のIoTデバイスからのリアルタイムストリーミングデータを処理する必要があります。各デバイスは毎秒データを送信します。最も適切なサービスはどれですか？

A. Amazon SQS  
B. Amazon SNS  
C. Amazon Kinesis Data Streams  
D. Amazon MQ

<details>
<summary>回答を表示</summary>

**正解: C**

**解説:**

  - **Amazon Kinesis Data Streams** はリアルタイムストリーミングデータ用に設計されています。
  - 数千のプロデューサーからの大規模なデータ取り込みに対応。
  - 複数のコンシューマーが同じデータストリームを読み取ることができます。
  - データ保持期間（24時間から365日）。
  - シャードごとに順序付けられたレコード。

**Kinesis Data Streams アーキテクチャ**:

```
IoT デバイス (プロデューサー)
   ↓
Kinesis ストリーム (シャード)
   ├→ Lambda (リアルタイム処理)
   ├→ Kinesis Data Analytics (SQL クエリ)
   ├→ Kinesis Data Firehose (S3/Redshift へ)
   └→ カスタムコンシューマー (EC2, コンテナ)
```

**Kinesis の主要コンセプト**:

**1. シャード (Shards)**:

  - スループットの単位。
  - 1シャードあたり書き込み 1 MB/秒、読み取り 2 MB/秒。
  - 1シャードあたり書き込み 1,000 レコード/秒。
  - シャード内で順序保証。

**2. レコード (Records)**:

  - パーティションキー (シャードを決定)。
  - シーケンス番号 (シャード内の順序)。
  - データブロブ (最大 1 MB)。

**3. コンシューマー (Consumers)**:

  - 複数のコンシューマーが同じストリームを読取可能。
  - 拡張ファンアウト (コンシューマーごとに専用の 2 MB/秒)。
  - 共有スループットモード (合計 2 MB/秒)。

**Kinesis の機能**:

| 機能 | 説明 |
|---------|-------------|
| **保持期間** | デフォルト24時間、最大365日 |
| **順序付け** | パーティションキー/シャードごと |
| **リプレイ** | データの再処理が可能 |
| **スケーリング** | シャードの追加/削除 |
| **暗号化** | 保管時 (KMS)、転送時 (HTTPS) |

**容量計画**:

```
入力データ: 1,000 デバイス × 1 KB/秒 = 1 MB/秒
必要なシャード: 1 MB/秒 ÷ 1 MB/秒(1シャード) = 1 シャード

入力データ: 5,000 デバイス × 1 KB/秒 = 5 MB/秒
必要なシャード: 5 MB/秒 ÷ 1 MB/秒(1シャード) = 5 シャード
```

**Kinesis プロデューサー** (Python):

```python
import boto3
import json

kinesis = boto3.client('kinesis')

def send_iot_data(device_id, temperature, humidity):
    data = {
        'device_id': device_id,
        'temperature': temperature,
        'humidity': humidity,
        'timestamp': int(time.time())
    }
    
    kinesis.put_record(
        StreamName='IoTDataStream',
        Data=json.dumps(data),
        PartitionKey=device_id  # 特定のシャードにルーティング
    )
```

**Kinesis コンシューマー** (Lambda):

```python
def lambda_handler(event, context):
    for record in event['Records']:
        # Kinesis データは base64 エンコードされている
        payload = base64.b64decode(record['kinesis']['data'])
        data = json.loads(payload)
        
        # IoT データの処理
        device_id = data['device_id']
        temperature = data['temperature']
        
        # 温度が高すぎる場合にアラート
        if temperature > 80:
            send_alert(device_id, temperature)
```

**Kinesis ファミリー**:

| サービス | ユースケース |
|---------|----------|
| **Data Streams** | カスタムのリアルタイム処理 |
| **Data Firehose** | S3, Redshift, ES へのロード (容易) |
| **Data Analytics** | ストリーミングデータへの SQL 実行 |
| **Video Streams** | ビデオの取り込みと処理 |

**Kinesis vs SQS**:

| 特徴 | Kinesis | SQS |
|---------|---------|-----|
| **順序付け** | シャードごと | FIFO キューのみ |
| **複数コンシューマー** | 可能 | 不可 (SNS経由でファンアウト) |
| **保持期間** | 最大365日 | 最大14日 |
| **リプレイ** | 可能 | 不可 |
| **ユースケース** | ストリーミング、分析 | 切り離し、バッファリング |

**Kinesis を使用すべき時**:

  - リアルタイム分析。
  - ログおよびイベントデータの収集。
  - IoT データの取り込み。
  - 複数のコンシューマーが同じデータを必要とする場合。
  - データをリプレイ（再送）する必要がある場合。
  - ストリーミング ETL。

**参照:** Amazon Kinesis Data Streams, Real-Time Streaming, IoT Data Ingestion

</details>

-----

### 問題 4

ある企業が、1つのSNSメッセージが異なるサービスの複数のSQSキューをトリガーするファンアウトパターンを実装したいと考えています。このアーキテクチャは何と呼ばれますか？

A. SQS Message Chaining（メッセージ連鎖）  
B. SNS to SQS Fan-out  
C. Kinesis Fan-out  
D. EventBridge Routing

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**

  - **SNS to SQS Fan-out** は、並列・非同期処理のための一般的なパターンです。
  - SNSへの単一のメッセージが複数のSQSキューをトリガーします。
  - 各サービスは独自のキューからメッセージを消費します。
  - 切り離された（デカップリングされた）、回復力の高いアーキテクチャです。

**SNS-SQS Fan-out アーキテクチャ**:

```
イベントソース
      ↓
   SNS トピック
      ↓
  ┌──┴──┬────────┬────────┐
  ↓     ↓        ↓        ↓
SQS-1  SQS-2    SQS-3    SQS-4
  ↓     ↓        ↓        ↓
Svc-A  Svc-B    Svc-C    Svc-D
```

**メリット**:

**1. 並列処理**:

  - サービスが独立して処理を行う。
  - サービス間でのブロッキング（待ち）が発生しない。

**2. 信頼性**:

  - SQS がメッセージの永続性を提供。
  - 失敗時のリトライが可能。
  - 失敗したメッセージのためのデッドレターキュー（DLQ）。

**3. スケーラビリティ**:

  - 各サービスが独立してスケールする。
  - 既存の仕組みを変更せずに新しいサブスクライバーを追加できる。

**4. 切り離し (Decoupling)**:

  - サービス同士がお互いを知る必要がない。
  - サービスの追加・削除が容易。

**設定手順**:

**ステップ 1: SNS トピックの作成**:

```bash
aws sns create-topic --name OrderEvents
```

**ステップ 2: SQS キューの作成**:

```bash
aws sqs create-queue --queue-name InventoryQueue
aws sqs create-queue --queue-name PaymentQueue
aws sqs create-queue --queue-name ShippingQueue
```

**ステップ 3: キューをトピックにサブスクライブ（登録）**:

```bash
aws sns subscribe \
  --topic-arn arn:aws:sns:us-east-1:123456789012:OrderEvents \
  --protocol sqs \
  --notification-endpoint arn:aws:sqs:us-east-1:123456789012:InventoryQueue
```

**ステップ 4: SQS ポリシーの更新** (SNS からの送信を許可):

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {"Service": "sns.amazonaws.com"},
    "Action": "SQS:SendMessage",
    "Resource": "arn:aws:sqs:us-east-1:123456789012:InventoryQueue",
    "Condition": {
      "ArnEquals": {
        "aws:SourceArn": "arn:aws:sns:us-east-1:123456789012:OrderEvents"
      }
    }
  }]
}
```

**実例 - Eコマースの注文**:

```
顧客が注文
        ↓
  SNS: OrderPlaced
        ↓
  ┌─────┴─────┬──────────┬──────────┐
  ↓           ↓          ↓          ↓
在庫          決済       配送       Eメール
キュー        キュー     キュー     キュー
  ↓           ↓          ↓          ↓
在庫          決済       出荷       確認
更新          処理       作成       送信
```

**メッセージフィルタリング** (各サービスが必要なメッセージのみを取得):

```json
{
  "InventoryQueue": {
    "FilterPolicy": {
      "order_type": ["physical_goods"]
    }
  },
  "EmailQueue": {
    "FilterPolicy": {
      "notification_type": ["order_confirmation"]
    }
  }
}
```

**エラー処理 - デッドレターキュー (DLQ)**:

```
SQS キュー → 処理 → 失敗 (リトライ後)
                            ↓
                    デッドレターキュー
                            ↓
                    アラート/手動確認
```

**ベストプラクティス**:

1.  メッセージフィルタリングを使用して不要な処理を減らす。
2.  失敗したメッセージ用にデッドレターキューを設定する。
3.  適切な可視性タイムアウトを設定する。
4.  コスト最適化のためにバッチ操作を使用する。
5.  キューの深さ（CloudWatch）を監視する。
6.  コンシューマー側に冪等性（Idempotency）を実装する。

**SNS-SQS vs Kinesis**:

  - **SNS-SQS**: サービスごとに異なる処理を行う場合。
  - **Kinesis**: 同じデータを異なるコンシューマーが処理する場合。

**参照:** SNS to SQS Fan-out, Asynchronous Processing, Decoupling Patterns

</details>

-----

### 問題 5

サーバーレスアプリケーションにおいて、条件分岐ロジック、並列実行、およびエラー処理を含む複数の Lambda 関数を調整（コーディネート）する必要があります。どのサービスを使用すべきですか？

A. 複数のキューを持つ Amazon SQS  
B. メッセージフィルタリングを持つ Amazon SNS  
C. AWS Step Functions  
D. Amazon EventBridge

<details>
<summary>回答を表示</summary>

**正解: C**

**解説:**

  - **AWS Step Functions** はサーバーレスワークフローをオーケストレート（調整）します。
  - 視覚的なワークフローデザイナー。
  - 組み込みのエラー処理とリトライ。
  - 並列実行、条件分岐ロジック、待機状態。
  - 200以上の AWS サービスと統合。

**Step Functions ステートマシン例**:
（※JSONコード部分は技術的な内容のため原文のままですが、各ステート名は注文処理ワークフローの流れを示しています：注文検証 → 在庫確認 → 並列処理（決済・在庫確保・確認送信） → 出荷作成）

**Step Functions のステートタイプ**:

| ステートタイプ | 目的 | 例 |
|------------|---------|---------|
| **Task** | 作業の実行 (Lambda, ECS, SNS など) | API 呼び出し、データ処理 |
| **Choice** | 条件分岐ロジック (if/else) | 在庫状況の確認 |
| **Parallel** | 分岐を同時に実行 | 決済 + 在庫確保 + Eメール |
| **Wait** | 遅延 (秒数、タイムスタンプ) | 承認待ち |
| **Pass** | 入力を出力に渡し、変換する | データ変換 |
| **Map** | 配列の反復処理 | バッチ項目の処理 |
| **Succeed** | 正常終了 | 注文完了 |
| **Fail** | 異常終了 | 検証失敗 |

**エラー処理**:

**1. Retry (リトライ)**:
エラー発生時に自動的に再試行する設定。

**2. Catch (キャッチ)**:
特定のエラーが発生した際に、別のパス（返金処理など）へ遷移させる設定。

**Step Functions ワークフロータイプ**:

| タイプ | 実行期間 | 実行レート | ユースケース |
|------|----------|----------------|----------|
| **標準 (Standard)** | 最大1年間 | 2,000/秒 | 長期間実行、正確に1回 |
| **高速 (Express)** | 最大5分間 | 100,000/秒 | 大規模ボリューム、少なくとも1回 |

**Step Functions vs 代替案**:

| ツール | 最適な用途 |
|------|----------|
| **Step Functions** | 複雑なワークフロー、オーケストレーション |
| **Lambda** | 単一のタスク |
| **SQS** | キューベースの処理 |
| **EventBridge** | イベントルーティング |

**実例**:

1.  **ETL パイプライン**: 抽出 → 変換 → ロード。
2.  **注文処理**: 検証 → 決済 → 履行。
3.  **ビデオ処理**: アップロード → トランスコード → サムネイル → 配信。

**参照:** AWS Step Functions, Serverless Orchestration, Workflow Management

</details>

-----

### 問題 6

最小限のコードで、ストリーミングデータを S3、Redshift、および Elasticsearch にロードする必要があります。どのサービスが最も適切ですか？

A. カスタムコンシューマーを使用した Amazon Kinesis Data Streams  
B. Amazon Kinesis Data Firehose  
C. Kinesis によってトリガーされる AWS Lambda  
D. Lambda コンシューマーを使用した Amazon SQS

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**

  - **Amazon Kinesis Data Firehose** は、ストリーミングデータをロードするための完全マネージド型サービスです。
  - コードは不要です（Data Streams はカスタムコンシューマーの作成が必要）。
  - 自動スケーリング。
  - 組み込みのデータ変換（Lambda 使用）。
  - S3, Redshift, Elasticsearch, HTTP エンドポイントへの直接配信。

**Kinesis Data Firehose アーキテクチャ**:

```
データプロデューサー
  ├─ アプリケーション
  ├─ IoT デバイス
  ├─ クリックストリーム
  └─ ログ
        ↓
Kinesis Data Firehose
  ├─ オプション: Lambda 変換
  ├─ オプション: フォーマット変換 (Parquet, ORC)
  └─ バッファリング (サイズ/時間)
        ↓
配信先
  ├─ Amazon S3
  ├─ Amazon Redshift (S3経由)
  ├─ Amazon OpenSearch (旧 Elasticsearch)
  ├─ Splunk
  └─ HTTP エンドポイント
```

**Firehose vs Data Streams**:

| 特徴 | Data Firehose | Data Streams |
|---------|---------------|--------------|
| **管理** | 完全マネージド型 | シャードの管理が必要 |
| **配信先** | 組み込み (S3, Redshift, 等) | カスタムコードが必要 |
| **スケーリング** | 自動 | 手動でのシャード管理 |
| **保持期間** | 保持なし | 24時間～365日 |
| **コンシューマー** | 1つ (配信) | 複数可能 |
| **ユースケース** | 配信先へのロード | カスタム処理 |

**Firehose を使用すべき時**:

  - S3, Redshift, ES, Splunk にデータをロードしたい。
  - 複数のコンシューマーを必要としない。
  - 完全マネージドなソリューションを求めている。
  - ニアリアルタイム（60秒以上の遅延）で許容できる。

**参照:** Amazon Kinesis Data Firehose, Streaming Data Delivery, Data Lake

</details>

-----

### 問題 7

ある企業が、イベントの内容に基づいて、複数の AWS サービスからのイベントを異なるターゲットにルーティングしたいと考えています。中央集中型のイベントルーティングを提供するサービスはどれですか？

A. Amazon SNS  
B. Amazon SQS  
C. Amazon EventBridge  
D. AWS Step Functions

<details>
<summary>回答を表示</summary>

**正解: C**

**解説:**

  - **Amazon EventBridge** は、アプリケーションおよび AWS サービスのイベント用サーバーレスイベントバスです。
  - イベントパターンを使用したコンテンツベースのルーティング。
  - 100以上の AWS サービスがイベントソースとして統合。
  - 20以上の AWS サービスをターゲットとして設定可能。

**EventBridge コンポーネント**:

1.  **イベント**: 発生した事象（JSONデータ）。
2.  **ルール**: どのイベントをどのターゲットに送るかを決定するフィルタ。
3.  **イベントバス**: イベントを受信する場所（デフォルト、カスタム、パートナー）。
4.  **ターゲット**: イベントがルーティングされる先（Lambda, SQS, SNS等）。

**主な機能**:

  - **コンテンツベースのフィルタリング**: 特定の属性（数値、プレフィックス、IPアドレス等）に一致するものだけをルーティング。
  - **入力トランスフォーマー**: ターゲットに送る前に JSON を整形。
  - **アーカイブとリプレイ**: イベントを保存し、後で再実行可能。
  - **スケジュール済みイベント**: cron 形式での定時実行。

**EventBridge vs SNS vs SQS**:

| 特徴 | EventBridge | SNS | SQS |
|---------|-------------|-----|-----|
| **パターン** | イベントルーティング | パブ/サブ | キュー |
| **フィルタリング** | 高度 (コンテンツベース) | 基本 (属性ベース) | なし |
| **ソース** | 100以上の AWS サービス | アプリケーション | アプリケーション |

**参照:** Amazon EventBridge, Event-Driven Architecture, Serverless Integration

</details>

-----

### 問題 8

レガシーアプリケーションが Java Message Service (JMS) API を使用しています。コードの変更を最小限に抑えた、最適な AWS 移行パスは何ですか？

A. Amazon SQS へ移行  
B. Amazon SNS へ移行  
C. Amazon MQ へ移行  
D. Kinesis を使用して書き換え

<details>
<summary>回答を表示</summary>

**正解: C**

**解説:**

  - **Amazon MQ** は、Apache ActiveMQ および RabbitMQ 用のマネージド型メッセージブローカーです。
  - JMS, AMQP, MQTT, OpenWire, STOMP プロトコルをサポート。
  - 「リフト＆シフト」において、コード変更を最小限に抑えられます。
  - 既存のメッセージング API と互換性があります。

**Amazon MQ vs SQS/SNS**:

| 特徴 | Amazon MQ | SQS/SNS |
|---------|-----------|---------|
| **プロトコル** | JMS, AMQP, MQTT, STOMP | AWS API のみ |
| **移行** | 最小限のコード変更 | コードの書き換えが必要 |
| **管理** | マネージドブローカー | 完全サーバーレス |

**参照:** Amazon MQ, Message Brokers, JMS Migration

</details>

-----

### 問題 9

アプリケーションがカスタムメトリクスを CloudWatch に発行しています。これらのメトリクスに基づいて自動応答をトリガーする必要があります。最適なアプローチはどれですか？

A. 定期的に CloudWatch API をポーリングする  
B. SNS を備えた CloudWatch アラームを使用する  
C. CloudWatch イベント用に EventBridge ルールを使用する  
D. メトリクスをチェックするために Lambda を使用する

<details>
<summary>回答を表示</summary>

**正解: B**

**解説:**

  - **CloudWatch アラーム**は、メトリクスを監視し、自動的にアクションをトリガーします。
  - 通知や自動応答のために SNS と統合されています。
  - Auto Scaling、EC2 アクション、SNS、Systems Manager をトリガーできます。

**アラームのアクション**:

1.  **SNS 通知**: Eメール/SMS/Lambda 等への通知。
2.  **Auto Scaling**: インスタンスのスケーリング。
3.  **EC2 アクション**: インスタンスの停止/終了/再起動。
4.  **Systems Manager**: オートメーションプレイブックの実行。

**参照:** CloudWatch Alarms, Automated Monitoring, Metric-Based Actions

</details>

-----

### 問題 10

ある企業が、API パスに基づいて異なる Lambda 関数をトリガーするために API Gateway を実装したいと考えています。これを可能にする機能はどれですか？

A. API Gateway ステージ (Stages)  
B. API Gateway メソッド (Methods)  
C. Lambda 統合を備えた API Gateway リソース (Resources)  
D. API Gateway オーソライザー (Authorizers)

<details>
<summary>回答を表示</summary>

**正解: C**

**解説:**

  - **API Gateway リソース**は API パスを定義します。
  - 各リソース/メソッドは、異なる Lambda 関数と統合できます。
  - マイクロサービスへの REST API ルーティングを可能にします。

**例**:

  - `GET /users` → Lambda: ユーザー一覧
  - `POST /orders` → Lambda: 注文作成

**その他の主要機能**:

  - **スロットリング**: レート制限。
  - **キャッシュ**: レスポンスをキャッシュして負荷軽減。
  - **ステージ変数**: 環境（dev/prod）ごとの変数設定。

**参照:** API Gateway, Lambda Integration, REST API Routing

</details>

-----

### 問題 41 〜 46 (要約)

  * **問題 41 (SaaS 連携):** Salesforce から S3 へのデータ転送をコードなしで行う → **Amazon AppFlow**
  * **問題 42 (GraphQL):** モバイルアプリ用の GraphQL API マネージドサービス → **AWS AppSync**
  * **問題 43 (ハイブリッドコンテナ):** オンプレミスサーバーで ECS を管理・実行する → **Amazon ECS Anywhere**
  * **問題 44 (モバイルテスト):** クラウド上の実機デバイスでテストを行う → **AWS Device Farm**
  * **問題 45 (マーケティング):** ユーザー行動に基づくターゲット通知・SMS 送信 → **Amazon Pinpoint**
  * **問題 46 (Managed Kafka):** AWS 上の完全マネージドな Apache Kafka 環境 → **Amazon MSK**

-----

## まとめ：サービス選択のクイックガイド

| ニーズ | 使用するサービス |
| :--- | :--- |
| ファンアウト（同じメッセージを多くのサービスへ） | **SNS** |
| キューイング（切り離し、バッファリング） | **SQS** |
| 正確に1回、順序保証 | **SQS FIFO** |
| リアルタイムストリーム処理 | **Kinesis Data Streams** |
| ストリームを S3/Redshift 等へロード | **Kinesis Data Firehose** |
| 複雑なワークフロー、オーケストレーション | **Step Functions** |
| AWS サービス間のイベントルーティング | **EventBridge** |
| JMS/AMQP からの移行 | **Amazon MQ** |
| メトリクスベースの自動化 | **CloudWatch Alarms** |
| Lambda 用の REST API | **API Gateway** |
| SaaS データの統合 | **AppFlow** |
| マネージド GraphQL API | **AppSync** |
| オンプレミスでのコンテナ実行 | **ECS Anywhere** |
| 実機デバイスでのテスト | **Device Farm** |
| ターゲットを絞ったユーザーメッセージング | **Pinpoint** |
| マネージド Kafka | **MSK** |
