# ⚡ Fast Learning - ストレージサービス

> **完了時間**: 60-75 分 | **試験配点**: ~15-20%

## 🎯 必須理解コンセプト（5分）

### ストレージサービス選択（SEFI ルール）
```
OBJECT STORAGE? → S3 (files, static content)
BLOCK STORAGE? → EBS (databases, OS)
FILE STORAGE? → EFS/FSx (shared file systems)
ARCHIVAL? → S3 Glacier (long-term backup)
```

**記憶法**: "SEF-I store data" = S3, EBS, EFS/FSx, Ice (Glacier)

## 📊 クイックリファレンステーブル

### S3 ストレージクラス（試験最重要!）
| Class | Retrieval | Availability | Min Duration | 用途 | Cost |
|-------|-----------|--------------|--------------|----------|------|
| **Standard** | Instant | 99.99% | なし | 頻繁にアクセスされるデータ | $$$$ |
| **Intelligent-Tiering** | Instant | 99.9% | 30日 | アクセスパターン不明 | $$$ |
| **Standard-IA** | Instant | 99.9% | 30日 | アクセス頻度が低いデータ | $$ |
| **One Zone-IA** | Instant | 99.5% | 30日 | 非重要・低頻度アクセス | $ |
| **Glacier Instant** | Instant | 99.9% | 90日 | アーカイブ・即時取得 | $ |
| **Glacier Flexible** | 分〜時間 | 99.99% | 90日 | アーカイブ・まれにアクセス | $ |
| **Glacier Deep** | 12時間 | 99.99% | 180日 | 長期アーカイブ | $ |

**記憶法**: "SISO-GGG" = Standard, Intelligent, Standard-IA, One Zone-IA, Glacier x3

### EBS ボリュームタイプ（GISP）
| Type | Name | IOPS | Throughput | 用途 | Boot? |
|------|------|------|------------|----------|-------|
| **gp3** | General SSD | 16,000 | 1,000 MB/s | ほとんどのワークロード | ✅ |
| **gp2** | General SSD | 16,000 | 250 MB/s | 旧世代の一般用途 | ✅ |
| **io2** | Provisioned SSD | 64,000+ | 1,000 MB/s | ミッションクリティカル DB | ✅ |
| **io1** | Provisioned SSD | 64,000 | 1,000 MB/s | 旧世代の高性能用途 | ✅ |
| **st1** | Throughput HDD | 500 | 500 MB/s | ビッグデータ、ログ | ❌ |
| **sc1** | Cold HDD | 250 | 250 MB/s | 低頻度アクセス | ❌ |

**記憶法**: "GP = General 目的, IO = Input/Output intensive, ST = Streaming Throughput, SC = Slow/Cold"

## 🔥 試験頻出トピック

### 1. S3 機能 早見表
| Feature | 目的 | Exam Scenario |
|---------|---------|---------------|
| **Versioning** | すべてのバージョンを保持 | 削除から保護 |
| **Encryption** | データを保護 | コンプライアンス要件 |
| **MFA Delete** | 削除に MFA を要求 | 重要データ保護 |
| **Lifecycle Rules** | 自動遷移 / 自動削除 | コスト最適化 |
| **Replication** | 別バケットへコピー | DR、コンプライアンス |
| **Transfer Acceleration** | グローバルアップロード高速化 | 世界中のユーザー |
| **Static Hosting** | Web サイトをホスト | シンプルな静的サイト |

### 2. S3 暗号化オプション（SSEC）
```
SSE-S3 (AWS-managed keys)
└── AWS manages everything
└── AES-256 encryption
└── デフォルト option

SSE-KMS (KMS-managed keys)
└── More control, audit trail
└── CloudTrail logs key usage
└── Can set key policies

SSE-C (Customer-provided keys)
└── You manage keys
└── AWS encrypts/decrypts
└── Keys sent with each request

Client-Side Encryption
└── Encrypt before upload
└── You manage everything
```

**記憶法**: SSE-S3 = Simple、SSE-KMS = キー制御、SSE-C = 顧客キー、Client = 完全制御

### 3. EBS vs EFS vs Instance Store
| Feature | EBS | EFS | Instance Store |
|---------|-----|-----|----------------|
| **Type** | Block | File | Block |
| **Attach** | 1 インスタンス* | 複数インスタンス | 1 インスタンス |
| **AZ** | 単一 AZ | マルチ AZ | 単一 AZ |
| **Persist** | あり | あり | なし（ephemeral） |
| **Performance** | 高 | 共有 | 非常に高 |
| **用途** | DB、ブート | 共有ファイル | キャッシュ、一時領域 |

*Multi-attach available for io1/io2 in same AZ

**記憶法**: "BEI" = Block（EBS）、Everyone shares（EFS）、Instance-ephemeral（Instance Store）

### 4. S3 整合性モデル
```
✅ STRONG READ-AFTER-WRITE CONSISTENCY
└── PUT new object → Immediately readable
└── DELETE object → Immediately gone
└── UPDATE object → Immediately reflects new version

ALL OPERATIONS: Consistent since Dec 2020
```

## 💡 よくある試験シナリオ

### Scenario 1: 古いデータのコスト最適化
**質問**: データは最初の 30 日は頻繁にアクセスされ、90 日後はまれにアクセスされる
**✅ 正解**: Lifecycle policy: Standard → Standard-IA (30 days) → Glacier (90 days)

### Scenario 2: 重要な S3 データを保護
**質問**: 重要ファイルの誤削除を防ぎたい
**✅ 正解**: Enable versioning + MFA Delete + Bucket policy with explicit deny

### Scenario 3: EC2 インスタンス間でファイル共有
**質問**: 複数の EC2 インスタンスが同じファイルに read/write する必要がある
**✅ 正解**: EFS（EBS ではない - 1 インスタンスにのみマウント）

### Scenario 4: データベースボリューム性能
**質問**: データベースで 50,000 IOPS が必要
**✅ 正解**: io2 EBS volume (up to 64,000+ IOPS)

### Scenario 5: S3 への高速グローバルアップロード
**質問**: 世界中のユーザーが S3 にアップロードするため、速度が必要
**✅ 正解**: S3 Transfer Acceleration

### Scenario 6: コンプライアンス - 7 年保持
**質問**: 規制要件で 7 年保存が必要、アクセスはまれ
**✅ 正解**: S3 Glacier Deep Archive（長期保存で最安）

### Scenario 7: 一時的な高速ストレージ
**質問**: EC2 が処理のために非常に高速な一時ストレージを必要としている
**✅ 正解**: Instance Store（ephemeral、最速）

## 🎓 速習のコツ

### S3 バケット命名ルール
- 3-63 文字
- 小文字のみ
- 大文字とアンダースコアは不可
- 先頭は文字または数字
- グローバルで一意である必要がある

### S3 オブジェクトキー = フルパス
```
Bucket: my-bucket
Key: folder/subfolder/file.txt
URL: https://my-bucket.s3.amazonaws.com/folder/subfolder/file.txt
```

### EBS スナップショットの要点
- 増分バックアップ
- S3 に保存（AWS 管理）
- リージョン間コピー可能
- スナップショットから AMI 作成可能
- コピー時に暗号化可能
- 最初のスナップショットはフル、以降は増分

### S3 レプリケーションの種類
```
CRR (Cross-Region Replication)
└── Different regions
└── Compliance, lower latency
└── Disaster recovery

SRR (Same-Region Replication)
└── Same region
└── Log aggregation
└── Prod/test sync
```

**要件**: 両方のバケットで Versioning を有効化

## 📝 ラピッドファイア事実集

### S3 制限
- 最大 object size: **5 TB**
- Single PUT: **5 GB**
- Multi-part upload: Required for > **5 GB**, recommended for > **100 MB**
- 最大 parts: **10,000**
- Part size: **5 MB to 5 GB**

### EBS の事実
- 単一 AZ のみ（snapshot → 別 AZ へ restore は可能）
- detach/reattach 可能（ブートボリュームを除く）
- オンラインでサイズ変更可能
- スナップショットは S3 に保存（マルチ AZ）
- 既存の非暗号化ボリュームも snapshot 経由で暗号化可能

### EFS の機能
- デフォルトでマルチ AZ
- 自動スケール（プロビジョニング不要）
- 使った分だけ課金
- NFSv4.1 プロトコル
- Linux のみ
- 数千の同時接続

### FSx クイック比較
| Type | OS | Protocol | 用途 |
|------|-----|----------|----------|
| **FSx for Windows** | Windows | SMB | Windows アプリ、AD |
| **FSx for Lustre** | Linux | Lustre | HPC、ML、ビッグデータ |
| **FSx for NetApp ONTAP** | Any | NFS/SMB | マルチプロトコル |
| **FSx for OpenZFS** | Linux | NFS | Linux ワークロード |

## 🚀 5分マスターレビュー

### ストレージ意思決定ツリー
```
1. What type of data?
   OBJECTS (files, images) → S3
   BLOCKS (OS, DB) → EBS or Instance Store
   FILES (shared) → EFS or FSx
   
2. For S3, how often accessed?
   FREQUENT → S3 Standard
   INFREQUENT → Standard-IA
   ARCHIVE → Glacier
   UNKNOWN → Intelligent-Tiering
   
3. For EBS, what performance?
   GENERAL → gp3
   HIGH IOPS → io2
   THROUGHPUT → st1
   COLD → sc1
   
4. For File Storage, what OS?
   LINUX → EFS
   WINDOWS → FSx for Windows
   HPC → FSx for Lustre
```

### S3 ライフサイクルルール例
```
Transition:
Standard → Standard-IA (30 days)
Standard-IA → Glacier (90 days)
Glacier → Deep Archive (180 days)

Expiration:
Delete objects after 365 days
Delete incomplete multipart uploads after 7 days
```

### 避けるべきよくあるミス
❌ 共有ストレージに EBS を使う（EFS を使う）
❌ EBS が単一 AZ であることを忘れる
❌ コスト削減のためのライフサイクルポリシーを設定しない
❌ 誤った S3 ストレージクラスを選ぶ
❌ レプリケーション前に versioning を有効化し忘れる
❌ 低頻度アクセスデータに Standard を使う
❌ 永続データに Instance Store を使う（ephemeral!）
❌ 機密データを暗号化しない

## 🎯 試験練習スピードラン

**クイック問題**（答えは下）

1. 最大 S3 object size? __
2. 50,000 IOPS に適した EBS タイプは? __
3. EFS は Windows で使える? __
4. アクセスパターン不明時の S3 ストレージクラスは? __
5. EBS は複数インスタンスにアタッチできる? __
6. EFS が使うプロトコルは? __
7. EBS スナップショットはどこに保存される? __
8. Glacier Deep Archive の最短保存期間は? __

---

### S3 Pre-signed URLs
- **目的**: 非公開オブジェクトへの一時アクセス
- **有効期間**: 設定可能（秒〜日）
- **用途**: 非公開ファイルのダウンロード、バケットへのアップロード
- **例**: 公開せずに 1 時間だけファイル共有

### S3 Event Notifications
**トリガー**:
- オブジェクト作成（PUT, POST, COPY）
- オブジェクト削除（DELETE）
- オブジェクト復元（from Glacier）
- レプリケーションイベント

**送信先**:
- Lambda functions
- SQS queues
- SNS topics
- EventBridge

## 🔒 S3 セキュリティレイヤー
```
1. IAM Policies (user/role permissions)
2. Bucket Policies (resource-based)
3. ACLs (legacy, not recommended)
4. Encryption (at rest)
5. SSL/TLS (in transit)
6. VPC Endpoints (private access)
7. Block Public Access (account level)
```

## ⏱️ 次のステップ
- 学習時間: ~60-75分
- 演習: S3 バケット、ライフサイクルルール、EBS ボリュームを作成
- 準備完了: Storage practice questions
- 次へ: Module 05 - Database

---

**クイック解答**:
1) 5 TB
2) io2 or io1 (Provisioned IOPS SSD)
3) なし（Linux のみ）
4) S3 Intelligent-Tiering
5) なし（except io1/io2 multi-attach in same AZ）
6) NFSv4.1
7) S3 (managed by AWS)
8) 180 days
