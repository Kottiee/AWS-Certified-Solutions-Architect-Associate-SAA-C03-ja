# ⚡ Fast Learning - Security & Compliance

> **学習時間の目安**: 60〜75分 | **試験比重**: 約20〜25%  

## 🎯 必須コンセプト（5分）

### Security Service Selector (KSS-WCGI)
```
暗号鍵? → KMS (Key Management Service)
シークレット/パスワード? → Secrets Manager
証明書? → ACM (Certificate Manager)
ファイアウォール? → Security Groups, NACLs, WAF
DDoS対策? → Shield (Standard/Advanced)
コンプライアンス? → CloudTrail, Config, Inspector
アイデンティティ? → IAM, Cognito, Directory Service
検知? → GuardDuty, Macie, Detective
```

**記憶フレーズ**: "Keys Secure Secrets, While Controlling Guarding Identity"

## 📊 クイックリファレンステーブル

### Encryption Services Matrix
| Service | 種類 | 主な用途 | キー管理 |
|---------|------|----------|----------|
| **KMS** | 暗号鍵管理 | AWSリソースの暗号化 | AWS管理 または カスタマー管理 |
| **CloudHSM** | ハードウェアモジュール | 規制対応が必要なワークロード | カスタマーのみ |
| **Secrets Manager** | シークレットの自動ローテーション | DB認証情報、APIキー | 自動ローテーション |
| **Parameter Store** | 設定/シークレット | アプリ設定、シンプルなシークレット | 無料枠あり |
| **ACM** | SSL/TLS証明書 | HTTPS/TLS | AWS管理、自動更新 |

### Security Monitoring Services
| Service | 役割 | 検知対象 | 課金モデル |
|---------|------|----------|------------|
| **GuardDuty** | 脅威検出 | 不審なアクティビティや異常 | 解析したイベント数に基づく |
| **Macie** | データセキュリティ | S3内の機微情報（PIIなど） | スキャンしたGB数 |
| **Inspector** | 脆弱性評価 | EC2/ECRの脆弱性 | 評価実行数 |
| **Detective** | セキュリティインシデント調査 | 根本原因の分析 | 取り込んだGB数 |
| **Security Hub** | セキュリティの統合ダッシュボード | 各サービスの検出結果を集約 | 実行したチェック数 |

## 🔥 試験でよく出るトピック

### 1. KMS Key Types（超重要）
```
AWS MANAGED KEYS
├── 無料
├── 毎年自動ローテーション（必須）
├── 形式: aws/service-name（例: aws/s3）
└── 用途: デフォルト暗号化

CUSTOMER MANAGED KEYS (CMK)
├── 1キーあたり $1/月
├── 任意で自動ローテーション（年1回）
├── キーポリシーをフルコントロール可能
├── 無効化・削除が可能
└── 用途: カスタム要件、コンプライアンス対応

AWS OWNED KEYS
├── 無料
├── 複数アカウントでAWS内部的に使用
├── ユーザー側からは見えず、制御不可
└── 用途: DynamoDBのデフォルト暗号化など
```

**記憶フレーズ**: 「AMC」 = AWS（無料・自動）、Customer（コントロール・コスト）、Owned（中身は見えない）

### 2. データ暗号化の状態
```
ENCRYPTION AT REST（保存データの暗号化）
├── 多くのAWSサービスでKMSを利用
├── S3: SSE-S3, SSE-KMS, SSE-C
├── EBS: 暗号化ボリューム
├── RDS: 作成時に暗号化を有効化
└── DynamoDB: サーバーサイド暗号化

ENCRYPTION IN TRANSIT（転送中データの暗号化）
├── SSL/TLS（HTTPS）
├── VPN（Site-to-Site）
├── Direct Connect + VPN
├── クライアントサイド暗号化
└── 多くのAWSサービスはデフォルトでTLSを使用
```

### 3. WAF, Shield, Firewall Manager
| Service | レイヤー | 目的 | 主なユースケース |
|---------|----------|------|-------------------|
| **WAF** | レイヤー7 | Webアプリケーションファイアウォール | SQLインジェクション、XSS防御 |
| **Shield Standard** | レイヤー3/4 | DDoS保護 | 無料、自動有効 |
| **Shield Advanced** | レイヤー3/4/7 | 強化されたDDoS保護 | 月額約$3,000、24/7サポート |
| **Firewall Manager** | 複数レイヤー | セキュリティポリシーの一元管理 | 複数アカウントのセキュリティ統制 |

**WAFで設定できるルール例**:
- IPアドレス
- HTTPヘッダー/ボディ
- クエリストリング
- 地理的ロケーション
- レート制限

### 4. AWS Config と CloudTrail の違い
| 機能 | CloudTrail | Config |
|------|-----------|--------|
| **目的** | 誰が何をしたか | リソースがポリシーに準拠しているか |
| **追跡対象** | APIコール | リソース構成の変更 |
| **代表的な質問** | 「誰がこのEC2を起動した？」 | 「このS3バケットは暗号化されている？」 |
| **出力** | イベントログ | 構成スナップショット |
| **ルール** | なし | 準拠ルール |
| **自動修復** | なし | あり（Systems Managerと連携） |

**記憶フレーズ**: 「Trail = 行動の跡、Config = 設定のコンプライアンス」

## 💡 試験でありがちなシナリオ

### シナリオ1: 既存の非暗号化EBSを暗号化したい
**Q**: 暗号化されていないEBSボリュームを暗号化する必要がある  
**✅ ANSWER**: スナップショットを作成 → 暗号化オプション付きでコピー → 暗号化スナップショットから新しいボリュームを作成

### シナリオ2: データベース認証情報の自動ローテーション
**Q**: DBパスワードを30日ごとに自動ローテーションしたい  
**✅ ANSWER**: AWS Secrets Managerの自動ローテーションを使用（Parameter Storeではない）

### シナリオ3: DDoSからアプリケーションを保護したい
**Q**: WebアプリケーションにDDoS保護が必要  
- **基本（無料）**: Shield Standard（自動有効）  
- **高度な保護**: Shield Advanced + WAF + Route 53

### シナリオ4: 不正アクセスの検知
**Q**: AWSアカウントで不審なアクティビティがあったらアラートを受けたい  
**✅ ANSWER**: GuardDuty を有効化（脅威検出サービス）

### シナリオ5: S3内の機微データの検出
**Q**: クレジットカード情報やPIIを含むS3バケットを特定したい  
**✅ ANSWER**: Amazon Macie

### シナリオ6: APIコールの監査
**Q**: 誰がS3オブジェクトを削除したかを追跡したい  
**✅ ANSWER**: CloudTrail（すべてのAPIコールをログに記録）

### シナリオ7: コンプライアンスの維持
**Q**: すべてのS3バケットが暗号化され、パブリックアクセスされていないことを確認したい  
**✅ ANSWER**: AWS Configのコンプライアンスルールを使用

### シナリオ8: セキュアなクロスアカウントアクセス
**Q**: アカウントAからアカウントBのリソースへ安全にアクセスしたい  
**✅ ANSWER**: 信頼ポリシー付きのIAMロール（認証情報の共有はNG）

## 🎓 スピードラーニングTips

### KMS Envelope Encryption
```
仕組み:
1. Data Encryption Key (DEK) でデータを暗号化
2. CMK がDEKを暗号化
3. 暗号化されたデータ + 暗号化されたDEKを一緒に保存

なぜそうするか:
├── 高速（ローカルで暗号化）
├── ネットワークトラフィックの削減
├── 大容量データでも高パフォーマンス
└── CMKはKMSから出ない
```

### S3 Security Layers（多層防御）
```
1. IAMポリシー（ユーザー/ロールへの権限付与）
2. バケットポリシー（リソースベースポリシー）
3. S3 Block Public Access（アカウントレベルの公開制御）
4. 暗号化（SSE-S3, SSE-KMS, SSE-C）
5. バージョニング + MFA Delete
6. VPCエンドポイント（プライベートアクセス）
7. CloudTrail（アクセスの監査）
8. Macie（機微データ検出）
```

### Secrets Manager vs Parameter Store
| 機能 | Secrets Manager | Parameter Store |
|------|----------------|-----------------|
| **ローテーション** | ✅ 自動ローテーション（RDSなど） | ❌ 手動のみ |
| **コスト** | $$$（約 $0.40/シークレット/月） | 無料（Standard）、有料（Advanced） |
| **統合** | RDS, Redshift, DocumentDB と統合 | ほぼあらゆるサービスで利用可能 |
| **リージョン間** | シークレットをレプリケーション可能 | 手動コピーが必要 |
| **主な用途** | DB認証情報、APIキーなどのシークレット | アプリ設定、シンプルな値 |

**判断ポイント**: 自動ローテーションが必要 → Secrets Manager、それ以外 → Parameter Store

## 📝 覚えておきたい速習ファクト

### KMS の主な制限
- **最大リクエストレート**: 5,500 / 10,000 / 30,000（キータイプにより異なる）
- **暗号化できる最大データサイズ**: 4 KB（それ以上はEnvelope Encryptionを使用）
- **自動ローテーション**: 365日ごと（Customer Managed CMKのみ）
- **キー削除**: 7〜30日の待機期間あり
- **リージョン**: マルチリージョンキーが利用可能

### CloudTrail ベストプラクティス
✅ すべてのリージョンで有効化  
✅ ログファイル検証（完全性チェック）を有効化  
✅ KMSでログを暗号化  
✅ S3に保存し、ライフサイクルポリシーを設定  
✅ CloudWatch Logsで監視  
✅ AWS Organizationsの組織トレイルを活用（マルチアカウント）  
✅ 管理イベント＋データイベントを有効化  

### GuardDuty のデータソース
- VPC Flow Logs
- CloudTrail イベントログ  
- DNSログ
- EKS 監査ログ
- S3 データイベント
- RDS ログインイベント
- EBS ボリュームデータ
- Lambda ネットワークアクティビティ

### 代表的なコンプライアンスプログラム
- **PCI DSS** - クレジットカード決済関連
- **HIPAA** - 医療データ
- **SOC 1/2/3** - セキュリティ統制報告
- **ISO 27001** - 情報セキュリティマネジメント
- **FedRAMP** - 米国政府向けクラウド
- **GDPR** - EUの個人データ保護規則

## 🚀 5分で振り返るマスターチェック

### セキュリティベストプラクティス チェックリスト
✅ **Identity**: ルートユーザー＋全ユーザーでMFA有効化  
✅ **Access**: 最小権限、ロールベースアクセスの徹底  
✅ **Detection**: GuardDuty, CloudTrail, Config を有効化  
✅ **Infrastructure**: Security Groups, NACLsでネットワーク制御  
✅ **Data**: 保存時・転送時ともに暗号化  
✅ **Incident**: CloudTrailログ、バックアップ、インシデント対応手順を用意  
✅ **Compliance**: Configルールや自動チェックで継続的な準拠確認  

### Encryption Decision Tree
```
1. 何を暗号化するか?
   DATA AT REST → KMS, SSE
   DATA IN TRANSIT → TLS/SSL, VPN

2. 誰が鍵を管理するか?
   AWS → AWS Managed Keys（無料）
   YOU → Customer Managed Keys（有料）
   COMPLIANCE重視 → CloudHSM（専用HSM）

3. S3ではどこまで制御したいか?
   シンプル → SSE-S3
   監査証跡が必要 → SSE-KMS
   自前の鍵を使いたい → SSE-C
   アップロード前に暗号化 → クライアントサイド暗号化
```

### 証明書管理
```
ACM (AWS Certificate Manager)
├── 無料のSSL/TLS証明書
├── 自動更新
├── 対応サービス: ALB, CloudFront, API Gateway など
├── リージョン単位（ただしCloudFront用は us-east-1）
└── プライベートキーはエクスポート不可

IMPORT CERTIFICATES
├── サードパーティ証明書をインポート
├── 更新は手動
├── エクスポート可能
└── 既存の証明書や特別な要件がある場合に使用
```

### よくあるミス
❌ ルートアカウントにMFAを設定していない  
❌ 日常作業にルートアカウントを使っている  
❌ IAM認証情報を共有している  
❌ 全リージョンでCloudTrailを有効化していない  
❌ 機微データを暗号化していない  
❌ 機微データを含むS3バケットがパブリックになっている  
❌ 自動ローテーションが必要なのにParameter Storeを使っている  
❌ GuardDutyを有効化していない（脅威検出ができない）  
❌ ソースコードや環境変数にシークレットをハードコードしている  
❌ 無料のShield Standardを有効活用していない  

## 🎯 試験対策スピードラン

**クイッククエスチョン**（答えは一番下）

1. DBパスワードを自動でローテーションするサービスは？ __  
2. S3内のPIIを検出するサービスは？ __  
3. 既存の非暗号化RDSインスタンスを後から暗号化できる？ __  
4. APIコールを追跡するサービスは？ __  
5. Shield Standardのコストは？ __  
6. KMSキーで暗号化できる最大データサイズは？ __  
7. 機械学習で脅威を検出するサービスは？ __  
8. リソースのコンプライアンスをチェックするサービスは？ __  

***

### Shared Responsibility Model - Security
```
AWSの責任（Security OF the cloud）
├── データセンターの物理セキュリティ
├── ハードウェア/インフラストラクチャ
├── ネットワークインフラ
├── マネージドサービスの運用
└── ハイパーバイザー

お客様の責任（Security IN the cloud）
├── データの暗号化（保存時・転送時）
├── IAM（ユーザー、グループ、ロール、ポリシー）
├── OSパッチやアップデート
├── アプリケーションセキュリティ
├── ネットワーク設定（Security Group, NACL）
├── ファイアウォール設定
└── コンプライアンス遵守の確認
```

### AWS Organizations - セキュリティ機能
- **Service Control Policies (SCP)**: 最大権限の制限（ガードレール）
- **Consolidated Billing**: コストの一元管理（セキュリティ関連コストの可視化）
- **CloudTrail**: 組織トレイルで全アカウントの監査ログを集中管理
- **GuardDuty**: 委任管理者アカウントで組織全体を監視
- **Security Hub**: セキュリティ状況の集中ビュー
- **Config**: 組織単位のConfigルール

### AWS Systems Manager - Session Manager
- **目的**: EC2インスタンスへのセキュアなシェルアクセス  
- **SSH鍵不要**: IAMベースのアクセス制御  
- **監査**: すべてのセッションをCloudTrailで記録  
- **バスティオンホスト不要**: コンソール/CLIから直接接続  
- **ポート22不要**: 通信はポート443を使用  

## ⏱️ 次のステップ
- 想定時間: 約60〜75分  
- ハンズオン: GuardDuty有効化、KMSキー作成、CloudTrail設定  
- 準備完了レベル: セキュリティ分野の練習問題に取り組める状態  
- 次に進むモジュール: Module 08 - Application Integration  

***

**Quick Answers**:  
1) AWS Secrets Manager  
2) Amazon Macie  
3) No（暗号化スナップショットから新規作成する必要あり）  
4) AWS CloudTrail  
5) Free（自動的に有効）  
6) 4 KB（それ以上はEnvelope Encryption）  
7) Amazon GuardDuty  
8) AWS Config  
