# ⚡ Fast Learning - Security \& Compliance

> **Time to Complete**: 60-75 minutes | **Exam Weight**: ~20-25%

## 🎯 Must-Know Concepts (5 Minutes)

### Security Service Selector (KSS-WCGI)

```
ENCRYPTION KEYS? → KMS (Key Management Service)
SECRETS/PASSWORDS? → Secrets Manager
CERTIFICATES? → ACM (Certificate Manager)
FIREWALL? → Security Groups, NACLs, WAF
DDoS PROTECTION? → Shield (Standard/Advanced)
COMPLIANCE? → CloudTrail, Config, Inspector
IDENTITY? → IAM, Cognito, Directory Service
DETECTION? → GuardDuty, Macie, Detective
```

**Memory Aid**: "Keys Secure Secrets, While Controlling Guarding Identity"

## 📊 Quick Reference Tables

### Encryption Services Matrix

| Service | Type | Use Case | Key Management |
| :-- | :-- | :-- | :-- |
| **KMS** | Encryption keys | Encrypt AWS resources | AWS managed or Customer |
| **CloudHSM** | Hardware module | Regulatory compliance | Customer only |
| **Secrets Manager** | Secret rotation | DB credentials, API keys | Auto-rotation |
| **Parameter Store** | Config/secrets | App config, simple secrets | Free tier available |
| **ACM** | SSL/TLS certs | HTTPS/TLS | AWS managed, auto-renew |

### Security Monitoring Services

| Service | What It Does | Detects | Cost Model |
| :-- | :-- | :-- | :-- |
| **GuardDuty** | Threat detection | Malicious activity, anomalies | Events analyzed |
| **Macie** | Data security | Sensitive data in S3 (PII) | GB scanned |
| **Inspector** | Vulnerability assessment | EC2/ECR vulnerabilities | Assessments run |
| **Detective** | Security investigation | Root cause analysis | GB ingested |
| **Security Hub** | Central dashboard | Aggregates findings | Checks run |

## 🔥 Exam Hot Topics

### 1. KMS Key Types (Critical!)

```
AWS MANAGED KEYS
├── Free
├── Auto-rotation every year (mandatory)
├── Format: aws/service-name (e.g., aws/s3)
└── Use: Default encryption

CUSTOMER MANAGED KEYS (CMK)
├── $1/month per key
├── Optional auto-rotation (yearly)
├── Full control over key policies
├── Can be disabled/deleted
└── Use: Custom requirements, compliance

AWS OWNED KEYS
├── Free
├── Used by AWS for multiple accounts
├── No visibility or control
└── Use: DynamoDB encryption (default)
```

**Memory Aid**: "AMC" = AWS (free, auto), Customer (control, cost), Owned (opaque)

### 2. Data Encryption States

```
ENCRYPTION AT REST
├── KMS for most AWS services
├── S3: SSE-S3, SSE-KMS, SSE-C
├── EBS: Encrypted volumes
├── RDS: Encryption at creation
└── DynamoDB: Server-side encryption

ENCRYPTION IN TRANSIT
├── SSL/TLS (HTTPS)
├── VPN (Site-to-Site)
├── Direct Connect + VPN
├── Client-side encryption
└── AWS services use TLS by default
```


### 3. WAF, Shield, Firewall Manager

| Service | Layer | Purpose | Use Case |
| :-- | :-- | :-- | :-- |
| **WAF** | Layer 7 | Web app firewall | SQL injection, XSS protection |
| **Shield Standard** | Layer 3/4 | DDoS protection | Free, automatic |
| **Shield Advanced** | Layer 3/4/7 | Enhanced DDoS | \$3K/month, 24/7 support |
| **Firewall Manager** | Multiple | Central management | Multi-account security |

**WAF Rules**:

- IP addresses
- HTTP headers/body
- Query strings
- Geographic location
- Rate limiting


### 4. AWS Config vs CloudTrail

| Feature | CloudTrail | Config |
| :-- | :-- | :-- |
| **Purpose** | Who did what | Resource compliance |
| **Tracks** | API calls | Resource configuration changes |
| **Question** | "Who launched this EC2?" | "Is S3 bucket encrypted?" |
| **Output** | Event logs | Configuration snapshots |
| **Rules** | N/A | Compliance rules |
| **Remediation** | No | Yes (with Systems Manager) |

**Memory Aid**: "Trail = Trail of actions, Config = Configuration compliance"

## 💡 Common Exam Scenarios

### Scenario 1: Encrypt Existing Unencrypted EBS

**Q**: Need to encrypt unencrypted EBS volume
**✅ ANSWER**: Create snapshot → Copy with encryption → Create volume from encrypted snapshot

### Scenario 2: Rotate Database Credentials

**Q**: Automate database password rotation every 30 days
**✅ ANSWER**: AWS Secrets Manager with automatic rotation (not Parameter Store)

### Scenario 3: Protect Against DDoS

**Q**: Web application needs DDoS protection

- **Basic (free)**: Shield Standard (auto-enabled)
- **Advanced**: Shield Advanced + WAF + Route 53


### Scenario 4: Detect Unauthorized Access

**Q**: Get alerts on suspicious activity in AWS account
**✅ ANSWER**: Enable GuardDuty (threat detection)

### Scenario 5: Find Sensitive Data in S3

**Q**: Discover S3 buckets containing credit card numbers or PII
**✅ ANSWER**: Amazon Macie

### Scenario 6: Audit API Calls

**Q**: Track who deleted an S3 object
**✅ ANSWER**: CloudTrail (logs all API calls)

### Scenario 7: Ensure Compliance

**Q**: Verify all S3 buckets are encrypted and private
**✅ ANSWER**: AWS Config with compliance rules

### Scenario 8: Cross-Account Access Securely

**Q**: Account A needs to access resources in Account B
**✅ ANSWER**: IAM roles with trust policy (not sharing credentials)

## 🎓 Speed Learning Tips

### KMS Envelope Encryption

```
HOW IT WORKS:
1. Data Encryption Key (DEK) encrypts data
2. CMK encrypts DEK
3. Store encrypted data + encrypted DEK together

WHY:
├── Faster (encrypt locally)
├── Less network traffic
├── Better performance for large data
└── CMK never leaves KMS
```


### S3 Security Layers (Defense in Depth)

```
1. IAM Policies (user permissions)
2. Bucket Policies (resource-based)
3. S3 Block Public Access (account-level)
4. Encryption (SSE-S3, SSE-KMS, SSE-C)
5. Versioning + MFA Delete
6. VPC Endpoints (private access)
7. CloudTrail (audit access)
8. Macie (detect sensitive data)
```


### Secrets Manager vs Parameter Store

| Feature | Secrets Manager | Parameter Store |
| :-- | :-- | :-- |
| **Rotation** | ✅ Automatic (RDS, etc.) | ❌ Manual |
| **Cost** | \$\$\$  (\$0.40/secret/month) | Free (standard), \$ (advanced) |
| **Integration** | RDS, Redshift, DocumentDB | Any service |
| **Cross-region** | Replicate secrets | Must copy manually |
| **Use Case** | DB credentials, API keys | App config, simple values |

**Decision**: Need rotation? → Secrets Manager, Otherwise → Parameter Store

## 📝 Rapid-Fire Facts

### KMS Important Limits

- **Max request rate**: 5,500/10,000/30,000 (depends on key type)
- **Max data size**: 4 KB (use envelope encryption for larger)
- **Auto-rotation**: Every 365 days (CMK only)
- **Key deletion**: 7-30 day waiting period
- **Regions**: Multi-region keys available


### CloudTrail Best Practices

✅ Enable in all regions
✅ Enable log file validation (integrity)
✅ Encrypt logs with KMS
✅ Store in S3 with lifecycle policy
✅ Monitor with CloudWatch Logs
✅ Use Organizations trail (multi-account)
✅ Enable for management + data events

### GuardDuty Data Sources

- VPC Flow Logs
- CloudTrail event logs
- DNS logs
- EKS audit logs
- S3 data events
- RDS login events
- EBS volume data
- Lambda network activity


### Compliance Programs (Sample)

- **PCI DSS** - Payment cards
- **HIPAA** - Healthcare
- **SOC 1/2/3** - Security controls
- **ISO 27001** - Information security
- **FedRAMP** - US government
- **GDPR** - EU data protection


## 🚀 5-Minute Master Review

### Security Best Practices Checklist

✅ **Identity**: MFA on root + all users
✅ **Access**: Least privilege, use roles
✅ **Detection**: GuardDuty, CloudTrail, Config
✅ **Infrastructure**: Security groups, NACLs
✅ **Data**: Encrypt at rest + in transit
✅ **Incident**: CloudTrail, backup, playbooks
✅ **Compliance**: Config rules, automated checks

### Encryption Decision Tree

```
1. What to encrypt?
   DATA AT REST → KMS, SSE
   DATA IN TRANSIT → TLS/SSL, VPN
   
2. Who manages keys?
   AWS → AWS managed keys (free)
   YOU → Customer managed keys ($)
   COMPLIANCE → CloudHSM (dedicated)
   
3. For S3, what level of control?
   SIMPLE → SSE-S3
   AUDIT TRAIL → SSE-KMS
   YOUR KEYS → SSE-C
   BEFORE UPLOAD → Client-side
```


### Certificate Management

```
ACM (AWS Certificate Manager)
├── Free SSL/TLS certificates
├── Auto-renewal
├── Integration: ALB, CloudFront, API Gateway
├── Regional (except CloudFront = us-east-1)
└── Cannot export private key

IMPORT CERTIFICATES
├── Third-party certificates
├── Manual renewal
├── Can export
└── Use when: Existing certs, special requirements
```


### Common Mistakes to Avoid

❌ Not enabling MFA on root account
❌ Using root account for daily tasks
❌ Sharing IAM credentials
❌ Forgetting CloudTrail in all regions
❌ Not encrypting sensitive data
❌ Public S3 buckets with sensitive data
❌ Using Parameter Store when rotation needed
❌ Not enabling GuardDuty for threat detection
❌ Storing secrets in code or environment variables
❌ Not using Shield Standard (it's free!)

## 🎯 Exam Practice Speedrun

**Quick Questions** (Answers at bottom)

1. What rotates DB passwords automatically? __
2. Which service detects PII in S3? __
3. Can you encrypt existing unencrypted RDS? __
4. What tracks API calls? __
5. What's the cost of Shield Standard? __
6. KMS key max data size? __
7. What detects threats using ML? __
8. What checks resource compliance? __

---

### Shared Responsibility Model - Security

```
AWS RESPONSIBILITY (Security OF the cloud)
├── Physical security of data centers
├── Hardware/infrastructure
├── Network infrastructure
├── Managed service operations
└── Hypervisor

CUSTOMER RESPONSIBILITY (Security IN the cloud)
├── Data encryption (at rest & in transit)
├── IAM (users, groups, roles, policies)
├── OS patches and updates
├── Application security
├── Network configuration (SG, NACL)
├── Firewall configuration
└── Compliance validation
```


### AWS Organizations - Security Features

- **Service Control Policies (SCP)**: Limit permissions
- **Consolidated billing**: Security cost tracking
- **CloudTrail**: Organization trail
- **GuardDuty**: Delegated administrator
- **Security Hub**: Central security view
- **Config**: Organization rules


### AWS Systems Manager - Session Manager

- **Purpose**: Secure shell access to EC2
- **No SSH keys needed**: IAM-based access
- **Auditing**: CloudTrail logs all sessions
- **No bastion host**: Direct access via console/CLI
- **Port 22**: Not required (uses port 443)


## ⏱️ Next Steps

- Time spent: ~60-75 min
- Practice: Enable GuardDuty, create KMS key, CloudTrail
- Ready for: Security practice questions
- Move to: Module 08 - Application Integration

---

**Quick Answers**:

1) AWS Secrets Manager
2) Amazon Macie
3) No (must create new from snapshot with encryption)
4) AWS CloudTrail
5) Free (automatically enabled)
6) 4 KB (use envelope encryption for more)
7) Amazon GuardDuty
8) AWS Config

以下が日本語に翻訳したマークダウンです 。[^1][^2]

```markdown
# ⚡ 速習ガイド - セキュリティ & コンプライアンス

> **所要時間**: 60〜75分 | **試験の出題比率**: 約20〜25%

## 🎯 必須概念（5分）

### セキュリティサービス選択ガイド (KSS-WCGI)
```

暗号化キーが必要？ → KMS (Key Management Service)
シークレット/パスワードが必要？ → Secrets Manager
証明書が必要？ → ACM (Certificate Manager)
ファイアウォールが必要？ → Security Groups、NACLs、WAF
DDoS対策が必要？ → Shield（Standard/Advanced）
コンプライアンスが必要？ → CloudTrail、Config、Inspector
アイデンティティ管理が必要？ → IAM、Cognito、Directory Service
脅威検出が必要？ → GuardDuty、Macie、Detective

```

**記憶術**: "Keys Secure Secrets, While Controlling Guarding Identity"

## 📊 クイックリファレンス表

### 暗号化サービス一覧
| サービス | 種別 | ユースケース | キー管理 |
|---------|------|------------|---------|
| **KMS** | 暗号化キー | AWSリソースの暗号化 | AWSマネージドまたはカスタマー管理 |
| **CloudHSM** | ハードウェアモジュール | 規制コンプライアンス | カスタマー管理のみ |
| **Secrets Manager** | シークレットのローテーション | DBの認証情報、APIキー | 自動ローテーション |
| **Parameter Store** | 設定/シークレット | アプリ設定、簡易シークレット | 無料枠あり |
| **ACM** | SSL/TLS証明書 | HTTPS/TLS | AWSマネージド、自動更新 |

### セキュリティモニタリングサービス
| サービス | 機能 | 検出対象 | 課金モデル |
|---------|------|---------|---------|
| **GuardDuty** | 脅威検出 | 悪意のある活動、異常検知 | 分析イベント数 |
| **Macie** | データセキュリティ | S3内の機密データ（PII） | スキャンGB数 |
| **Inspector** | 脆弱性評価 | EC2/ECRの脆弱性 | 評価実行数 |
| **Detective** | セキュリティ調査 | 根本原因分析 | 取り込みGB数 |
| **Security Hub** | 集中ダッシュボード | 検出結果の集約 | チェック実行数 |

## 🔥 試験頻出トピック

### 1. KMSキーの種類（重要！）
```

AWSマネージドキー
├── 無料
├── 毎年自動ローテーション（必須）
├── 形式: aws/サービス名（例: aws/s3）
└── 用途: デフォルト暗号化

カスタマーマネージドキー（CMK）
├── 月額\$1/キー
├── 自動ローテーション任意（年1回）
├── キーポリシーの完全制御
├── 無効化/削除が可能
└── 用途: カスタム要件、コンプライアンス

AWSオウンドキー
├── 無料
├── AWSが複数アカウントに使用
├── 可視性・制御なし
└── 用途: DynamoDBの暗号化（デフォルト）

```

**記憶術**: "AMC" = AWS（無料・自動）、Customer（制御・コスト）、Owned（不透明）

### 2. データ暗号化の状態
```

保存時の暗号化（Encryption at Rest）
├── ほとんどのAWSサービスでKMSを使用
├── S3: SSE-S3、SSE-KMS、SSE-C
├── EBS: 暗号化ボリューム
├── RDS: 作成時に暗号化設定
└── DynamoDB: サーバーサイド暗号化

転送中の暗号化（Encryption in Transit）
├── SSL/TLS（HTTPS）
├── VPN（サイト間VPN）
├── Direct Connect + VPN
├── クライアントサイド暗号化
└── AWSサービスはデフォルトでTLSを使用

```

### 3. WAF、Shield、Firewall Manager
| サービス | レイヤー | 目的 | ユースケース |
|---------|---------|------|-----------|
| **WAF** | レイヤー7 | Webアプリファイアウォール | SQLインジェクション、XSS対策 |
| **Shield Standard** | レイヤー3/4 | DDoS防御 | 無料・自動適用 |
| **Shield Advanced** | レイヤー3/4/7 | 強化DDoS防御 | $3K/月、24/7サポート |
| **Firewall Manager** | 複数レイヤー | 集中管理 | マルチアカウントセキュリティ |

**WAFルールの適用対象**:
- IPアドレス
- HTTPヘッダー/ボディ
- クエリ文字列
- 地理的ロケーション
- レートリミット

### 4. AWS Config と CloudTrail の比較
| 機能 | CloudTrail | Config |
|------|-----------|--------|
| **目的** | 誰が何をしたか | リソースのコンプライアンス |
| **追跡対象** | APIコール | リソース設定の変更 |
| **問い** | "誰がこのEC2を起動したか？" | "S3バケットは暗号化されているか？" |
| **出力** | イベントログ | 設定スナップショット |
| **ルール** | なし | コンプライアンスルール |
| **修復** | 不可 | 可（Systems Manager連携） |

**記憶術**: "Trail = 行動の証跡、Config = 設定コンプライアンス"

## 💡 試験頻出シナリオ

### シナリオ1: 既存の未暗号化EBSを暗号化
**Q**: 未暗号化のEBSボリュームを暗号化する必要がある
**✅ 回答**: スナップショットを作成 → 暗号化を有効にしてコピー → 暗号化スナップショットからボリュームを作成

### シナリオ2: データベース認証情報をローテーションする
**Q**: データベースのパスワードを30日ごとに自動ローテーションする
**✅ 回答**: AWS Secrets Managerの自動ローテーションを使用（Parameter Storeは不可）

### シナリオ3: DDoS攻撃から保護する
**Q**: WebアプリケーションのDDoS対策が必要
- **基本（無料）**: Shield Standard（自動有効）
- **高度**: Shield Advanced + WAF + Route 53

### シナリオ4: 不正アクセスを検知する
**Q**: AWSアカウント内の不審な活動にアラートを設定する
**✅ 回答**: GuardDutyを有効化（脅威検出）

### シナリオ5: S3内の機密データを検出する
**Q**: クレジットカード番号やPIIを含むS3バケットを発見する
**✅ 回答**: Amazon Macie

### シナリオ6: APIコールを監査する
**Q**: 誰がS3オブジェクトを削除したかを追跡する
**✅ 回答**: CloudTrail（全APIコールをログ記録）

### シナリオ7: コンプライアンスを確保する
**Q**: すべてのS3バケットが暗号化されプライベートであることを検証する
**✅ 回答**: AWS Configのコンプライアンスルールを使用

### シナリオ8: クロスアカウントアクセスをセキュアに実現する
**Q**: アカウントAがアカウントBのリソースにアクセスする必要がある
**✅ 回答**: 信頼ポリシーを使用したIAMロール（認証情報の共有は不可）

## 🎓 速習のポイント

### KMSエンベロープ暗号化
```

仕組み:

1. データ暗号化キー（DEK）でデータを暗号化
2. CMKがDEKを暗号化
3. 暗号化データ + 暗号化DEKをまとめて保存

メリット:
├── 高速（ローカルで暗号化）
├── ネットワークトラフィックが少ない
├── 大容量データのパフォーマンスが向上
└── CMKはKMS外に出ない

```

### S3セキュリティレイヤー（多層防御）
```

1. IAMポリシー（ユーザー権限）
2. バケットポリシー（リソースベース）
3. S3パブリックアクセスのブロック（アカウントレベル）
4. 暗号化（SSE-S3、SSE-KMS、SSE-C）
5. バージョニング + MFA削除
6. VPCエンドポイント（プライベートアクセス）
7. CloudTrail（アクセス監査）
8. Macie（機密データの検出）
```

### Secrets Manager と Parameter Store の比較
| 機能 | Secrets Manager | Parameter Store |
|------|----------------|----------------|
| **ローテーション** | ✅ 自動（RDSなど） | ❌ 手動 |
| **コスト** | $$$ （$0.40/シークレット/月） | 無料（標準）、$（高度） |
| **統合** | RDS、Redshift、DocumentDB | 任意のサービス |
| **クロスリージョン** | シークレットをレプリケート | 手動でコピーが必要 |
| **ユースケース** | DB認証情報、APIキー | アプリ設定、シンプルな値 |

**判断基準**: ローテーションが必要 → Secrets Manager、不要 → Parameter Store

## 📝 重要事項まとめ

### KMSの制限値
- **最大リクエストレート**: 5,500/10,000/30,000（キーの種類による）
- **最大データサイズ**: 4 KB（より大きいデータはエンベロープ暗号化を使用）
- **自動ローテーション**: 365日ごと（CMKのみ）
- **キーの削除**: 7〜30日の待機期間
- **リージョン**: マルチリージョンキーが利用可能

### CloudTrailのベストプラクティス
✅ 全リージョンで有効化する
✅ ログファイル検証を有効化する（整合性確認）
✅ KMSでログを暗号化する
✅ ライフサイクルポリシーを設定したS3に保存する
✅ CloudWatch Logsで監視する
✅ Organizationsトレイルを使用する（マルチアカウント）
✅ 管理イベント + データイベントを有効化する

### GuardDutyのデータソース
- VPC Flow Logs
- CloudTrailイベントログ
- DNSログ
- EKS監査ログ
- S3データイベント
- RDSログインイベント
- EBSボリュームデータ
- Lambdaネットワークアクティビティ

### コンプライアンスプログラム（例）
- **PCI DSS** - 決済カード
- **HIPAA** - 医療
- **SOC 1/2/3** - セキュリティコントロール
- **ISO 27001** - 情報セキュリティ
- **FedRAMP** - 米国政府
- **GDPR** - EUデータ保護

## 🚀 5分間マスターレビュー

### セキュリティベストプラクティス チェックリスト
✅ **アイデンティティ**: ルートアカウントと全ユーザーにMFAを設定
✅ **アクセス**: 最小権限の原則、ロールを使用
✅ **検出**: GuardDuty、CloudTrail、Config
✅ **インフラ**: Security Groups、NACLs
✅ **データ**: 保存時・転送中の暗号化
✅ **インシデント**: CloudTrail、バックアップ、プレイブック
✅ **コンプライアンス**: Configルール、自動チェック

### 暗号化の選択フロー
```

1. 何を暗号化するか？
保存時のデータ → KMS、SSE
転送中のデータ → TLS/SSL、VPN
2. 誰がキーを管理するか？
AWS → AWSマネージドキー（無料）
自分 → カスタマーマネージドキー（有料）
コンプライアンス → CloudHSM（専用ハードウェア）
3. S3で必要な制御レベルは？
シンプル → SSE-S3
監査証跡 → SSE-KMS
自分のキー → SSE-C
アップロード前 → クライアントサイド暗号化
```

### 証明書管理
```

ACM（AWS Certificate Manager）
├── 無料のSSL/TLS証明書
├── 自動更新
├── 統合: ALB、CloudFront、API Gateway
├── リージョン別（CloudFrontのみus-east-1）
└── 秘密鍵のエクスポート不可

証明書のインポート
├── サードパーティ証明書
├── 手動更新
├── エクスポート可能
└── 用途: 既存の証明書、特別な要件がある場合

```

### よくある間違い
❌ ルートアカウントにMFAを設定しない
❌ 日常業務にルートアカウントを使用する
❌ IAM認証情報を共有する
❌ 全リージョンでCloudTrailを有効化しない
❌ 機密データを暗号化しない
❌ 機密データを含むS3バケットをパブリックにする
❌ ローテーションが必要なのにParameter Storeを使用する
❌ 脅威検出のためにGuardDutyを有効化しない
❌ コードや環境変数にシークレットを保存する
❌ Shield Standardを使用しない（無料なのに！）

## 🎯 試験練習 スピードラン

**クイッククイズ**（答えは下部）

1. DBパスワードを自動ローテーションするサービスは？ __
2. S3のPIIを検出するサービスは？ __
3. 既存の未暗号化RDSを暗号化できるか？ __
4. APIコールを追跡するサービスは？ __
5. Shield Standardのコストは？ __
6. KMSキーの最大データサイズは？ __
7. MLを使って脅威を検出するサービスは？ __
8. リソースコンプライアンスを確認するサービスは？ __

---

### 責任共有モデル - セキュリティ
```

AWSの責任（クラウドのセキュリティ）
├── データセンターの物理セキュリティ
├── ハードウェア/インフラストラクチャ
├── ネットワークインフラストラクチャ
├── マネージドサービスの運用
└── ハイパーバイザー

お客様の責任（クラウド上のセキュリティ）
├── データの暗号化（保存時・転送時）
├── IAM（ユーザー、グループ、ロール、ポリシー）
├── OSのパッチと更新
├── アプリケーションセキュリティ
├── ネットワーク設定（SG、NACL）
├── ファイアウォール設定
└── コンプライアンス検証

```

### AWS Organizations - セキュリティ機能
- **Service Control Policies（SCP）**: 権限の制限
- **一括請求**: セキュリティコストの追跡
- **CloudTrail**: 組織全体のトレイル
- **GuardDuty**: 委任管理者
- **Security Hub**: 集中セキュリティビュー
- **Config**: 組織全体のルール

### AWS Systems Manager - Session Manager
- **目的**: EC2へのセキュアなシェルアクセス
- **SSHキー不要**: IAMベースのアクセス
- **監査**: CloudTrailが全セッションをログ記録
- **踏み台ホスト不要**: コンソール/CLIから直接アクセス
- **ポート22不要**: ポート443を使用

## ⏱️ 次のステップ
- 所要時間: 約60〜75分
- 実践: GuardDutyの有効化、KMSキーの作成、CloudTrailの設定
- 準備完了: セキュリティ練習問題
- 次のモジュール: Module 08 - アプリケーション統合

---

**クイズの答え**: 
1) AWS Secrets Manager
2) Amazon Macie
3) 不可（暗号化を有効にしてスナップショットをコピーし、新しいボリュームを作成する必要がある）
4) AWS CloudTrail
5) 無料（自動的に有効化）
6) 4 KB（それ以上はエンベロープ暗号化を使用）
7) Amazon GuardDuty
8) AWS Config
```
