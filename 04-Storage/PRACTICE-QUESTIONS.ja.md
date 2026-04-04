# ストレージサービス - 練習問題

> **⚠️ 免責事項:** これらは AWS ドキュメントに基づき教育目的で作成した**オリジナル練習問題**です。AWS 認定試験の**実際の問題ではありません**。

## 試験標準問題（SAA-C03）

---

### 問題 1
ある企業は、低レイテンシかつ高スループットで頻繁にアクセスされるデータを保存する必要があります。コストは主な懸念事項ではありません。どの S3 ストレージクラスを使うべきですか？

A. S3 Standard  
B. S3 Intelligent-Tiering  
C. S3 Standard-IA  
D. S3 One Zone-IA  

<details>
<summary>解答を表示</summary>

**Answer: A**

**解説:**
- **S3 Standard** の特長:
  - ミリ秒レイテンシ
  - 高スループット
  - 99.99% の可用性
  - 11 ナインの耐久性
  - 頻繁にアクセスされるデータ向け

**S3 ストレージクラス比較**:
- **Standard**: 高頻度アクセス、最も高コスト
- **Intelligent-Tiering**: 不明/変動するアクセスパターン、自動階層化
- **Standard-IA**: 低頻度アクセス、低コスト、取り出し料金あり
- **One Zone-IA**: 低頻度アクセス、単一 AZ、IA で最安
- **Glacier**: アーカイブ、取り出しは数分〜数時間
- **Glacier Deep Archive**: 長期アーカイブ、取り出しは 12 時間以上

**References:** S3 Storage Classes, S3 Standard
</details>

---

### 問題 2
企業は S3 Standard-IA に低頻度アクセスデータを保存しています。必要になったらこのデータへ即時アクセスしたい場合、取り出し時間はどれですか？

A. 12 hours  
B. 3-5 hours  
C. 1-5 minutes  
D. Milliseconds (immediate)  

<details>
<summary>解答を表示</summary>

**Answer: D**

**解説:**
- **S3 Standard-IA** は即時アクセス（ミリ秒）を提供
- "IA" は Infrequent Access を意味し、遅いアクセスという意味ではない
- Standard より保存コストは低い
- GB 単位の取り出し料金が発生
- 最低保存期間: 30 日
- 最小オブジェクトサイズ: 128 KB

**S3 取り出し時間**:
- **Standard/Standard-IA/One Zone-IA**: Milliseconds
- **Intelligent-Tiering**: Milliseconds
- **Glacier Instant Retrieval**: Milliseconds
- **Glacier Flexible Retrieval**: 
  - Expedited: 1-5 minutes
  - Standard: 3-5 hours
  - Bulk: 5-12 hours
- **Glacier Deep Archive**:
  - Standard: 12 hours
  - Bulk: 48 hours

**References:** S3 Standard-IA, S3 Retrieval Times
</details>

---

### 問題 3
企業は 7 年間保持が必要で、年に 1〜2 回しかアクセスしないコンプライアンスデータを保存する必要があります。コスト最適化が最重要です。最も費用対効果の高いストレージはどれですか？

A. S3 Standard  
B. S3 Glacier Flexible Retrieval  
C. S3 Glacier Deep Archive  
D. S3 Intelligent-Tiering  

<details>
<summary>解答を表示</summary>

**Answer: C**

**解説:**
- **S3 Glacier Deep Archive** は S3 で最も安価なストレージクラス
- 長期保持（7〜10 年以上）向け
- 取り出し時間: 12〜48 時間（まれなアクセスなら許容可能）
- GB 単価が最安
- コンプライアンス・規制アーカイブに最適

**コスト比較（概算）**:
- **Deep Archive**: $0.00099/GB/month（最安）
- **Glacier Flexible**: $0.0036/GB/month
- **Standard-IA**: $0.0125/GB/month
- **Standard**: $0.023/GB/month

**使い分け**:
- **Deep Archive**: ほぼアクセスしない、7 年以上保持
- **Glacier Flexible**: ときどきアクセスするアーカイブ
- **Standard-IA**: 月次でアクセス
- **Standard**: 高頻度アクセス

**References:** S3 Glacier Deep Archive, Archive Storage
</details>

---

### 問題 4
ある Web アプリケーションは静的コンテンツ（images, CSS, JS）をグローバルユーザーへ配信しています。コンテンツは S3 に保存されています。性能向上とレイテンシ削減のための最適な方法はどれですか？

A. Enable S3 Transfer Acceleration  
B. Use CloudFront with S3 as origin  
C. Enable S3 Cross-Region Replication  
D. Use S3 Standard storage class  

<details>
<summary>解答を表示</summary>

**Answer: B**

**解説:**
- **CloudFront** は AWS の CDN（Content Delivery Network）
- 世界中 400 以上のエッジロケーションでコンテンツをキャッシュ
- 世界中のユーザー向けにレイテンシを削減
- オリジン S3 バケットの負荷を低減

**CloudFront の利点**:
- 低レイテンシ（最寄りエッジから配信）
- 高速転送
- DDoS 保護（AWS Shield）
- SSL/TLS サポート
- S3 データ転送コストの削減

**その他の選択肢**:
- **Transfer Acceleration**: S3 へのアップロード高速化であり、ダウンロード高速化ではない
- **Cross-Region Replication**: マルチリージョン冗長化であり、CDN ではない
- **Storage class**: 配信性能には直接影響しない

**References:** Amazon CloudFront, S3 with CloudFront
</details>

---

### 問題 5
企業は、安定した高 IOPS が必要なデータベースを実行する EC2 インスタンス向けにブロックストレージを必要としています。どのストレージオプションを使うべきですか？

A. Instance Store  
B. EBS General Purpose SSD (gp3)  
C. EBS Provisioned IOPS SSD (io2)  
D. Amazon EFS  

<details>
<summary>解答を表示</summary>

**Answer: C**

**解説:**
- 高性能データベースには **EBS Provisioned IOPS SSD (io2/io2 Block Express)**
- 一貫した IOPS 性能
- 1 ボリュームあたり最大 64,000 IOPS（io2）
- 最大 256,000 IOPS（io2 Block Express）
- 99.999% durability

**EBS ボリュームタイプ**:

| Type | Use Case | IOPS | Throughput |
|------|----------|------|------------|
| **gp3** | General purpose | 16,000 | 1,000 MB/s |
| **io2** | High-performance databases | 64,000 | 1,000 MB/s |
| **io2 Block Express** | Largest databases | 256,000 | 4,000 MB/s |
| **st1** | Big data, data warehouses | 500 | 500 MB/s |
| **sc1** | Cold storage | 250 | 250 MB/s |

**使い分け**:
- **io2**: 高い一貫 IOPS が必要なデータベース
- **gp3**: ほとんどのワークロード、コスト効率良
- **st1**: スループット重視、シーケンシャル
- **Instance Store**: 一時用途、最高性能

**References:** EBS Volume Types, Provisioned IOPS
</details>

---

### 問題 6
アプリケーションでは、NFS プロトコルを使って複数 AZ にまたがる複数の EC2 インスタンスからアクセス可能な共有ファイルストレージが必要です。どのサービスを使うべきですか？

A. Amazon EBS  
B. Amazon S3  
C. Amazon EFS  
D. Instance Store  

<details>
<summary>解答を表示</summary>

**Answer: C**

**解説:**
- **Amazon EFS**（Elastic File System）の提供内容:
  - 共有 NFS ファイルシステム
  - マルチ AZ アクセス
  - 自動スケーリング
  - Linux 互換（NFS v4）
  - 数千インスタンスから同時アクセス可能

**ストレージサービス比較**:

| Service | Protocol | Multi-Instance | Multi-AZ | Use Case |
|---------|----------|----------------|----------|----------|
| **EBS** | Block | No (single instance) | No | Databases, boot volumes |
| **EFS** | NFS | Yes | Yes | Shared file storage |
| **FSx for Windows** | SMB | Yes | Yes | Windows file shares |
| **S3** | HTTP/S | Yes | Yes | Object storage |

**EFS パフォーマンスモード**:
- **General Purpose**: 低レイテンシ、ほとんどのワークロード向け
- **Max I/O**: 高レイテンシだが大規模並列アクセス向け

**EFS スループットモード**:
- **Bursting**: サイズに応じてスループット拡張
- **Provisioned**: サイズに依存せずスループット指定

**References:** Amazon EFS, Shared File Storage
</details>

---

### 問題 7
企業は、アクセスパターンに基づいて S3 オブジェクトをより安価なストレージクラスへ自動移動したいと考えています。手動管理はしたくありません。何を使うべきですか？

A. S3 Lifecycle Policies  
B. S3 Intelligent-Tiering  
C. Manual scripts  
D. AWS Lambda functions  

<details>
<summary>解答を表示</summary>

**Answer: B**

**解説:**
- **S3 Intelligent-Tiering** はオブジェクトを階層間で自動移動
- アクセスパターンをモニタリング
- IA クラスと異なり取り出し料金なし
- オブジェクトごとに小さな月額モニタリング料金

**Intelligent-Tiering のアクセス階層**:
1. **Frequent Access**: デフォルト、高頻度アクセス
2. **Infrequent Access**: 30 日間アクセスなし
3. **Archive Instant Access**: 90 日間アクセスなし
4. **Archive Access** (optional): 90-270 days no access
5. **Deep Archive Access** (optional): 180-730 days no access

**Intelligent-Tiering vs Lifecycle**:
- **Intelligent-Tiering**: アクセスベースで自動、取り出し料金なし
- **Lifecycle**: ルールベース遷移、スケジュール指定

**使い分け**:
- **Intelligent-Tiering**: アクセスパターンが不明または変動
- **Lifecycle**: パターンが既知（例: 90 日後に Glacier へ）

**References:** S3 Intelligent-Tiering, Automatic Cost Optimization
</details>

---

### 問題 8
企業は S3 オブジェクトを保管時に暗号化する必要があります。暗号鍵の管理は AWS に任せたいです。どの暗号化方式を使うべきですか？

A. SSE-C (Customer-Provided Keys)  
B. SSE-S3 (S3-Managed Keys)  
C. SSE-KMS (KMS-Managed Keys)  
D. Client-Side Encryption  

<details>
<summary>解答を表示</summary>

**Answer: B**

**解説:**
- **SSE-S3** は S3 管理の暗号鍵を使用
- AWS が鍵管理をすべて実施
- AES-256 暗号化
- 追加費用なし
- 最もシンプルな暗号化オプション

**S3 暗号化オプション**:

| Method | Key Management | Cost | Use Case |
|--------|----------------|------|----------|
| **SSE-S3** | AWS manages | Free | Simple encryption |
| **SSE-KMS** | AWS KMS | KMS API costs | Audit trail, key rotation |
| **SSE-C** | Customer provides | Free | Customer controls keys |
| **Client-Side** | Customer | Free | Encrypt before upload |

**SSE-KMS の利点**（必要時）:
- 監査証跡（CloudTrail）
- 鍵ローテーション
- きめ細かなアクセス権限
- Envelope encryption

**試験対策**: 「AWS が鍵管理」「最もシンプル」とあれば SSE-S3 を選ぶ

**References:** S3 Encryption, SSE-S3, SSE-KMS
</details>

---

### 問題 9
企業は、災害対策のため S3 オブジェクトを us-east-1 から eu-west-1 にレプリケートする必要があります。どの機能を有効化すべきですか？

A. S3 Versioning  
B. S3 Cross-Region Replication (CRR)  
C. S3 Same-Region Replication (SRR)  
D. S3 Transfer Acceleration  

<details>
<summary>解答を表示</summary>

**Answer: B**

**解説:**
- **S3 Cross-Region Replication (CRR)** はリージョン間レプリケーション
- 自動・非同期で複製
- 送信元/送信先バケット両方で versioning が必要
- 用途: コンプライアンス、災害対策、レイテンシ削減

**CRR 要件**:
1. 送信元と送信先で versioning を有効化
2. 適切な IAM 権限
3. 異なる AWS リージョン

**CRR vs SRR**:
- **CRR**: 別リージョン、災害対策、コンプライアンス
- **SRR**: 同一リージョン、ログ集約、アカウント間レプリケーション

**レプリケーションオプション**:
- **Replication Time Control (RTC)**: 99.99% を 15 分以内に複製
- **Delete marker replication**: オプション
- **Existing object replication**: 手動バッチ操作

**References:** S3 Cross-Region Replication, Disaster Recovery
</details>

---

### 問題 10
あるアプリケーションが一時データを生成しており、高性能ストレージが必要です。インスタンス停止時にデータ消失しても構いません。どのストレージを使うべきですか？

A. EBS General Purpose SSD  
B. EBS Provisioned IOPS SSD  
C. Instance Store  
D. Amazon EFS  

<details>
<summary>解答を表示</summary>

**Answer: C**

**解説:**
- **Instance Store** の特長:
  - Ephemeral storage（一時）
  - ホストに物理接続
  - 最高 IOPS 性能
  - 追加コストなし
  - インスタンス stop/termination でデータ消失

**Instance Store の特性**:
- **Performance**: 数百万 IOPS も可能
- **Persistence**: stop/terminate で消失
- **Size**: インスタンスタイプ依存
- **Use cases**: キャッシュ、バッファ、一時データ、スクラッチデータ

**Instance Store vs EBS**:
- **Instance Store**: 一時、最高性能、無料
- **EBS**: 永続、ネットワーク接続、stop/start 後も維持

**試験のコツ**: キーワードで判断:
- "Temporary", "can be lost", "cache" → Instance Store
- "Persistent", "database", "survives restart" → EBS

**References:** EC2 Instance Store, Ephemeral Storage
</details>

---

### 問題 11
企業は、削除された S3 オブジェクトを 30 日間復元可能にしたいと考えています。何を有効化すべきですか？

A. S3 Lifecycle Policies  
B. S3 Versioning  
C. S3 Object Lock  
D. MFA Delete  

<details>
<summary>解答を表示</summary>

**Answer: B**

**解説:**
- **S3 Versioning** はオブジェクトの全バージョンを保持
- 削除されたオブジェクトは delete marker になり復元可能
- 過去バージョンを保持
- 誤削除対策になる

**S3 Versioning の機能**:
- 全バージョンを保存（削除含む）
- 誤削除から復旧
- アプリケーション障害から復旧
- 一時停止はできる（完全無効化は不可）
- 各バージョン分のストレージ課金あり

**関連機能**:
- **MFA Delete**: バージョン削除や versioning 停止に MFA 必須
- **Object Lock**: WORM（Write Once Read Many）、コンプライアンス
- **Lifecycle**: 一定期間後にバージョン遷移/削除

**ベストプラクティス**: versioning + lifecycle で古いバージョンを削除

**References:** S3 Versioning, Data Protection
</details>

---

### 問題 12
EC2 上で動作する Windows アプリケーションが、SMB プロトコルでアクセス可能な共有ファイルストレージを必要としています。どのサービスを使うべきですか？

A. Amazon EFS  
B. Amazon FSx for Windows File Server  
C. Amazon EBS  
D. Amazon S3  

<details>
<summary>解答を表示</summary>

**Answer: B**

**解説:**
- **Amazon FSx for Windows File Server**:
  - ネイティブ Windows ファイルシステム
  - SMB プロトコル対応
  - Active Directory 統合
  - Windows NTFS 機能
  - Multi-AZ 配置

**FSx ファミリー**:
- **FSx for Windows**: Windows ワークロード、SMB、AD
- **FSx for Lustre**: HPC、ML、高性能
- **FSx for NetApp ONTAP**: エンタープライズ NAS、マルチプロトコル
- **FSx for OpenZFS**: Linux ワークロード、スナップショット

**ファイルストレージの選択**:
- **Windows apps**: FSx for Windows（SMB）
- **Linux apps**: EFS（NFS）
- **HPC/ML**: FSx for Lustre
- **Block storage**: EBS

**References:** Amazon FSx for Windows, SMB File Shares
</details>

---

### 問題 13
企業は、機械学習トレーニングのためにペタバイト級データを可能な限り高いスループットで保存する必要があります。最も適切なストレージサービスはどれですか？

A. Amazon S3  
B. Amazon EFS  
C. Amazon FSx for Lustre  
D. Amazon EBS  

<details>
<summary>解答を表示</summary>

**Answer: C**

**解説:**
- **FSx for Lustre** は次の用途向けに設計:
  - High-performance computing (HPC)
  - Machine learning
  - Media processing
  - サブミリ秒レイテンシ
  - 数百 GB/s のスループット
  - 数百万 IOPS

**FSx for Lustre の特徴**:
- S3 連携（lazy loading）
- POSIX 準拠ファイルシステム
- Scratch と Persistent のデプロイタイプ
- ペタバイト級までスケール

**デプロイタイプ**:
- **Scratch**: 一時用途、最高性能、レプリケーションなし
- **Persistent**: 長期用途、レプリケーションあり、自動フェイルオーバー

**ML/HPC ストレージ**:
- **Training**: FSx for Lustre（from S3）
- **Inference**: EFS or S3
- **Dataset storage**: S3
- **Processing**: FSx for Lustre

**References:** Amazon FSx for Lustre, HPC Storage
</details>

---

### 問題 14
企業は、低頻度アクセスの EBS スナップショットを自動で安価なストレージに移したいと考えています。どの機能を使うべきですか？

A. S3 Lifecycle Policies  
B. EBS Snapshot Archive  
C. EBS Cold HDD volumes  
D. Amazon Glacier  

<details>
<summary>解答を表示</summary>

**Answer: B**

**解説:**
- **EBS Snapshot Archive** ティア:
  - 標準スナップショットより 75% 安価
  - 90 日以上保存するスナップショット向け
  - 復元時間: 24-72 hours
  - 最低 90 日保存

**EBS スナップショット管理**:
- **Standard**: 高速復元（数分）、高コスト
- **Archive**: 低コスト、低速復元（24-72 hrs）
- lifecycle policies による自動アーカイブ

**EBS スナップショット機能**:
- 増分バックアップ（変更ブロックのみ）
- S3 に保存（AWS 管理）
- リージョン間コピー可能
- 即時復旧向け Fast Snapshot Restore (FSR)

**ユースケース**:
- **Standard**: 頻繁な復元、DR
- **Archive**: コンプライアンス、長期保持

**References:** EBS Snapshots, Snapshot Archive
</details>

---

### 問題 15
アプリケーションは S3 に頻繁にデータを書き込みます。企業はオブジェクト作成時に即時通知を受け取りたいです。何を設定すべきですか？

A. S3 Event Notifications  
B. CloudWatch Logs  
C. AWS Config  
D. CloudTrail  

<details>
<summary>解答を表示</summary>

**Answer: A**

**解説:**
- **S3 Event Notifications** はバケットイベントでトリガー
- ほぼリアルタイム通知
- 送信先: SNS、SQS、Lambda

**S3 イベント**:
- Object created (PUT, POST, COPY, CompleteMultipartUpload)
- Object deleted
- Object restored from Glacier
- Replication events
- Lifecycle transitions

**イベント通知設定例**:
```json
{
  "Event": "s3:ObjectCreated:*",
  "Queue": "arn:aws:sqs:us-east-1:123456789012:MyQueue"
}
```

**一般的なパターン**:
- **S3 → Lambda**: アップロードファイル処理
- **S3 → SQS**: キュー処理
- **S3 → SNS**: 複数購読者へ通知

**代替**: EventBridge（より高度なフィルタリング）

**References:** S3 Event Notifications, Event-Driven Architecture
</details>

---

### 問題 16
企業は、規制コンプライアンスのために S3 オブジェクトが削除・上書きされないようにしたいです。どの機能を使うべきですか？

A. S3 Versioning  
B. S3 Object Lock  
C. MFA Delete  
D. S3 Lifecycle Policies  

<details>
<summary>解答を表示</summary>

**Answer: B**

**解説:**
- **S3 Object Lock** は WORM（Write Once Read Many）を提供
- 指定保持期間中の削除/上書きを防止
- コンプライアンスモードとガバナンスモードを提供

**Object Lock モード**:
- **Compliance**: 誰も（root でも）上書き/削除不可
- **Governance**: 特別権限ユーザーは上書き可能
- **Legal Hold**: 期限なし保護、手動解除

**Object Lock vs Versioning**:
- **Versioning**: 保護できるがバージョン削除は可能
- **Object Lock**: 保持を強制し、真に不変

**要件**:
- Versioning 有効化必須
- バケット作成時に設定
- 保持期間または legal hold 設定

**ユースケース**:
- 金融記録
- 医療データ（HIPAA）
- 法的文書
- 規制コンプライアンス

**References:** S3 Object Lock, WORM Compliance
</details>

---

### 問題 17
アプリケーションが大容量ファイル（100 GB+）を S3 へ最適な性能でアップロードする必要があります。何を使うべきですか？

A. Single PUT operation  
B. S3 Multipart Upload  
C. S3 Transfer Acceleration  
D. AWS DataSync  

<details>
<summary>解答を表示</summary>

**Answer: B**

**解説:**
- **S3 Multipart Upload**:
  - 大きなオブジェクトを分割アップロード
  - パート並列アップロードでスループット向上
  - 一時停止/再開が可能
  - 100 MB 超で推奨
  - 5 GB 超で必須

**Multipart Upload の利点**:
- スループット向上（並列アップロード）
- ネットワーク問題からの復旧が容易
- アップロードの一時停止と再開
- 最終サイズ未確定でも開始可能

**ベストプラクティス**:
- 100 MB 超で利用
- 5 GB 超は必須
- パートを並列アップロード
- 未完了アップロード中断用の lifecycle を設定

**Transfer Acceleration**:
- 別機能（CloudFront edge locations を使用）
- エッジロケーション経由でアップロード高速化
- Multipart Upload と併用可能

**References:** S3 Multipart Upload, Large File Uploads
</details>

---

### 問題 18
企業は Internet Gateway や NAT を使わずに、EC2 インスタンスから S3 へアクセスしたいです。何を設定すべきですか？

A. VPN Connection  
B. AWS Direct Connect  
C. VPC Endpoint for S3 (Gateway Endpoint)  
D. VPC Peering  

<details>
<summary>解答を表示</summary>

**Answer: C**

**解説:**
- **VPC Endpoint for S3**（Gateway Endpoint）:
  - VPC から S3 へのプライベート接続
  - インターネット不要
  - データ転送料不要
  - トラフィックは AWS ネットワーク内に留まる

**VPC Endpoint の種類**:

| Type | Services | Cost | Implementation |
|------|----------|------|----------------|
| **Gateway** | S3, DynamoDB | Free | Route table entry |
| **Interface** | Most AWS services | Hourly + data | ENI in subnet |

**S3 Endpoint の利点**:
- セキュリティ向上（インターネット公開なし）
- 性能向上
- NAT Gateway コスト削減
- endpoint policy でアクセス制御可能

**設定手順**:
1. S3 用 Gateway Endpoint を作成
2. VPC とルートテーブルを選択
3. endpoint policy を設定（任意）
4. S3 トラフィックは自動ルーティング

**References:** VPC Endpoints, S3 Gateway Endpoint, Private Connectivity
</details>

---

### 問題 19
データベースバックアップを S3 に保存しています。必要時に素早く復元できるようにしたいです。どの機能を有効化すべきですか？

A. S3 Transfer Acceleration  
B. S3 Versioning  
C. S3 Cross-Region Replication  
D. Enable S3 Retrieval directly  

<details>
<summary>解答を表示</summary>

**Answer: B**

**解説:**
- バックアップ保護には **S3 Versioning**:
  - 全バージョンへ即時アクセス
  - 誤削除/上書きから保護
  - 高速復旧（ミリ秒取り出し）
  - 複数バックアップバージョンを保持

**バックアップのベストプラクティス**:
1. versioning を有効化
2. 古いバージョンを lifecycle で遷移
3. DR 用に Cross-Region Replication を有効化
4. バックアップオブジェクトへタグ付け
5. 復元手順をテスト

**追加保護**:
- **CRR**: 地理冗長
- **Object Lock**: 不変バックアップ
- **MFA Delete**: 誤削除防止

**Storage Class**: 迅速復元には Standard または Standard-IA

**References:** S3 for Backups, Versioning, Backup Strategies
</details>

---

### 問題 20
企業には S3 Standard に保存されたデータがあります。30 日以内にアクセスされるオブジェクトは Standard に残し、それより古いオブジェクトは S3 Glacier に移動したいです。これを自動化するにはどうすればよいですか？

A. S3 Intelligent-Tiering  
B. S3 Lifecycle Policies  
C. AWS Lambda function  
D. Manual migration  

<details>
<summary>解答を表示</summary>

**Answer: B**

**解説:**
- **S3 Lifecycle Policies** は遷移と期限切れを自動化
- ストレージクラス間をルールベースで遷移
- prefix や tags でフィルタ可能
- コード不要

**Lifecycle Policy 例**:
```json
{
  "Rules": [
    {
      "Id": "MoveToGlacier",
      "Status": "Enabled",
      "Transitions": [
        {
          "Days": 30,
          "StorageClass": "GLACIER"
        }
      ]
    }
  ]
}
```

**Lifecycle アクション**:
- **Transition**: 別ストレージクラスへ移動
- **Expiration**: オブジェクト削除
- **NoncurrentVersionTransition**: 過去バージョンを遷移
- **NoncurrentVersionExpiration**: 過去バージョン削除
- **AbortIncompleteMultipartUpload**: 未完了アップロードをクリーンアップ

**遷移ルール**:
- Standard → Standard-IA（最短 30 日）
- Standard → Glacier（最短 0 日）
- 逆方向遷移は不可（Glacier → Standard）

**References:** S3 Lifecycle Policies, Storage Class Transitions
</details>

---

## まとめ

**Total Questions**: 20  
**Topics Covered**:
- S3 Storage Classes and Use Cases
- S3 Encryption (SSE-S3, SSE-KMS, SSE-C)
- S3 Versioning and Object Lock
- S3 Replication (CRR, SRR)
- S3 Lifecycle Policies
- S3 Event Notifications
- EBS Volume Types (gp3, io2, st1, sc1)
- EBS Snapshots and Archive
- Amazon EFS (Shared NFS File Storage)
- Amazon FSx (Windows, Lustre)
- Instance Store
- VPC Endpoints for S3
- CloudFront with S3

**試験のコツ**:

**S3 ストレージクラス決定ツリー**:
1. **Frequent access** → S3 Standard
2. **Infrequent access (immediate)** → Standard-IA or One Zone-IA
3. **Archive (immediate access)** → Glacier Instant Retrieval
4. **Archive (minutes-hours)** → Glacier Flexible Retrieval
5. **Archive (12+ hours)** → Glacier Deep Archive
6. **Unknown pattern** → Intelligent-Tiering

**EBS ボリュームタイプ**:
- **Databases (high IOPS)** → io2/io2 Block Express
- **General purpose** → gp3
- **Big data (throughput)** → st1
- **Infrequent access** → sc1
- **Temporary** → Instance Store

**ファイルストレージ**:
- **Windows (SMB)** → FSx for Windows
- **Linux (NFS), shared** → EFS
- **HPC/ML** → FSx for Lustre
- **Single instance** → EBS

**S3 機能**:
- **Versioning**: 削除保護、CRR 有効化要件
- **Object Lock**: WORM コンプライアンス
- **Lifecycle**: 遷移/期限切れの自動化
- **CRR**: クロスリージョン災害対策
- **Event Notifications**: Lambda/SQS/SNS トリガー

**コスト最適化**:
- パターン不明なら Intelligent-Tiering
- パターン既知なら Lifecycle policies
- 長期保持は Archive tier
- NAT コスト回避には VPC Endpoint

**パフォーマンス**:
- グローバル配信には CloudFront
- 大容量ファイルには Multipart Upload
- 長距離アップロードには Transfer Acceleration
- プライベート接続には VPC Endpoint

**次のステップ**:
- S3 ストレージクラスの用途とコストを暗記
- 各 EBS ボリュームタイプの使い分けを理解
- ファイルストレージの選択（EFS vs FSx）を整理
- lifecycle policy 設定を実践
