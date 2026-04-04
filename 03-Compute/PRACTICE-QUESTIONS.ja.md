# コンピューティングサービス - 練習問題

> **⚠️ 免責事項:** これらは AWS ドキュメントに基づいて教育目的で作成した**オリジナル問題**です。AWS 認定試験の**実際の試験問題ではありません**。

## 試験レベル問題（SAA-C03）

---

### 問題 1
中断を許容できるバッチ処理ジョブを実行し、コストを最小化したい企業があります。利用すべき EC2 の料金モデルはどれですか？

A. オンデマンドインスタンス  
B. リザーブドインスタンス  
C. スポットインスタンス  
D. Dedicated Hosts  

<details>
<summary>解答を表示</summary>

**答え: C**

**解説:**
- スポットインスタンスはオンデマンド比で最大 90% 割引
- AWS が容量を必要とするとき、2 分前通知で中断される可能性がある
- フォールトトレラントで柔軟なワークロードに最適
- **主な用途**: バッチ処理、データ分析、画像処理、CI/CD
- A: オンデマンドは最も高価
- B: リザーブドは 1〜3 年のコミットが必要
- D: Dedicated Hosts はコンプライアンス向けで高コスト

**重要ポイント**:
- Spot = 最安だが中断あり
- 向いている用途: ステートレス・耐障害・柔軟なワークロード

**参照:** EC2 料金モデル、Spot Instances
</details>

---

### 問題 2
あるアプリケーションは毎週月曜の午前9時に予測可能なトラフィックスパイクが発生します。最もコスト効率の高い Auto Scaling の方式はどれですか？

A. Target Tracking Scaling  
B. Simple Scaling  
C. Step Scaling  
D. Scheduled Scaling  

<details>
<summary>解答を表示</summary>

**答え: D**

**解説:**
- Scheduled Scaling は事前定義スケジュールでスケールする
- 予測可能なトラフィックパターンに最適
- スパイク前にスケールできる（プロアクティブ）
- 既知パターンでは最もコスト効率が高い

**スケジュール例**:
```bash
aws autoscaling put-scheduled-update-group-action \
  --auto-scaling-group-name my-asg \
  --scheduled-action-name monday-morning-scale \
  --recurrence "0 8 * * MON" \
  --desired-capacity 10
```

- A: Target Tracking はメトリクス変化後のリアクティブ対応
- B/C: Step/Simple もリアクティブ
- **Scheduled = プロアクティブ、Target/Step/Simple = リアクティブ**

**参照:** Auto Scaling Policies、Scheduled Scaling
</details>

---

### 問題 3
複数 AZ にまたがる高可用性と、自動トラフィック分散が必要な Web アプリがあります。HTTP/HTTPS で高度なルーティングが必要な場合、どのロードバランサーを使うべきですか？

A. Classic Load Balancer  
B. Application Load Balancer  
C. Network Load Balancer  
D. Gateway Load Balancer  

<details>
<summary>解答を表示</summary>

**答え: B**

**解説:**
- **Application Load Balancer (ALB)** はレイヤー7（HTTP/HTTPS）で動作
- パス/ホスト/ヘッダーなど高度なルーティングに対応
- WebSocket と HTTP/2 をサポート
- モダンな Web アプリに最適

**ALB の機能**:
- パスベース: `/api/*` → API サーバー、`/images/*` → 画像サーバー
- ホストベース: `api.example.com` と `www.example.com`
- クエリ文字列/ヘッダーでのルーティング
- 固定レスポンス、リダイレクト
- AWS WAF 連携
- 認証（OIDC、Cognito）

**ロードバランサー比較**:
- **CLB**: レガシー、Layer 4/7、基本機能
- **ALB**: Layer 7、HTTP/HTTPS、高度ルーティング
- **NLB**: Layer 4、TCP/UDP、超高性能、静的 IP
- **GLB**: Layer 3、サードパーティ仮想アプライアンス

**参照:** Application Load Balancer、ELB Types
</details>

---

### 問題 4
マイクロサービスアプリケーションで、超低レイテンシ・秒間数百万リクエスト・静的 IP アドレスが必要です。どのロードバランサーを使うべきですか？

A. Application Load Balancer  
B. Network Load Balancer  
C. Classic Load Balancer  
D. CloudFront  

<details>
<summary>解答を表示</summary>

**答え: B**

**解説:**
- **Network Load Balancer (NLB)** は Layer 4（TCP/UDP/TLS）で動作
- 秒間数百万リクエストを処理可能
- 超低レイテンシ（マイクロ秒）
- 静的 IP（AZ ごとに 1 つ）
- 送信元 IP を保持

**NLB のユースケース**:
- 極限の性能要件
- 静的/Elastic IP が必要（ファイアウォール許可リスト）
- TCP/UDP トラフィック
- PrivateLink エンドポイント
- ゲーム、IoT、リアルタイムアプリ

**NLB vs ALB**:
- **NLB**: Layer 4、高速、静的 IP、TCP/UDP
- **ALB**: Layer 7、高度ルーティング、HTTP/HTTPS

**参照:** Network Load Balancer、Layer 4 vs Layer 7
</details>

---

### 問題 5
サーバー管理なしで、イベントに応じてコード実行したい企業があります。トリガー時のみ実行され、自動スケールも必要です。どのサービスを使うべきですか？

A. Amazon EC2 with Auto Scaling  
B. AWS Lambda  
C. Amazon ECS  
D. AWS Elastic Beanstalk  

<details>
<summary>解答を表示</summary>

**答え: B**

**解説:**
- **AWS Lambda** はサーバーレスコンピュート
- イベント駆動で実行
- 自動スケーリング（同時実行 1 〜 数千）
- 実行時間分のみ課金（ミリ秒単位）
- サーバー管理不要

**Lambda の特性**:
- **最大実行時間**: 15 分
- **メモリ**: 128 MB 〜 10,240 MB
- **デプロイパッケージ**: 50 MB（zip）、250 MB（展開後）
- **同時実行数**: 1000（デフォルト、引き上げ可）
- **課金**: リクエスト数 + 実行時間（GB 秒）

**Lambda のトリガー**:
- API Gateway（REST API）
- S3 イベント
- DynamoDB Streams
- EventBridge（スケジュール/イベント）
- SNS、SQS
- Kinesis

**参照:** AWS Lambda、Serverless Computing
</details>

---

### 問題 6
EC2 上のアプリでトラフィックが変動し、CPU 平均使用率を 50% に維持したいです。設定すべき Auto Scaling ポリシーはどれですか？

A. Simple Scaling  
B. Step Scaling  
C. Target Tracking Scaling  
D. Predictive Scaling  

<details>
<summary>解答を表示</summary>

**答え: C**

**解説:**
- **Target Tracking Scaling** は目標メトリクスを維持するよう自動調整
- 例: CPU 50% を指定すれば Auto Scaling が自動制御
- 設定/運用が最も簡単
- CloudWatch アラームを自動作成・管理

**設定例**:
```json
{
  "TargetValue": 50.0,
  "PredefinedMetricSpecification": {
    "PredefinedMetricType": "ASGAverageCPUUtilization"
  }
}
```

**定義済みメトリクス**:
- `ASGAverageCPUUtilization`
- `ASGAverageNetworkIn`
- `ASGAverageNetworkOut`
- `ALBRequestCountPerTarget`

**スケーリングポリシーの種類**:
- **Target Tracking**: 指定値維持（最も簡単）
- **Step**: しきい値に応じ段階的増減
- **Simple**: 単一調整（レガシー）
- **Predictive**: ML 予測ベース

**参照:** Target Tracking Scaling、Auto Scaling Policies
</details>

---

### 問題 7
EC2 インスタンスを管理せずに Docker コンテナを実行したい場合、どのサービスを使うべきですか？

A. Amazon ECS（EC2 起動タイプ）  
B. Amazon ECS（Fargate 起動タイプ）  
C. Amazon EKS  
D. AWS Elastic Beanstalk  

<details>
<summary>解答を表示</summary>

**答え: B**

**解説:**
- **AWS Fargate** はサーバーレスコンテナ基盤
- EC2 インスタンス管理が不要
- 使用した vCPU/メモリ分だけ課金
- ECS/EKS の両方で利用可能

**コンテナサービスの選択肢**:

| サービス | 説明 | 管理範囲 |
|---------|-------------|------------|
| **ECS + EC2** | EC2 上のコンテナオーケストレーション | インスタンス管理が必要 |
| **ECS + Fargate** | サーバーレスコンテナ | インスタンス管理不要 |
| **EKS + EC2** | EC2 上の Kubernetes | インスタンス + K8s 管理 |
| **EKS + Fargate** | サーバーレス Kubernetes | インスタンス管理不要 |

**Fargate の利点**:
- インスタンスの調達/スケーリング不要
- パッチ適用やセキュリティ管理不要
- タスク単位課金
- 運用がシンプル

**Fargate と EC2 の使い分け**:
- **Fargate**: シンプル運用、運用負荷を減らしたい
- **EC2**: 特定インスタンスタイプ/GPU 必要、長期稼働でコスト最適化

**参照:** AWS Fargate、Amazon ECS
</details>

---

### 問題 8
同一クラスター内の他インスタンスと低遅延・高スループット接続が必要な EC2 インスタンスがあります。どのプレースメントグループを使うべきですか？

A. Cluster Placement Group  
B. Spread Placement Group  
C. Partition Placement Group  
D. プレースメントグループは不要  

<details>
<summary>解答を表示</summary>

**答え: A**

**解説:**
- **Cluster Placement Group** は単一 AZ 内でインスタンスを近接配置
- 低遅延ネットワーク（インスタンス間 10 Gbps）
- 高スループット/拡張ネットワーキング
- 同一ラック付近へ配置

**プレースメントグループの種類**:

| 種類 | 目的 | AZ | 最大インスタンス |
|------|---------|----|----|
| **Cluster** | 低遅延・高スループット | 単一 AZ | インスタンスタイプ依存 |
| **Spread** | 障害の相関を低減 | 複数 AZ | AZ ごとに 7 台 |
| **Partition** | 大規模分散ワークロード | 複数 AZ | AZ ごとに 7 パーティション |

**ユースケース**:
- **Cluster**: HPC、ビッグデータ、低遅延アプリ
- **Spread**: 重要アプリ（インスタンスを別ラックに分離）
- **Partition**: Hadoop、Cassandra、Kafka

**制限事項**:
- Cluster: 単一 AZ のみ
- Spread: AZ あたり最大 7 台
- 対応インスタンスタイプに制限あり

**参照:** EC2 Placement Groups
</details>

---

### 問題 9
インフラ管理なしで、Web アプリを自動スケーリング・ロードバランシング・ヘルス監視付きでデプロイしたい場合、どのサービスを使うべきですか？

A. AWS Lambda  
B. Amazon EC2 with Auto Scaling  
C. AWS Elastic Beanstalk  
D. Amazon Lightsail  

<details>
<summary>解答を表示</summary>

**答え: C**

**解説:**
- **AWS Elastic Beanstalk** は PaaS
- コードをアップロードするとデプロイを自動化
- EC2/ALB/Auto Scaling/RDS/監視を自動構成
- Java、.NET、PHP、Node.js、Python、Ruby、Go、Docker など対応

**Elastic Beanstalk の機能**:
- 自動キャパシティプロビジョニング
- ロードバランシング
- Auto Scaling
- ヘルス監視
- プラットフォーム更新
- リソースへの制御権は保持

**デプロイ方式**:
- **All at once**: 最速、ダウンタイムあり
- **Rolling**: 分割展開、一時的に容量減
- **Rolling with additional batch**: フル容量維持
- **Immutable**: 新規インスタンスで安全性高
- **Blue/Green**: URL 交換で切替

**Beanstalk と他サービス比較**:
- **Lambda**: 関数実行向けでアプリ全体向けではない
- **EC2 + Auto Scaling**: 手動設定が多い
- **Lightsail**: シンプルだが拡張性は限定的

**参照:** AWS Elastic Beanstalk、PaaS
</details>

---

### 問題 10
特定 AZ で EC2 のキャパシティを保証予約したいが、長期コミットは避けたいです。どの選択肢が適切ですか？

A. Reserved Instances  
B. Savings Plans  
C. On-Demand Capacity Reservations  
D. Spot Instances  

<details>
<summary>解答を表示</summary>

**答え: C**

**解説:**
- **On-Demand Capacity Reservations** は特定 AZ の容量を予約
- 長期コミット不要（いつでもキャンセル可）
- 使用有無に関わらずオンデマンド料金で課金
- 必要時の容量確保に有効

**キャパシティ選択肢の比較**:

| 選択肢 | コミット | 割引 | キャパシティ保証 |
|--------|------------|----------|-------------------|
| **On-Demand** | なし | なし | なし |
| **Reserved** | 1〜3年 | 最大 72% | あり（リージョン or AZ） |
| **Savings Plans** | 1〜3年 | 最大 66% | なし |
| **Capacity Reservations** | なし | なし | あり（AZ） |
| **Spot** | なし | 最大 90% | なし（中断あり） |

**ユースケース**:
- 災害対策（常時稼働しないが容量予約）
- 重要イベント（例: Black Friday）
- 規制/コンプライアンス要件

**コスト最適化**: Capacity Reservation と Savings Plan を組み合わせる

**参照:** On-Demand Capacity Reservations
</details>

---

### 問題 11
大きなファイルを処理する Lambda 関数が 3 秒でタイムアウトします。何を変更すべきですか？

A. Lambda メモリ割り当てを増やす  
B. Lambda タイムアウト設定を増やす  
C. Lambda レイヤーを使う  
D. EC2 に切り替える  

<details>
<summary>解答を表示</summary>

**答え: B**

**解説:**
- Lambda のデフォルトタイムアウトは 3 秒
- 最大 15 分（900 秒）まで拡張可能
- タイムアウト設定はメモリ設定とは別

**Lambda 設定**:
- **メモリ**: 128 MB 〜 10,240 MB（10 GB）
- **タイムアウト**: 1 秒 〜 900 秒（15 分）
- **CPU はメモリに比例**（1,792 MB = 1 vCPU）
- **一時ストレージ (/tmp)**: 512 MB 〜 10,240 MB

**性能調整の手順**:
1. 長時間処理ではタイムアウトを延長
2. CPU ボトルネックならメモリ増加
3. コード最適化
4. さらに長い処理は非同期パターンを検討

**Lambda が不向きなケース**:
- 15 分超の処理
- GPU 必要
- ステートフルアプリ
- 厳しいリアルタイム要件（コールドスタート影響）

**参照:** Lambda Configuration、Lambda Limits
</details>

---

### 問題 12
コンプライアンス要件のため、専有物理サーバー上で EC2 を実行したい企業があります。どの選択肢を使うべきですか？

A. Dedicated Instances  
B. Dedicated Hosts  
C. Reserved Instances  
D. Spot Instances  

<details>
<summary>解答を表示</summary>

**答え: B**

**解説:**
- **Dedicated Hosts**: 物理サーバーを専有利用
- ソケット数、コア数、ホスト ID の可視性あり
- BYOL（Windows Server、SQL Server）をサポート
- 厳格なコンプライアンス要件に対応
- コストは最も高い

**Dedicated Instances と Dedicated Hosts**:

| 項目 | Dedicated Instances | Dedicated Hosts |
|---------|-------------------|----------------|
| **分離レベル** | インスタンス単位 | 物理サーバー単位 |
| **可視性** | ハードウェア可視性なし | ソケット/コア可視性あり |
| **BYOL** | 非対応 | 対応 |
| **配置制御** | 自動 | 制御可能 |
| **課金** | インスタンス単位 | ホスト単位 |
| **用途** | ソフト要件の分離 | BYOL/厳格コンプライアンス |

**Dedicated Hosts の用途**:
- サーバー固定ライセンス
- 規制対応
- 物理ホスト利用の追跡

**参照:** Dedicated Hosts、Dedicated Instances、BYOL
</details>

---

### 問題 13
Auto Scaling グループに 10 台あります。管理者が手動で 3 台終了しました。次に何が起きますか？

A. Auto Scaling がさらに 3 台終了する  
B. Auto Scaling が 3 台新規起動する  
C. Auto Scaling は何もしない  
D. Auto Scaling がアラートを送信する  

<details>
<summary>解答を表示</summary>

**答え: B**

**解説:**
- Auto Scaling は desired capacity を維持
- 手動/自動で終了しても ASG が補充
- 目標台数を継続的に保つ

**Auto Scaling Group の設定**:
- **Minimum**: 最小台数
- **Desired**: 目標台数
- **Maximum**: 最大台数

**スケーリング動作**:
- スケールアウト: 起動（必要台数が現状より多い）
- スケールイン: 終了（必要台数が現状より少ない）
- 異常インスタンス置換: 終了して再作成
- AZ 間リバランス

**インスタンス保護**:
- 特定インスタンスにスケールイン保護を設定可能
- 保護インスタンスは Auto Scaling による終了を回避
- ただし手動終了は可能

**参照:** Auto Scaling Groups、Desired Capacity
</details>

---

### 問題 14
Lambda を使うバッチ処理で、複数ステップの調整・エラーハンドリング・リトライが必要です。どのサービスを使うべきですか？

A. Amazon SQS  
B. Amazon SNS  
C. AWS Step Functions  
D. Amazon EventBridge  

<details>
<summary>解答を表示</summary>

**答え: C**

**解説:**
- **AWS Step Functions** はサーバーレスワークフローのオーケストレーション
- 複数 Lambda を連携
- エラー処理、リトライ、並列実行を標準搭載
- 視覚的ワークフローデザイナーあり

**Step Functions の機能**:
- **State machine**: 状態遷移でワークフロー定義
- **Error handling**: 例外捕捉とリトライ
- **Parallel execution**: 並列実行
- **Wait states**: 待機
- **Choice states**: 条件分岐
- **Map states**: 配列反復

**ワークフロー種別**:
- **Standard**: 長時間実行（最大1年）、exactly-once
- **Express**: 短時間（5分）、at-least-once、高スループット

**ユースケース**:
- ETL パイプライン
- 注文処理
- 動画処理
- 機械学習ワークフロー

**代替との比較**:
- **SQS**: キュー機能のみ
- **SNS**: Pub/Sub のみ
- **EventBridge**: イベントルーティング中心

**参照:** AWS Step Functions、Serverless Orchestration
</details>

---

### 問題 15
ベースライン CPU 性能を保ちつつ、必要時にバーストしたいアプリがあります。どの EC2 インスタンスタイプが適切ですか？

A. M5（汎用）  
B. C5（コンピュート最適化）  
C. T3（バースト性能）  
D. R5（メモリ最適化）  

<details>
<summary>解答を表示</summary>

**答え: C**

**解説:**
- **T3/T4g** はバースト可能インスタンス
- ベースライン CPU + クレジット方式
- ベースライン以下でクレジット蓄積
- ベースライン超過時にクレジット消費でバースト
- 変動負荷でコスト効率が高い

**T3 CPU クレジット**:
- ベースラインはサイズ依存
- CPU < ベースラインで獲得
- CPU > ベースラインで消費
- **Unlimited モード**: クレジット超過バースト可（追加料金）

**T3 ベースライン例**:
- t3.nano: 5%
- t3.micro: 10%
- t3.small: 20%
- t3.medium: 20%
- t3.large: 30%

**ユースケース**:
- Web サーバー、開発/テスト
- 小規模 DB
- コードリポジトリ
- マイクロサービス

**T3 が不向きなケース**:
- 高 CPU 常時利用
- 予測可能な高性能要求
- 持続性能は C5/M5 が適切

**参照:** T3 Instances、Burstable Performance、CPU Credits
</details>

---

### 問題 16
Kubernetes のコントロールプレーンを管理せずにコンテナアプリをデプロイしたい場合、どのサービスを使うべきですか？

A. Amazon ECS  
B. Amazon EKS  
C. AWS Fargate  
D. AWS Elastic Beanstalk  

<details>
<summary>解答を表示</summary>

**答え: B**

**解説:**
- **Amazon EKS** はマネージド Kubernetes
- Kubernetes コントロールプレーンを AWS が管理
- kubectl/Helm など標準ツールと互換
- 複数 AZ の高可用性コントロールプレーン

**EKS の特徴**:
- 管理対象 K8s コントロールプレーン（etcd、API サーバー）
- 自動アップグレード/パッチ
- IAM/VPC/ALB/CloudWatch と統合
- CNCF 認定（標準 Kubernetes）

**EKS ワーカーノードの選択肢**:
- **Self-managed nodes**: EC2 を自前管理
- **Managed node groups**: EC2 ライフサイクルを AWS 管理
- **Fargate**: ノード管理不要

**ECS vs EKS**:
- **ECS**: AWS 独自、シンプル、AWS 連携強い
- **EKS**: 標準 Kubernetes、可搬性高いが複雑

**EKS を使う場面**:
- 既存 Kubernetes ワークロード
- チームに Kubernetes 経験がある
- マルチクラウド/ハイブリッド要件
- 標準 K8s ツールが必要

**参照:** Amazon EKS、Kubernetes on AWS
</details>

---

### 問題 17
Auto Scaling グループで、新規起動インスタンスがトラフィック受信前に準備完了していることを担保したいです。何を設定すべきですか？

A. EC2 Status Checks  
B. ELB Health Checks  
C. Auto Scaling Health Check Grace Period  
D. CloudWatch Alarms  

<details>
<summary>解答を表示</summary>

**答え: C**

**解説:**
- **Health Check Grace Period** は起動直後の準備時間を確保
- 初期化中の早期終了を防止
- デフォルトは 300 秒（5 分）
- アプリ起動時間より長く設定する

**Auto Scaling のヘルスチェック**:
- **EC2**: インスタンス稼働状態（既定）
- **ELB**: ロードバランサーのヘルスチェック合格
- Grace period は両方に適用

**推奨設定**:
1. Grace period を起動時間より長くする
2. ELB ヘルスチェックを有効化
3. 間隔・しきい値を適切化

**例**:
```bash
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name my-asg \
  --health-check-type ELB \
  --health-check-grace-period 600  # 10 minutes
```

**参照:** Auto Scaling Health Checks、Grace Period
</details>

---

### 問題 18
Windows アプリをリファクタリングせずにリフト＆シフトで AWS へ移行したい企業があります。最も適切なコンピュート選択肢はどれですか？

A. AWS Lambda  
B. Amazon ECS  
C. Amazon EC2  
D. AWS Fargate  

<details>
<summary>解答を表示</summary>

**答え: C**

**解説:**
- **Amazon EC2** はリフト＆シフトに最適
- Windows Server 上で既存アプリをそのまま実行可能
- リファクタリングを最小化
- OS と設定をフル制御

**移行戦略（6R）**:
1. **Rehost**: そのまま移行（Lift-and-Shift）
2. **Replatform**: 小規模最適化
3. **Repurchase**: SaaS へ置換
4. **Refactor/Re-architect**: クラウドネイティブ再設計
5. **Retire**: 廃止
6. **Retain**: オンプレ継続

**Lift-and-Shift の特徴**:
- 迅速に移行できる
- リスクが低い
- 後で最適化可能
- AWS Application Migration Service (MGN) を活用

**他選択肢が不向きな理由**:
- Lambda: コード変更が必要
- ECS/Fargate: コンテナ化が必要
- 「現状維持移行」には EC2 が最適

**参照:** Migration Strategies、EC2 for Windows
</details>

---

### 問題 19
S3 イベントで起動する Lambda アプリがあります。ピーク時に何千ものファイルが同時アップロードされます。Lambda はどう処理すべきですか？

A. Lambda メモリを増やす  
B. Lambda の予約同時実行を有効化  
C. Lambda は自動的に同時実行スケールする  
D. Step Functions を使う  

<details>
<summary>解答を表示</summary>

**答え: C**

**解説:**
- Lambda は同時実行を自動スケール
- 各 S3 イベントごとに別インボケーション
- アカウント同時実行デフォルトは 1000（引き上げ可）
- 基本的なスケールでは追加設定不要

**Lambda 同時実行の種類**:
- **Account concurrency**: 全関数合計（既定 1000）
- **Reserved concurrency**: 関数専用上限/確保
- **Provisioned concurrency**: 事前初期化（コールドスタート低減）

**Reserved concurrency を使う場面**:
- 下流システムを保護するため上限設定
- 重要関数に実行枠を確保
- 1 関数が全体同時実行を使い切るのを防止

**例**:
- 1000 ファイル同時アップロード
- 同時に 1000 回起動（上限内）
- 上限超過分はスロットリング（429）

**参照:** Lambda Concurrency、Automatic Scaling
</details>

---

### 問題 20
Web アプリでセッション状態を維持する必要があります。アプリは ALB 配下の複数 EC2 で動作しています。セッション状態はどう管理すべきですか？

A. EC2 のローカルストレージに保存する  
B. ALB のスティッキーセッションを有効化する  
C. Amazon ElastiCache または DynamoDB に保存する  
D. ALB ではなく NLB を使う  

<details>
<summary>解答を表示</summary>

**答え: C**

**解説:**
- **外部セッションストア**（ElastiCache/DynamoDB）がベストプラクティス
- どのインスタンスからもセッション参照可能
- インスタンス障害でもセッション継続
- 真のステートレス設計を実現

**セッション管理の選択肢**:

| 方式 | 長所 | 短所 |
|--------|------|------|
| **ElastiCache/DynamoDB** | ステートレス・拡張性・耐障害性 | 追加サービスが必要 |
| **Sticky Sessions** | 実装が簡単 | 負荷偏り・耐障害性低い |
| **Local Storage** | 高速 | 障害時に消失 |

**ElastiCache/DynamoDB が優れる理由**:
- ✅ どのインスタンスでも処理可能
- ✅ Auto Scaling と相性が良い
- ✅ 障害時にもセッション損失を回避
- ✅ 負荷分散が均等化

**Sticky Sessions の課題**:
- インスタンス利用率の偏り
- 新規インスタンスにトラフィックが流れにくい
- 障害時にセッション喪失
- リファクタリング不可の場合の暫定策

**参照:** Stateless Architecture、Session Management、ElastiCache
</details>

---

### 問題 21
厳格なデータ所在地要件のため、自社データセンター内で AWS サービスを実行しつつ、ハイブリッドクラウドとして一貫した運用体験を維持したい企業があります。どの AWS サービスを使うべきですか？

A. AWS Outposts  
B. Amazon EC2 Dedicated Hosts  
C. AWS Snowball Edge  
D. AWS Direct Connect  

<details>
<summary>解答を表示</summary>

**答え: A**

**解説:**
- AWS Outposts はオンプレミスに AWS ネイティブのサービス/インフラ/運用モデルを提供
- 一貫したハイブリッド体験を実現
- EC2、EBS、RDS、ECS、EKS、S3 などをサポート
- Dedicated Hosts はコンプライアンス分離用途
- Snowball Edge はエッジ計算やデータ転送用途
- Direct Connect はネットワーク接続用途

**参照:** AWS Outposts、Hybrid Cloud
</details>

---

### 問題 22
研究機関が大規模・高スループットのバッチジョブを AWS 上で実行し、ジョブスケジューリングとリソースプロビジョニングを自動化したいです。どのサービスを使うべきですか？

A. AWS Batch  
B. Amazon EC2 Auto Scaling  
C. AWS Lambda  
D. Amazon EMR  

<details>
<summary>解答を表示</summary>

**答え: A**

**解説:**
- AWS Batch はフルマネージドなバッチコンピューティングサービス
- コンピュートリソースの調達とジョブスケジューリングを自動化
- Docker と EC2/Spot/Fargate をサポート
- EC2 Auto Scaling はインスタンス拡張向けでジョブ管理は弱い
- Lambda は短時間イベント駆動向け
- EMR はビッグデータ処理向け

**参照:** AWS Batch、Batch Computing
</details>

---

### 問題 23
AWS やサードパーティが公開したサーバーレスアプリケーションを共有・デプロイしたい開発チームがあります。どのサービスを使うべきですか？

A. AWS Serverless Application Repository  
B. AWS Marketplace  
C. AWS Lambda Layers  
D. AWS CloudFormation StackSets  

<details>
<summary>解答を表示</summary>

**答え: A**

**解説:**
- AWS Serverless Application Repository はサーバーレスアプリ向けの管理リポジトリ
- Lambda ベースアプリの共有/デプロイが可能
- Marketplace は商用ソフト流通向け
- Lambda Layers はコード共有向け
- StackSets はマルチアカウント展開向け

**参照:** AWS Serverless Application Repository、Serverless Deployment
</details>

---

### 問題 24
VMware ツールをそのまま使って統合運用しながら、AWS 上で VMware ワークロードを実行したい企業があります。どのソリューションを使うべきですか？

A. VMware Cloud on AWS  
B. AWS Outposts  
C. Amazon EC2 Dedicated Hosts  
D. AWS Snowball Edge  

<details>
<summary>解答を表示</summary>

**答え: A**

**解説:**
- VMware Cloud on AWS は vSphere/NSX/vSAN と AWS インフラを統合
- シームレスな移行とハイブリッド運用を実現
- AWS と VMware が共同でマネージド提供
- Outposts は AWS サービスのオンプレ展開
- Dedicated Hosts はコンプライアンス用途
- Snowball Edge はエッジ用途

**参照:** VMware Cloud on AWS、Hybrid VMware
</details>

---

### 問題 25
モバイルネットワークのエッジに AWS の計算/ストレージを配置し、超低レイテンシのアプリを提供したいグローバル企業があります。どの AWS サービスを使うべきですか？

A. AWS Wavelength  
B. AWS Outposts  
C. Amazon CloudFront  
D. AWS Direct Connect  

<details>
<summary>解答を表示</summary>

**答え: A**

**解説:**
- AWS Wavelength は 5G ネットワークのエッジへ AWS サービスを提供
- モバイル/エッジ向け超低レイテンシを実現
- Outposts はオンプレ DC 向け
- CloudFront は CDN
- Direct Connect は専用線接続

**参照:** AWS Wavelength、Edge Computing
</details>

---

### 問題 26
AWS 外の自社インフラで ECS/EKS ワークロードを実行し、AWS Console から管理したい企業があります。どのサービスを使うべきですか？

A. Amazon ECS Anywhere と Amazon EKS Anywhere  
B. AWS Outposts  
C. AWS Fargate  
D. Amazon EC2 Dedicated Hosts  

<details>
<summary>解答を表示</summary>

**答え: A**

**解説:**
- ECS Anywhere / EKS Anywhere はオンプレ基盤にコンテナ運用を拡張
- AWS Console から管理できる
- Outposts は AWS インフラ自体をオンプレ配置
- Fargate は AWS 内サーバーレス実行
- Dedicated Hosts はコンプライアンス用途

**参照:** ECS Anywhere、EKS Anywhere、Hybrid Containers
</details>

---

### 問題 27
アプリ向けに、Apache Cassandra 互換のマネージドでスケーラブルなデータベースが必要です。どの AWS サービスを使うべきですか？

A. Amazon Keyspaces  
B. Amazon DynamoDB  
C. Amazon RDS for PostgreSQL  
D. Amazon Aurora  

<details>
<summary>解答を表示</summary>

**答え: A**

**解説:**
- Amazon Keyspaces は Cassandra 互換のマネージド DB
- CQL（Cassandra Query Language）をサポート
- DynamoDB は NoSQL だが Cassandra 互換ではない
- RDS/Aurora はリレーショナル DB

**参照:** Amazon Keyspaces、Managed Cassandra
</details>

---

### 問題 28
金融機関が、完全マネージドで不変かつ暗号学的に検証可能な台帳データベースを必要としています。どの AWS サービスを使うべきですか？

A. Amazon QLDB  
B. Amazon Aurora  
C. Amazon RDS  
D. Amazon DynamoDB  

<details>
<summary>解答を表示</summary>

**答え: A**

**解説:**
- Amazon QLDB（Quantum Ledger Database）はマネージド台帳 DB
- 不変で暗号学的検証可能なトランザクションログを提供
- Aurora/RDS/DynamoDB は台帳 DB ではない

**参照:** Amazon QLDB、Ledger Database
</details>

---

## まとめ

**総問題数**: 28  
