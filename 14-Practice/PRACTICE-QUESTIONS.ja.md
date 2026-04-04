# AWS SAA-C03 練習問題

> 弱点分野と試験で頻出のシナリオに特化した練習問題

> **⚠️ 免責事項:** これらは教育目的で作成された**オリジナル練習問題**です。AWS認定試験の実際の問題ではありません。内容はすべて公開されているAWSドキュメントに基づいています。

## 📋 カテゴリ
- [Auto Scaling & 高可用性](#auto-scaling--高可用性)
- [Storage & データ管理](#storage--データ管理)
- [Networking](#networking)
- [Databases](#databases)
- [Serverless & Containers](#serverless--containers)
- [Security](#security)
- [Cost Optimization](#cost-optimization)

---

## Auto Scaling & 高可用性

### 問題 1
CloudWatch アラームが発火しているにもかかわらず、Auto Scaling group が反応しません。最も可能性が高い原因はどれですか？

**選択肢:**
A. CloudWatch metric の設定が誤っている
B. Auto Scaling group が cooldown period 中である ✓
C. ASG の最小サイズが 0
D. ASG の希望サイズが 0

**解説:** Cooldown period は、スケーリングイベント後にメトリクスが安定するまでスケーリングアクションを抑制します。

---

### 問題 2
ASG 内の EC2 instance 1台を、トラフィックを受けずにアップグレードしたいです。どうすべきですか？

**選択肢:**
A. instance を Hibernate する
B. cooldown timer を使う
C. instance を Standby にしてアップグレードし、InService に戻す ✓
D. lifecycle hook を使う

**解説:** Standby は ASG には残したまま load balancing から切り離すため、置き換えを防ぎつつ保守できます。

---

## Storage & データ管理

### 問題 3
S3 で WORM (Write Once Read Many) を実現するにはどうしますか？

**選択肢:**
A. versioning を有効化する
B. bucket 作成時に Object Lock を有効化する ✓
C. S3 Glacier を使う
D. bucket policy を設定する

**解説:** Object Lock は保持期間を使って WORM を強制します。bucket 作成時に有効化が必要です。

---

### 問題 4
S3 object を削除した後（versioning 無効）、list-objects 呼び出しでは直後に何が見えますか？

**選択肢:**
A. object はまだ表示される
B. object は削除済みで表示されない ✓
C. 整合性によって表示されたりされなかったりする
D. object に delete marker が付く

**解説:** S3 は強い整合性を提供するため、削除成功直後に object は一覧から消えます。

---

## Networking

### 問題 5
S3 と DynamoDB へのプライベート接続をコストなしで提供する VPC 機能はどれですか？

**選択肢:**
A. NAT Gateway
B. VPC Peering
C. Gateway Endpoints ✓
D. Interface Endpoints

**解説:** S3/DynamoDB 用の Gateway endpoint は無料で、route table ベースで動作します。

---

### 問題 6
internet-facing ALB へのリクエストが、target が healthy なのに失敗します。何が原因として有力ですか？

**選択肢:**
A. ALB subnet の route table に IGW ルートがない ✓
B. target が public subnet にある
C. ALB に Elastic IP がない
D. cross-zone load balancing が無効

**解説:** ALB は public subnet に配置され、`0.0.0.0/0 → IGW` のルートが必要です。

---

## Databases

### 問題 7
読み取り中心アプリで RDS が CPU 100% に達しています。読み取りスケールに有効なのはどれですか？（3つ選択）

**選択肢:**
A. Read Replicas を追加する ✓
B. Storage Auto Scaling を有効化する
C. SQS でスロットリングする
D. ElastiCache を使う ✓
E. 複数 RDS instance にシャーディングする ✓
F. Multi-AZ を有効化する

**解説:** Read Replica、caching、sharding はいずれも primary への読み取り負荷を減らします。

---

### 問題 8
Multi-AZ RDS deployment が提供するものは何ですか？

**選択肢:**
A. 読み取りスケーリング
B. 高可用性のための自動フェイルオーバー ✓
C. クロスリージョン災害対策
D. コスト最適化

**解説:** Multi-AZ は同期 standby を維持して自動フェイルオーバーを実現します。読み取りスケール目的ではありません。

---

## Serverless & Containers

### 問題 9
Lambda が CloudWatch Logs に書き込むには、どの権限が必要ですか？（3つ選択）

**選択肢:**
A. logs:CreateLogGroup ✓
B. logs:GetLogEvents
C. logs:CreateLogStream ✓
D. logs:DescribeLogStreams
E. logs:PutLogEvents ✓

**解説:** Lambda runtime は log group 作成、log stream 作成、event 書き込みの3権限が必要です。

---

### 問題 10
task レベルの security groups を提供する ECS networking mode はどれですか？

**選択肢:**
A. Host
B. Bridge
C. awsvpc ✓
D. Default

**解説:** awsvpc では各 task に専用 ENI が割り当てられ、task 単位の security groups が使えます。

---

## Security

### 問題 11
S3 bucket へのアクセスを特定 IAM role のみに制限するにはどうしますか？

**選択肢:**
A. 許可 role 以外をすべて明示 deny する bucket policy
B. 許可ユーザー以外をすべて明示 deny する bucket policy
C. 特定 role のみ許可する bucket policy を作成し、ユーザーはその role を assume する ✓
D. NotPrincipal を使う bucket policy

**解説:** bucket へのアクセス権は role に付与し、ユーザーは認証後にその role を assume します。

---

### 問題 12
CloudFront が S3 の private content を配信しています。複数ファイルへのアクセスを許可するにはどうしますか？

**選択肢:**
A. S3 pre-signed URL
B. CloudFront signed URL
C. CloudFront signed cookies ✓
D. CloudFront geo restrictions

**解説:** Signed cookies を使うと URL を変えずに複数 object へのアクセス許可が可能です。

---

## Cost Optimization

### 問題 13
低利用率の EC2 instance を特定できるサービスはどれですか？（2つ選択）

**選択肢:**
A. CloudWatch ✓
B. SNS
C. Trusted Advisor ✓
D. CloudTrail
E. Config

**解説:** CloudWatch metric と Trusted Advisor のチェックで、低利用率 instance を特定できます。

---

### 問題 14
小規模なベンダーチームが VPC 内 database に接続する最もコスト効率の高い方法はどれですか？

**選択肢:**
A. AWS Client VPN ✓
B. Direct Connect
C. VGW への Site-to-Site VPN
D. Transit Gateway への Site-to-Site VPN

**解説:** Client VPN はユーザー単位認証、従量課金、最小限のセットアップで利用できます。

---

## Architecture Patterns

### 問題 15
ビジュアルワークフローと優先度キューを持つ serverless メディア処理パイプラインとして最適なのはどれですか？

**選択肢:**
A. S3 → Lambda → SQS → DynamoDB
B. S3 → Lambda → SWF → DynamoDB
C. S3 → Lambda → process → DynamoDB
D. S3 → Lambda → Step Functions → DynamoDB ✓

**解説:** Step Functions は可視化されたオーケストレーションを提供し、優先度は入口を分けることで制御できます。

---

*各回答を自信を持って説明できるまで、このシナリオを繰り返し練習してください！*
