# AWS Fundamentals - 練習問題

> **⚠️ 免責事項**: これらは、AWS認定資格試験の公式問題ではなく、AWS公式ドキュメントに基づいて教育目的で作成された **独自の練習問題** です。

## 試験形式の問題 (SAA-C03)

---

### 問題 1
ある企業が AWS 上に新しいアプリケーションをデプロイし、最高レベルの可用性とフォールトトレランスを確保する必要があります。アプリケーションはデータセンター全体が障害を起こした場合でも動作を継続する必要があります。使用すべき AWS インフラストラクチャコンポーネントはどれですか？

A. 複数の Edge Locations にデプロイする  
B. 単一リージョン内の複数の Availability Zones にデプロイする  
C. 複数のリージョンにデプロイする  
D. AWS Local Zones を使用してデプロイする  

<details>
<summary>回答を表示</summary>

**正解: B**

**説明:**
- Availability Zones (AZ) はリージョン内の独立したデータセンターです
- 複数の AZ にデプロイすることで、データセンター障害から保護されます
- これはリージョン内での高可用性の標準的なアプローチです
- オプション C (複数リージョン) は災害復旧用であり、データセンター障害への対応ではありません
- Edge Locations はコンテンツ配信用であり、アプリケーションホスティングには向きません
- Local Zones は超低遅延を提供しますが、同じレベルのフォールトトレランスは提供しません

**参考資料:** AWS Global Infrastructure、Well-Architected Framework - Reliability Pillar
</details>

---

### 問題 2
ソリューションアーキテクトは、グローバルに静的コンテンツにアクセスするユーザーの遅延を最小化するソリューションを設計する必要があります。使用すべき AWS サービスの組み合わせはどれですか？

A. Cross-Region Replication を使用した Amazon S3  
B. オリジンとして S3 を使用した Amazon CloudFront  
C. Transfer Acceleration を使用した Amazon S3  
D. 複数リージョン内の複数の EC2 インスタンス  

<details>
<summary>回答を表示</summary>

**正解: B**

**説明:**
- CloudFront は AWS の Content Delivery Network (CDN) で、グローバルに 400+ エッジロケーションを持っています
- コンテンツをユーザーの近くにキャッシュし、遅延を最小化します
- S3 が静的コンテンツのオリジンとなります
- オプション A は冗長性を提供しますが、遅延を最適化しません
- オプション C はアップロードを高速化しますが、ダウンロードではありません
- オプション D はコスト効率が悪く、管理が複雑です

**参考資料:** CloudFront、Edge Locations、Global Infrastructure
</details>

---

### 問題 3
AWS Shared Responsibility Model に従うと、以下のうち AWS の責務はどれですか？

A. S3 のデータ保存時の暗号化  
B. EC2 インスタンス上のゲスト OS のパッチ管理  
C. データセンターの物理的セキュリティ  
D. セキュリティグループの設定  

<details>
<summary>回答を表示</summary>

**正解: C**

**説明:**
- AWS は「クラウドの Security OF」（物理インフラストラクチャ）の責任を担います
- これにはデータセンター、ハードウェア、施設が含まれます
- お客様は「クラウド IN の Security」の責任を担います
- オプション A、B、D はすべてお客様の責務です
- お客様は暗号化するかどうか、OS のパッチを当てるかどうか、セキュリティを設定するかどうかを選択します

**参考資料:** AWS Shared Responsibility Model
</details>

---

### 問題 4
スタートアップ企業は、サーバー、オペレーティングシステム、ランタイム環境を管理せずにアプリケーションをデプロイしたいと考えています。どの AWS サービスカテゴリが最も適しています？

A. Infrastructure as a Service (IaaS)  
B. Platform as a Service (PaaS)  
C. Software as a Service (SaaS)  
D. Function as a Service (FaaS)  

<details>
<summary>回答を表示</summary>

**正解: B**

**説明:**
- PaaS サービス（Elastic Beanstalk など）はインフラストラクチャ管理を抽象化します
- ユーザーはサーバーや OS を管理せずにコードをデプロイします
- IaaS（EC2 など）は仮想マシンの管理が必要です
- SaaS は完全に管理されたアプリケーション（Amazon Chime など）です
- FaaS（Lambda など）はイベント駆動ですが、通常は完全なアプリケーションには使用されません
- PaaS がインフラストラクチャ管理なしでアプリケーションをデプロイするのに最適です

**参考資料:** AWS Service Categories、Cloud Computing Models
</details>

---

### 問題 5
企業は、AWS に保存されたデータが特定の地理的位置から出ないようにするという規制要件があります。これはどのように実現できますか？

A. AWS GuardDuty を有効にする  
B. 適切な AWS リージョンを選択し、クロスリージョンフィーチャーを有効にしない  
C. AWS Organizations で Service Control Policies を使用する  
D. AWS CloudTrail を有効にする  

<details>
<summary>回答を表示</summary>

**正解: B**

**説明:**
- AWS リージョンのデータは、明示的に設定しない限りそのリージョン内にとどまります
- クロスリージョンレプリケーション、バックアップ、データ転送を設定しないことで、データ所在地が保証されます
- GuardDuty は脅威検出用であり、データ所在地のためではありません
- SCP はポリシーを強制できますが、重要な制御は適切なリージョン選択です
- CloudTrail はロギング用であり、データ所在地用ではありません
- 主な制御: リージョンを選択し、クロスリージョンサービスを設定しません

**参考資料:** AWS Regions、Data Sovereignty、Compliance
</details>

---

### 問題 6
AWS Well-Architected Framework のどの柱が、障害からの復旧と動的なコンピューティングリソースの取得の能力に焦点を当てています？

A. Operational Excellence  
B. Security  
C. Reliability  
D. Performance Efficiency  

<details>
<summary>回答を表示</summary>

**正解: C**

**説明:**
- Reliability 柱は、障害からの復旧とワークロード機能の維持に焦点を当てています
- 重要な側面: フォールトトレランス、災害復旧、自己修復
- Operational Excellence は運用と監視に焦点を当てています
- Security は情報とシステムの保護に焦点を当てています
- Performance Efficiency はリソースの効率的な使用に焦点を当てています

**参考資料:** AWS Well-Architected Framework - Reliability Pillar
</details>

---

### 問題 7
企業は、デプロイ前に計画された AWS インフラストラクチャのコストを推定したいと考えています。どの AWS ツールを使用すべきですか？

A. AWS Cost Explorer  
B. AWS Budgets  
C. AWS Pricing Calculator  
D. AWS Cost and Usage Report  

<details>
<summary>回答を表示</summary>

**正解: C**

**説明:**
- AWS Pricing Calculator は計画されたアーキテクチャのコストを推定します
- Cost Explorer は既存/過去のコストを分析します
- AWS Budgets はバジェットアラートを設定します
- Cost and Usage Report は詳細な請求データを提供します
- デプロイ前の推定には Pricing Calculator が正しい選択です

**参考資料:** AWS Pricing Tools、Cost Management
</details>

---

### 問題 8
アプリケーションは特定の都市圏内のユーザーに対して 15 ミリ秒以下の遅延が必要です。どの AWS インフラストラクチャコンポーネントを使用すべきですか？

A. AWS Region  
B. Availability Zone  
C. AWS Local Zone  
D. AWS Wavelength Zone  

<details>
<summary>回答を表示</summary>

**正解: C**

**説明:**
- AWS Local Zones はコンピューティング、ストレージ、データベースをエンドユーザーの近くに配置します
- 単一桁のミリ秒の遅延要件に対応するように設計されています
- 都市圏に配置されており、超低遅延アプリケーション向けです
- Wavelength Zones は 5G エッジコンピューティング向けです
- 通常のリージョン/AZ は超低遅延要件を満たさない場合があります

**参考資料:** AWS Local Zones、Global Infrastructure
</details>

---

### 問題 9
AWS 内で複数の AWS アカウントを管理するための統合インターフェイスを提供するサービスはどれですか？

A. AWS IAM  
B. AWS Organizations  
C. AWS Control Tower  
D. AWS Systems Manager  

<details>
<summary>回答を表示</summary>

**正解: B**

**説明:**
- AWS Organizations は複数の AWS アカウントを集中管理します
- 統合請求、アカウント作成、ポリシー管理を提供します
- IAM はアカウント内のアクセス許可を管理します
- Control Tower は複数アカウント環境をセットアップします（Organizations を使用しています）
- Systems Manager は AWS リソースを管理しており、アカウントではありません
- マルチアカウント管理の直接的な回答: Organizations

**参考資料:** AWS Organizations、Account Management
</details>

---

### 問題 10
AWS Well-Architected Framework に従うと、Security 柱に対して推奨される設計原則はどれですか？

A. 強い認証基盤を実装する  
B. 数分でグローバルに展開する  
C. データセンター運用の支出を停止する  
D. フィードバックループを実装する  

<details>
<summary>回答を表示</summary>

**正解: A**

**説明:**
- 「強い認証基盤を実装する」は Security 柱の原則です
- 最小権限、職務分離、集約された認証管理が含まれます
- オプション B はグローバルデプロイメント（一般的な AWS の利点）に関連しています
- オプション C はクラウドの利点であり、セキュリティの原則ではありません
- オプション D は Operational Excellence 柱に関連しています

**参考資料:** AWS Well-Architected Framework - Security Pillar Design Principles
</details>

---

### 問題 11
企業は AWS CLI を使用してリソースを管理したいですが、長期の認証情報をスクリプトに埋め込みたくありません。最良のプラクティスは何ですか？

A. ルートアカウント認証情報を使用する  
B. IAM ユーザーを作成し、認証情報をスクリプトに保存する  
C. 一時的なセキュリティ認証情報を持つ IAM ロールを使用する  
D. アクセスキーをシークレットキーなしで使用する  

<details>
<summary>回答を表示</summary>

**正解: C**

**説明:**
- IAM ロールは AWS STS を使用して一時的なセキュリティ認証情報を提供します
- 認証情報は自動的にローテーションし、セキュリティを強化します
- 日常的なタスクにはルートアカウント認証情報を使用しないでください
- 認証情報をスクリプトにハードコードしないでください
- アクセスキーには常にシークレットキーが必要です
- 最良のプラクティス: 一時的な認証情報を持つ IAM ロール

**参考資料:** IAM Best Practices、AWS CLI Security
</details>

---

### 問題 12
以下のうち、AWS リージョンを使用することの利点はどれですか？（2 つ選択）

A. 特定の地理的地域のユーザーの遅延を低減する  
B. すべてのリージョン間での自動データレプリケーション  
C. データ主権要件への準拠  
D. 単一リージョンの使用と比較してコストが低い  
E. リージョン間の自動フェイルオーバー  

<details>
<summary>回答を表示</summary>

**正解: A、C**

**説明:**
- **A が正解**: リージョン内にデプロイすることで、ユーザーに近い場所でレイテンシが低減されます
- **C が正解**: リージョンによってデータ所在地/主権要件を満たすことができます
- B が誤り: レプリケーションは自動ではなく、設定が必要です
- D が誤り: 複数リージョンは通常、コストが増加します
- E が誤り: フェイルオーバーは自動ではなく、アーキテクチャ設計が必要です

**参考資料:** AWS Regions、Global Infrastructure Benefits
</details>

---

### 問題 13
ソリューションアーキテクトは、Well-Architected Framework の「Design for Failure」原則に従うシステムを設計する必要があります。どのようなアプローチを取るべきですか？

A. 障害を防ぐために最大の EC2 インスタンスタイプのみを使用する  
B. アプリケーションをコンポーネント障害に適切に対応するように設計する  
C. シンプルなので、すべてのリソースを単一の Availability Zone にデプロイする  
D. すべての障害を AWS Support に依存する  

<details>
<summary>回答を表示</summary>

**正解: B**

**説明:**
- 「Design for Failure」とは、コンポーネントが障害を起こすと想定し、それに応じて計画することを意味します
- アプリケーションはリトライロジック、ヘルスチェックなどで障害に適切に対応する必要があります
- より大きなインスタンスは障害を防ぎません
- 単一 AZ デプロイは障害リスクを増加させます
- サポートのみに依存せず、障害に対応する設計が必要です
- 適切なアプローチ: グレースフルデグラデーション、自動復旧

**参考資料:** AWS Well-Architected Framework - Reliability Pillar
</details>

---

### 問題 14
AWS Edge Locations に関する正しいステートメントはどれですか？

A. Edge Locations は CloudFront コンテンツ配信にのみ使用される  
B. Edge Locations は Regions より数が少ない  
C. Edge Locations はコンテンツ配信とエッジコンピューティング両方に使用できる  
D. Edge Locations は Availability Zones と同じ  

<details>
<summary>回答を表示</summary>

**正解: C**

**説明:**
- Edge Locations は CloudFront（CDN）、Lambda@Edge、その他のエッジサービスをサポートします
- 400以上のエッジロケーション vs 30以上のリージョンが存在します
- Edge Locations ≠ Availability Zones（異なる目的です）
- コンテンツ配信とエッジコンピューティング（Lambda@Edge、CloudFront Functions）の両方に使用されます

**参考資料:** AWS Global Infrastructure - Edge Locations
</details>

---

### 問題 15
企業は月間 AWS コストが閾値を超えたときにアラートを受け取りたいです。どのサービスを使用すべきですか？

A. AWS Cost Explorer  
B. AWS Budgets  
C. AWS Pricing Calculator  
D. AWS Trusted Advisor  

<details>
<summary>回答を表示</summary>

**正解: B**

**説明:**
- AWS Budgets はカスタムコスト/使用量バジェットをアラートと共に設定できます
- 閾値を超えたときに SNS 経由で通知を送信できます
- Cost Explorer は過去のコストを可視化しますがアラートを送信しません
- Pricing Calculator は将来のコストを推定します
- Trusted Advisor はベストプラクティスの推奨事項を提供します
- 閾値アラート向け: AWS Budgets

**参考資料:** AWS Budgets、Cost Management
</details>

---

### 問題 16
AWS Shared Responsibility Model に従うと、EC2 インスタンスの基盤となるハイパーバイザーのパッチ管理は誰の責務ですか？

A. お客様  
B. AWS  
C. AWS とお客様の両方  
D. サードパーティベンダー  

<details>
<summary>回答を表示</summary>

**正解: B**

**説明:**
- AWS はハイパーバイザーレイヤー（クラウドの Security OF）を管理します
- お客様はゲスト OS のパッチを管理します（クラウド IN の Security）
- ハイパーバイザーはインフラストラクチャで、AWS の責務です
- お客様は OS、アプリケーション、データ暗号化のパッチを当てます

**参考資料:** AWS Shared Responsibility Model - EC2
</details>

---

### 問題 17
企業は 5G モバイルユーザーに近づいた場所にアプリケーションをデプロイし、超低遅延を実現したいと考えています。どの AWS サービスを使用すべきですか？

A. AWS Local Zones  
B. AWS Wavelength  
C. AWS Outposts  
D. AWS Direct Connect  

<details>
<summary>回答を表示</summary>

**正解: B**

**説明:**
- AWS Wavelength は 5G ネットワークエッジにコンピューティングを埋め込みます
- モバイルデバイスに対して単一桁ミリ秒の遅延を提供します
- Local Zones は都市圏向けですが 5G 専用ではありません
- Outposts はオンプレミス AWS インフラストラクチャ向けです
- Direct Connect は専用ネットワーク接続です
- 5G モバイル向け: Wavelength

**参考資料:** AWS Wavelength、Edge Computing
</details>

---

### 問題 18
AWS Well-Architected Framework のどの柱に「キャパシティニーズを推測するのをやめる」という原則が含まれていますか？

A. Cost Optimization  
B. Performance Efficiency  
C. Reliability  
D. Operational Excellence  

<details>
<summary>回答を表示</summary>

**正解: B**

**説明:**
- Performance Efficiency 柱はキャパシティプランニングの原則を含みます
- 「キャパシティをやめる」- Auto Scaling とエラスティックサービスを使用します
- 適切なサイジングと動的キャパシティ調整を有効にします
- Cost Optimization はムダの排除に焦点を当てています
- Reliability は復旧とテストに焦点を当てています
- Operational Excellence は実行と監視に焦点を当てています

**参考資料:** AWS Well-Architected Framework - Performance Efficiency Pillar
</details>

---

### 問題 19
企業は複数の開発チーム向けに独立した AWS アカウントが必要で、統合請求を望んでいます。どのサービスを実装すべきですか？

A. AWS Control Tower  
B. 統合請求を使用した AWS Organizations  
C. 単一アカウント内の複数の IAM ユーザー  
D. AWS Resource Groups  

<details>
<summary>回答を表示</summary>

**正解: B**

**説明:**
- AWS Organizations は複数アカウント間の統合請求を提供します
- 各チームは独立したアカウントでリソース分離されます
- 組織全体に対して単一の請求書
- Control Tower は Organizations をセットアップするのに役立ちますが、Organizations が直接的な回答です
- IAM ユーザーはアカウントレベルの分離を提供しません
- Resource Groups はリソースを整理しますが、請求ではありません

**参考資料:** AWS Organizations、Consolidated Billing
</details>

---

### 問題 20
AWS リージョン内の Availability Zones の主な目的は何ですか？

A. 異なる価格階層を提供する  
B. フォールトトレランスと高可用性を有効にする  
C. 異なる AWS サービスをサポートする  
D. データ転送コストを削減する  

<details>
<summary>回答を表示</summary>

**正解: B**

**説明:**
- Availability Zones はリージョン内の独立した場所です
- 主な目的: フォールトトレランスと高可用性
- 各 AZ は独立した電源、冷却、ネットワークを持ちます
- AZ 間でのデプロイは単一障害点から保護します
- 価格、サービス可用性、コスト削減向けではありません
- 中核的な目的: レジリエンスと可用性

**参考資料:** AWS Availability Zones、High Availability Architecture
</details>

---

### 問題 21
企業は 50 の AWS アカウントを持ち、請求を一元管理し、組織全体のセキュリティポリシーを適用したいです。どの AWS サービスを使用すべきですか？

A. AWS IAM  
B. AWS Organizations  
C. AWS Control Tower  
D. AWS Systems Manager  

<details>
<summary>回答を表示</summary>

**正解: B**

**説明:**
- **AWS Organizations** は複数の AWS アカウント管理用のサービスです
- すべてのアカウント間での統合請求を提供します
- Organization 全体のセキュリティポリシー（SCP）を有効にします
- Organizational Units (OU) で論理グループ化ができます
- Control Tower (C) は Organizations の上に構築されていますが、自動セットアップ向けです
- IAM (A) は単一アカウント内のユーザー/ロール管理です
- Systems Manager (D) は運用向けで、マルチアカウント管理ではありません

**参考資料:** AWS Organizations、Multi-Account Strategy
</details>

---

### 問題 22
セキュリティチームは、AWS Organization の全メンバーアカウントが us-east-1 と eu-west-1 以外のリージョンでリソースを作成するのを防ぎたいです。どのように実装すべきですか？

A. 各アカウントで IAM ポリシーを作成し、リージョンを制限する  
B. AWS Config ルールを使用して非準拠なリソースを検出する  
C. 他のリージョンでのアクション拒否する Service Control Policy（SCP）を作成する  
D. AWS Firewall Manager を使用してリージョンアクセスをブロックする  

<details>
<summary>回答を表示</summary>

**正解: C**

**説明:**
- **Service Control Policies（SCP）** は集約された予防制御を提供します
- SCP はどの AWS リージョンを使用できるかを制限できます
- Organization、OU、またはアカウントレベルで適用されます
- メンバーアカウントのユーザーでは上書きできません
- IAM ポリシー (A) はアカウント管理者が変更できます
- Config ルール (B) は検出用で、予防的ではありません
- Firewall Manager (D) はセキュリティグループ/WAF ルール向けで、リージョン制限ではありません

**SCP の例:**
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Deny",
    "Action": "*",
    "Resource": "*",
    "Condition": {
      "StringNotEquals": {
        "aws:RequestedRegion": ["us-east-1", "eu-west-1"]
      }
    }
  }]
}
```

**参考資料:** Service Control Policies、AWS Organizations、Region Restrictions
</details>

---

### 問題 23
Service Control Policies（SCP）に関する正しいステートメントはどれですか？

A. SCP はユーザーとロールにアクセス許可を付与します  
B. SCP は AWS Organization のマネジメントアカウントに影響します  
C. SCP はメンバーアカウントの最大アクセス許可を定義します  
D. SCP はアカウント単位でのみ適用でき、OU には適用できません  

<details>
<summary>回答を表示</summary>

**正解: C**

**説明:**
- **SCP は最大アクセス許可を定義します** - ガードレールとして機能します
- SCP はアクセス許可を付与しません (A は誤り)
- ガードレールとしてのみ機能し、何を制限するかのみを指定します
- SCP はマネジメントアカウントに影響しません (B は誤り)
- SCP は Organization ルート、OU、またはアカウントに適用できます (D は誤り)
- 有効なアクセス許可 = IAM ポリシー AND SCP

**SCP のルール:**
- ❌ アクセス許可を付与しません
- ❌ マネジメントアカウントに影響しません
- ✅ 最大アクセス許可境界を設定します
- ✅ OU に適用できます
- ✅ 階層を下に継承されます

**参考資料:** Service Control Policies、Permission Boundaries
</details>

---

### 問題 24
企業は、ベストプラクティスに従った安全なマルチアカウント AWS 環境を迅速にセットアップし、自動化されたアカウントプロビジョニングと事前設定されたガバナンスガードレールが必要です。どのサービスを使用すべきですか？

A. AWS Organizations  
B. AWS Control Tower  
C. AWS CloudFormation StackSets  
D. AWS Service Catalog  

<details>
<summary>回答を表示</summary>

**正解: B**

**説明:**
- **AWS Control Tower** は自動化されたマルチアカウントセットアップを提供します
- Landing Zone（ベストプラクティスベースライン）を含みます
- 事前設定されたガードレール（予防的・検出的）を含みます
- アカウントプロビジョニング用の Account Factory
- AWS Organizations (A) は手動セットアップが必要です
- CloudFormation StackSets (C) はテンプレートをデプロイしますが、ガバナンスではありません
- Service Catalog (D) はセルフサービス IT リソース向けです

**Control Tower の機能:**
- ✅ 自動化されたセットアップ（数日対数分）
- ✅ 事前構築ガードレール
- ✅ Account Factory
- ✅ コンプライアンスダッシュボード
- ✅ Organizations、IAM Identity Center、CloudTrail と統合

**使用時期:**
- クイックセットアップとベストプラクティス
- AWS 専門知識が少ない
- 事前構築ガバナンスが必要

**直接 Organizations を使用する時期:**
- 最大の柔軟性が必要
- カスタム要件がある
- 経験豊富な AWS チーム

**参考資料:** AWS Control Tower、Landing Zone、Multi-Account Governance
</details>

---

### 問題 25
企業は一元化されたネットワークアカウントを持ち、複数のアプリケーションアカウントと VPC サブネットを共有したいですが、VPC インフラストラクチャを複製したくありません。どの AWS サービスでこれが可能ですか？

A. VPC Peering  
B. AWS Transit Gateway  
C. AWS Resource Access Manager（RAM）  
D. AWS PrivateLink  

<details>
<summary>回答を表示</summary>

**正解: C**

**説明:**
- **AWS Resource Access Manager（RAM）** はアカウント間でリソースを共有できます
- VPC サブネットをアカウント間で共有できます
- リソースは所有者アカウントに残りますが、共有アカウントがアクセス可能です
- VPC の複製が不要です
- VPC Peering (A) は VPC を接続しますが、サブネットを共有しません
- Transit Gateway (B) はネットワークを接続しますが、サブネットを共有しません
- PrivateLink (D) はサービスから VPC への接続用です

**RAM を使用したサブネット共有の利点:**
- ✅ 一元化されたネットワーク管理
- ✅ VPC スプロール削減
- ✅ 効率的な IP アドレス使用
- ✅ ネットワークアーキテクチャ簡素化
- ✅ 運用オーバーヘッド削減

**RAM 経由で共有可能な他のリソース:**
- VPC サブネット（最も一般的）
- Transit Gateway アタッチメント
- Route 53 Resolver ルール
- License Manager 設定
- Aurora DB クラスター
- プレフィックスリスト

**参考資料:** AWS Resource Access Manager、VPC Subnet Sharing、Centralized Networking
</details>

---

### 問題 26
企業は AWS Organizations で統合請求を使用しています。単一のアカウント S3 ストレージでは適格量に達していないのに、S3 ストレージでボリュームディスカウントを受けています。理由は何ですか？

A. Organizations は自動ディスカウントを提供します  
B. 統合請求は全アカウント間の使用量を組み合わせてボリューム価格を適用します  
C. マネジメントアカウントがすべてのディスカウントを取得します  
D. SCP は自動的にコスト削減を有効にします  

<details>
<summary>回答を表示</summary>

**正解: B**

**説明:**
- **統合請求** は全アカウント間の使用量を組み合わせます
- AWS は Organization 全体を単一請求エンティティとして扱います
- ボリュームディスカウントは組み合わせた使用量に適用されます
- 例: アカウント 1 が 500GB、アカウント 2 が 800GB、アカウント 3 が 700GB の場合、合計 1500GB で高い階層価格が全アカウントに適用されます
- 自動ディスカウント (A) ではなく、使用量の集約です
- マネジメントアカウントのみではなく、すべてのアカウントが利益を得ます (C)
- SCP はアクセス許可向けであり、コスト向けではありません (D)

**統合請求の利点:**
- ✅ アカウント間のボリュームディスカウント
- ✅ 単一支払い方法
- ✅ より簡単なコスト追跡
- ✅ Organization 全体の cost allocation タグ
- ✅ Reserved Instance の共有

**参考資料:** AWS Organizations、Consolidated Billing、Volume Pricing
</details>

---

### 問題 27
どの AWS Organizations フィーチャーにより、アカウントが Organization を離脱することを防止できるポリシーを作成できますか？

A. IAM Policy  
B. Service Control Policy（SCP）  
C. Resource Control Policy  
D. Organizational Lock  

<details>
<summary>回答を表示</summary>

**正解: B**

**説明:**
- **Service Control Policies（SCP）** はアカウントの脱退を防止できます
- SCP は `organizations:LeaveOrganization` アクション拒否できます
- Organization またはOU レベルで適用されます
- メンバーアカウントでは上書きできません

**SCP の例:**
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Deny",
    "Action": "organizations:LeaveOrganization",
    "Resource": "*"
  }]
}
```

**その他の予防的 SCP の使用例:**
- CloudTrail の削除を防止
- パブリック S3 バケットを拒否
- 承認されたインスタンスタイプのみを制限
- 暗号化要件を強制
- ルートユーザーのアクションを制限

**参考資料:** Service Control Policies、Organization Governance
</details>

---

## まとめ

**総問題数**: 27  
**カバーされたトピック**:
- AWS Global Infrastructure（Regions、AZ、Edge Locations、Local Zones、Wavelength）
- AWS Well-Architected Framework（すべての 6 つの柱）
- Shared Responsibility Model
- AWS Account Management（Organizations、Consolidated Billing）
- **Service Control Policies（SCP）** - NEW
- **AWS Control Tower** - NEW
- **AWS Resource Access Manager（RAM）** - NEW
- Cost Management Tools（Pricing Calculator、Budgets、Cost Explorer）
- Service Categories とCloud Models

**試験ヒント**:
1. ✅ Regions、AZ、Edge Locations、Local Zones、Wavelength の違いを理解
2. ✅ Well-Architected Framework の 6 つの柱を暗記
3. ✅ Shared Responsibility Model で AWS とお客様が何を管理するかを知る
4. ✅ **重大**: SCP を理解 - アクセス許可を付与しない、ガードレールのみ
5. ✅ **重大**: SCP はマネジメントアカウントに影響しません
6. ✅ Control Tower 対手動 Organizations セットアップの使用時期を知る
7. ✅ VPC サブネット共有用 RAM を理解（一般的な試験シナリオ）
8. ✅ 統合請求によるアカウント間のボリュームディスカウント
9. ✅ 各コスト管理ツールの使用時期を理解
10. ✅ マルチアカウント管理向け AWS Organizations を知る

**AWS Organizations の重要な試験コンセプト:**
- **マネジメントアカウント**: SCP で制限できず、すべての請求を支払う
- **メンバーアカウント**: SCP が適用される、OU/Organization から継承される
- **Organizational Units（OU）**: 論理グループ化、最大 5 レベルのネスト
- **SCP**: 最大アクセス許可、アクセス許可を付与しない、予防的制御
- **統合請求**: ボリュームディスカウント、単一支払い、RI 共有
- **Control Tower**: 自動化セットアップ、Landing Zone、ガードレール
- **RAM**: リソース共有（特に VPC サブネット）のアカウント間

**一般的な試験シナリオ:**
- 「特定リージョンのサービス使用を防止」→ SCP
- 「50 アカウント間での一元化請求」→ AWS Organizations
- 「ガバナンス付きのクイックマルチアカウントセットアップ」→ Control Tower
- 「アカウント間での VPC サブネット共有」→ RAM
- 「すべてのアカウント間で暗号化を強制」→ SCP
- 「アカウント間でのボリュームディスカウント」→ Consolidated Billing

**次のステップ**:
- 不正解の解答を確認
- README の参照トピックを確認
- シナリオベースの質問を練習
- モジュールクイズで学習を強化
- **焦点**: SCP に時間をかけてください - 頻繁に出題されます！
