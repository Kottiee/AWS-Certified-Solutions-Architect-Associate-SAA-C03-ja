# Monitoring & Management - 練習問題

> **⚠️ 免責事項:** これらは AWS ドキュメントを基に教育目的で作成された**オリジナルの練習問題**です。AWS 認定試験の**実際の試験問題ではありません**。

## 試験レベルの問題 (SAA-C03)

***

### 問題 1
ある企業が EC2 インスタンスのメモリ使用率を監視し、メモリ使用率が 80% を超えた際にアラームをトリガーしたいと考えています。ソリューションアーキテクトが取るべき組み合わせは次のうちどれですか？

A. EC2 インスタンスの詳細モニタリングを有効にし、デフォルトのメモリメトリクスに CloudWatch アラームを作成する
B. EC2 インスタンスに CloudWatch Agent をインストールし、メモリメトリクスを送信するよう設定して、CloudWatch アラームを作成する
C. AWS Systems Manager Session Manager を使用してメモリメトリクスを確認し、手動でアラートを作成する
D. CloudTrail ログを有効にし、CloudWatch Logs Insights を使用してメモリ使用率をクエリする

<details>
<summary>答えを見る</summary>

**答え: B**

**解説:**
- EC2 インスタンスはデフォルトではメモリメトリクスを送信しない
- メモリメトリクスを収集するには CloudWatch Agent のインストールが必要
- エージェントが CloudWatch にメトリクスを送信した後、アラームを作成できる
- A はメモリがデフォルトメトリクスでないため不正解
- C は自動アラートを提供しない
- D は API 呼び出しの監査用であり、パフォーマンスメトリクス用ではない

**参考:** CloudWatch Agent、EC2 モニタリング、カスタムメトリクス
</details>

***

### 問題 2
セキュリティチームが、先週重要な EC2 インスタンスを終了した IAM ユーザーを特定する必要があります。使用すべき AWS サービスはどれですか？

A. Amazon CloudWatch Logs
B. AWS Config
C. AWS CloudTrail
D. AWS Systems Manager

<details>
<summary>答えを見る</summary>

**答え: C**

**解説:**
- CloudTrail は誰が実行したかを含む API 呼び出しを記録する
- TerminateInstances などの管理イベントを追跡する
- CloudTrail ログにはユーザー ID、タイムスタンプ、アクションが含まれる
- A (CloudWatch Logs) はアプリケーションログを監視するもので API 呼び出しではない
- B (Config) はリソース構成を追跡するもので誰が変更したかではない
- D (Systems Manager) は運用管理用

**参考:** CloudTrail、API 呼び出し監査、ガバナンス
</details>

***

### 問題 3
ある企業が全ての S3 バケットでバージョニングが有効になっていることを確保し、この要件に違反した際に自動通知を受け取りたいと考えています。使用すべき AWS サービスはどれですか？

A. AWS CloudTrail と CloudWatch Logs
B. カスタムメトリクスを使用した Amazon CloudWatch
C. AWS マネージドルールを使用した AWS Config
D. AWS Systems Manager State Manager

<details>
<summary>答えを見る</summary>

**答え: C**

**解説:**
- AWS Config はルールに照らしてリソース構成を評価する
- マネージドルール `s3-bucket-versioning-enabled` がバージョニングを確認する
- Config は非準拠時に SNS 通知を送信できる
- CloudTrail は誰が変更したかを追跡するが、コンプライアンスを評価しない
- CloudWatch はパフォーマンスメトリクスを監視するもので、構成コンプライアンスではない
- Systems Manager State Manager は EC2 インスタンスの構成管理用

**参考:** AWS Config、Config ルール、コンプライアンス監査
</details>

***

### 問題 4
ソリューションアーキテクトが、ポート 22 を開放したり SSH キーを管理したりせずに EC2 インスタンスにアクセスしてトラブルシューティングを行う必要があります。この機能を提供する AWS サービスはどれですか？

A. AWS CloudShell
B. AWS Systems Manager Session Manager
C. Amazon EC2 Instance Connect
D. AWS Direct Connect

<details>
<summary>答えを見る</summary>

**答え: B**

**解説:**
- Session Manager は SSH キーやオープンポートなしでセキュアなシェルアクセスを提供する
- アクセス制御に IAM 権限を使用する
- セッションログを S3 または CloudWatch Logs に送信できる
- EC2 Instance Connect はポート 22 の開放が必要
- CloudShell は AWS CLI コマンドの実行用で、インスタンスへのアクセス用ではない
- Direct Connect はネットワーク接続用

**参考:** Systems Manager Session Manager、セキュアアクセス
</details>

***

### 問題 5
ある企業が、スケジュールされたメンテナンスウィンドウ中にフリート内の全ての EC2 インスタンスを自動的にパッチ適用したいと考えています。使用すべき AWS サービスはどれですか？

A. カスタムスクリプトを使用した AWS CloudFormation
B. AWS Systems Manager Patch Manager
C. Lambda を使用した Amazon EventBridge
D. 修復アクションを使用した AWS Config

<details>
<summary>答えを見る</summary>

**答え: B**

**解説:**
- Patch Manager は OS およびアプリケーションのパッチ適用を自動化する
- メンテナンスウィンドウでパッチ適用のタイミングを定義する
- パッチベースラインでインストールするパッチを指定する
- CloudFormation はインフラのコード化用であり、パッチ適用用ではない
- EventBridge はパッチ適用をトリガーできるが、専用サービスではない
- Config はコンプライアンスを評価するがパッチ適用は行わない

**参考:** Systems Manager Patch Manager、メンテナンスウィンドウ
</details>

***

### 問題 6
あるアプリケーションが CloudWatch Logs にログデータを書き込んでいます。運用チームは「ERROR」という単語が 5 分間に 10 回以上出現した際にアラートを受け取る必要があります。どのように設定すべきですか？

A. CloudWatch Logs Insights を使用してエラーをクエリし、手動で確認する
B. 「ERROR」の出現回数をカウントするメトリクスフィルターを作成し、そのメトリクスにアラームを作成する
C. ログを S3 にエクスポートし、Athena を使用してエラーをクエリする
D. CloudTrail を使用してエラーイベントを追跡し、SNS 通知を作成する

<details>
<summary>答えを見る</summary>

**答え: B**

**解説:**
- メトリクスフィルターはログデータからメトリクスを抽出する
- フィルターパターンで「ERROR」の出現回数をカウントできる
- CloudWatch アラームがカスタムメトリクスでトリガーできる
- これにより自動化されたリアルタイムのアラートが実現する
- A は手動介入が必要
- C は不必要な複雑さを加えており、リアルタイムではない
- CloudTrail は API 呼び出し用であり、アプリケーションログ用ではない

**参考:** CloudWatch Logs、メトリクスフィルター、CloudWatch アラーム
</details>

***

### 問題 7
ある企業が複数の AWS アカウントおよびリージョンにまたがるセキュリティグループへの全ての構成変更を追跡する必要があります。最も効率的なソリューションはどれですか？

A. 各アカウントおよびリージョンで CloudTrail を有効にする
B. セキュリティグループの変更を監視する Lambda 関数を作成する
C. Config Aggregator を使用した AWS Config を利用する
D. 各リージョンで CloudWatch Events を使用する

<details>
<summary>答えを見る</summary>

**答え: C**

**解説:**
- AWS Config はリソースの構成変更を記録する
- Config Aggregator はアカウントおよびリージョンをまたいだ一元化されたビューを提供する
- 構成履歴と関連性を追跡する
- CloudTrail は誰が変更したかを追跡するが、構成を集約しない
- Lambda はカスタム開発と保守が必要
- CloudWatch Events は変更を検出できるが、履歴追跡は提供しない

**参考:** AWS Config、Config Aggregator、マルチアカウント管理
</details>

***

### 問題 8
ソリューションアーキテクトが SSH アクセスなしでフリート全体の全 EC2 インスタンスにスクリプトを実行する必要があります。スクリプトはセキュリティアップデートをインストールするものです。この機能を提供するサービスはどれですか？

A. AWS Systems Manager Run Command
B. EC2 API を使用した AWS Lambda
C. Amazon CloudWatch Events
D. AWS Config Remediation

<details>
<summary>答えを見る</summary>

**答え: A**

**解説:**
- Run Command はマネージドインスタンスにリモートでコマンドを実行する
- SSH 不要で、IAM 権限を使用する
- レート制御とエラー処理を提供する
- コマンド履歴が CloudTrail に記録される
- Lambda は Run Command を呼び出せるが、直接的なソリューションではない
- CloudWatch Events は Run Command をトリガーできるが、実行サービスではない
- Config Remediation は SSM オートメーションドキュメントを使用する

**参考:** Systems Manager Run Command、フリート管理
</details>

***

### 問題 9
ある組織が、EC2 インスタンス作成の急増など異常な API アクティビティを検出したいと考えています。有効にすべき CloudTrail の機能はどれですか？

A. CloudTrail データイベント
B. CloudTrail 管理イベント
C. CloudTrail Insights イベント
D. CloudTrail マルチリージョントレイル

<details>
<summary>答えを見る</summary>

**答え: C**

**解説:**
- CloudTrail Insights は機械学習を使用して異常なアクティビティを検出する
- リソースプロビジョニングや IAM アクションの急増などの異常を特定する
- 管理イベントは API 呼び出しを追跡するが、異常を検出しない
- データイベントは大量のオペレーション (S3 オブジェクト、Lambda 呼び出し) を追跡する
- マルチリージョントレイルはログを収集するがパターンを分析しない

**参考:** CloudTrail Insights、異常検出
</details>

***

### 問題 10
ある企業がコンプライアンス要件を満たすために CloudWatch Logs を 10 年間保存する必要があります。最もコスト効率の高いアプローチはどれですか？

A. CloudWatch Logs に 10 年間の保持期間を設定してログを保存する
B. ログを S3 にエクスポートし、S3 Glacier Deep Archive に移行する
C. ログを S3 にエクスポートし、S3 Intelligent-Tiering を使用する
D. ログを Kinesis Data Firehose にストリーミングし、Redshift に保存する

<details>
<summary>答えを見る</summary>

**答え: B**

**解説:**
- CloudWatch Logs の長期保存はコストが高い
- コスト効率の高い長期保存のために S3 にエクスポートする
- S3 Glacier Deep Archive はアーカイブに最も安価 (GB あたり月 $0.00099)
- S3 Intelligent-Tiering は Glacier Deep Archive より高価
- Redshift は分析用であり、コスト効率の高いアーカイブには不向き

**参考:** CloudWatch Logs エクスポート、S3 Glacier Deep Archive、コスト最適化
</details>

***

### 問題 11
開発チームが問題のトラブルシューティングのためにアプリケーションログをクエリする必要があります。ログは CloudWatch Logs に保存されています。アドホックなログ分析に使用すべき機能はどれですか？

A. CloudWatch メトリクス
B. CloudWatch Logs Insights
C. CloudWatch ダッシュボード
D. CloudWatch アラーム

<details>
<summary>答えを見る</summary>

**答え: B**

**解説:**
- CloudWatch Logs Insights はインタラクティブなログ分析を提供する
- ログの検索と分析のための専用クエリ言語がある
- エラーの検索、イベントのカウント、パーセンタイルの計算が可能
- メトリクスは数値パフォーマンスデータ用
- ダッシュボードは可視化するがクエリは行わない
- アラームはしきい値でトリガーされる

**参考:** CloudWatch Logs Insights、ログ分析
</details>

***

### 問題 12
ある企業が非準拠リソースを自動的に修復したいと考えています。例えば、暗号化なしで S3 バケットが作成された場合に自動的に暗号化されるようにしたいと考えています。この要件を実現するソリューションはどれですか？

A. SSM オートメーションドキュメントを使用した自動修復を備えた AWS Config ルール
B. Lambda 関数を使用した CloudWatch Events
C. SNS 通知を使用した AWS CloudTrail
D. Systems Manager State Manager

<details>
<summary>答えを見る</summary>

**答え: A**

**解説:**
- Config ルールはコンプライアンスを評価する
- 自動修復は SSM オートメーションドキュメントを使用する
- リソースが非準拠になった際に修復をトリガーできる
- CloudWatch Events も機能するが、Config はコンプライアンス専用に構築されている
- CloudTrail は変更を追跡するだけで修復しない
- State Manager は EC2 設定を維持するもので S3 ではない

**参考:** AWS Config、自動修復、SSM オートメーション
</details>

***

### 問題 13
ソリューションアーキテクトが、EC2 インスタンスおよび Lambda 関数からアクセスできるデータベースパスワードなどの機密設定データを保存する必要があります。ソリューションは暗号化とバージョン履歴をサポートする必要があります。使用すべきサービスはどれですか？

A. AWS Secrets Manager
B. AWS Systems Manager Parameter Store
C. バージョニングを有効にした Amazon S3
D. AWS Config

<details>
<summary>答えを見る</summary>

**答え: B**

**解説:**
- Parameter Store は設定データとシークレットを安全に保存する
- KMS による暗号化をサポートする
- バージョン履歴を維持する
- EC2、Lambda、CloudFormation と統合されている
- Secrets Manager も有効だが高価 (自動ローテーションを含む)
- 試験の文脈では、Parameter Store は Systems Manager の一部
- S3 は構成管理用に設計されていない
- Config はコンプライアンス追跡用

**参考:** Systems Manager Parameter Store、シークレット管理
</details>

***

### 問題 14
ある企業がマルチリージョンアプリケーションを持っており、全リージョンの CloudWatch メトリクスを表示する統合ダッシュボードを作成する必要があります。これは可能ですか？

A. いいえ、CloudWatch ダッシュボードはリージョン固有のみ
B. はい、CloudWatch ダッシュボードはクロスリージョンメトリクスをサポートする
C. はい、ただし CloudWatch Logs のみで、メトリクスではない
D. はい、ただしデータを集約するために CloudWatch Events が必要

<details>
<summary>答えを見る</summary>

**答え: B**

**解説:**
- CloudWatch ダッシュボードはクロスリージョンおよびクロスアカウントのビューをサポートする
- 複数のリージョンのグラフを単一のダッシュボードに追加できる
- 分散アプリケーションのグローバルビューが可能
- 追加の集約サービスは不要

**参考:** CloudWatch ダッシュボード、クロスリージョンモニタリング
</details>

***

### 問題 15
運用チームが、インストール済みアプリケーション、OS の詳細、ネットワーク構成など、全 EC2 インスタンスに関するメタデータを収集する必要があります。使用すべき Systems Manager の機能はどれですか？

A. Systems Manager Session Manager
B. Systems Manager Inventory
C. Systems Manager Patch Manager
D. Systems Manager Run Command

<details>
<summary>答えを見る</summary>

**答え: B**

**解説:**
- Systems Manager Inventory はマネージドインスタンスからメタデータを収集する
- OS、アプリケーション、ネットワーク構成に関する情報を収集する
- Inventory ダッシュボードでクエリおよび可視化できる
- Session Manager はシェルアクセス用
- Patch Manager はパッチ適用用
- Run Command はコマンドの実行用

**参考:** Systems Manager Inventory、メタデータ収集
</details>

***

### 問題 16
ある企業がコンプライアンス監査のために CloudTrail ログが改ざんされていないことを確保する必要があります。有効にすべき機能はどれですか？

A. CloudTrail マルチリージョントレイル
B. CloudTrail ログファイル整合性検証
C. CloudTrail Insights
D. CloudTrail データイベント

<details>
<summary>答えを見る</summary>

**答え: B**

**解説:**
- ログファイル整合性検証はデジタル署名を使用する
- 配信後にログが変更されていないことを確認する
- コンプライアンスおよびフォレンジック調査に必要
- マルチリージョントレイルはリージョンをまたいでログを有効化する
- Insights は異常なアクティビティを検出する
- データイベントはリソースオペレーションを追跡する

**参考:** CloudTrail、ログ整合性、コンプライアンス
</details>

***

### 問題 17
ソリューションアーキテクトが CloudWatch Logs をリアルタイムで処理し、フィルタリングされたデータを分析アプリケーションに送信する必要があります。使用すべきソリューションはどれですか？

A. ログを S3 にエクスポートし、Athena を使用する
B. Kinesis Data Streams を使用した CloudWatch Logs サブスクリプションを利用する
C. スケジュールされたクエリを使用した CloudWatch Logs Insights を利用する
D. ログを S3 にエクスポートし、Lambda を使用する

<details>
<summary>答えを見る</summary>

**答え: B**

**解説:**
- CloudWatch Logs サブスクリプションはリアルタイム処理を可能にする
- リアルタイム分析のために Kinesis Data Streams に送信できる
- Kinesis Data Firehose および Lambda もサポートしている
- S3 エクスポートはリアルタイムではない (バッチ処理)
- Logs Insights はアドホッククエリ用であり、ストリーミング用ではない

**参考:** CloudWatch Logs サブスクリプション、リアルタイム処理
</details>

***

### 問題 18
ある企業が特定の IAM ポリシーがいつロールにアタッチされたかを追跡し、完全な構成履歴を表示したいと考えています。この機能を提供するサービスはどれですか？

A. AWS CloudTrail のみ
B. AWS Config のみ
C. CloudTrail と Config の両方
D. IAM Access Analyzer

<details>
<summary>答えを見る</summary>

**答え: C**

**解説:**
- CloudTrail は誰がポリシーをアタッチしたか、いつ行ったかを示す (API 呼び出しの詳細)
- Config は構成履歴と変更のタイムラインを示す
- 両サービスは完全な可視性のために補完し合う
- CloudTrail: 「誰が何をいつしたか」
- Config: 「現在および過去にわたってどのような状態か」
- IAM Access Analyzer は外部アクセスのためのリソースポリシーを分析する

**参考:** CloudTrail と Config の比較、構成履歴
</details>

***

### 問題 19
あるアプリケーションが EC2 インスタンスの望ましい状態を維持し、特定のソフトウェアが常にインストールおよび実行されていることを確保する必要があります。使用すべき Systems Manager の機能はどれですか？

A. Systems Manager Run Command
B. Systems Manager State Manager
C. Systems Manager Automation
D. Systems Manager Patch Manager

<details>
<summary>答えを見る</summary>

**答え: B**

**解説:**
- State Manager は望ましい状態の構成を維持する
- ドキュメントとインスタンスの間のアソシエーションを作成する
- 継続的に構成を適用し続ける
- Run Command は一回限りのコマンドを実行する
- Automation はワークフローを実行する
- Patch Manager はパッチ適用を処理する

**参考:** Systems Manager State Manager、構成管理
</details>

***

### 問題 20
ある企業がセキュリティインシデントを調査するために 5 年分の CloudTrail ログをクエリする必要があります。最も効率的なソリューションはどれですか？

A. S3 から全ログをダウンロードし、ローカルツールを使用する
B. Amazon Athena を使用して S3 の CloudTrail ログをクエリする
C. SQL を使用して CloudTrail Lake でログをクエリする
D. ログを Elasticsearch にインポートする

<details>
<summary>答えを見る</summary>

**答え: C**

**解説:**
- CloudTrail Lake は CloudTrail ログのクエリのために専用に構築されている
- SQL を使用してイベントをクエリする
- 最大 7 年間イベントを保持できる
- 複数のアカウントおよびリージョンからログを集約する
- Athena も機能するが、CloudTrail Lake はこのユースケースに最適化されている
- A は非効率でスケーラブルではない
- Elasticsearch は不必要な複雑さを加える

**参考:** CloudTrail Lake、ログクエリと分析
</details>

***

## まとめ

### テストされる主要概念:
1. **CloudWatch**: メトリクス、ログ、アラーム、ダッシュボード、エージェント
2. **CloudTrail**: API 監査、誰が/何を/いつ、ログ整合性
3. **AWS Config**: 構成追跡、コンプライアンスルール、修復
4. **Systems Manager**: Session Manager、Patch Manager、Run Command、State Manager、Parameter Store、Inventory
5. **サービス比較**: CloudTrail vs Config vs CloudWatch

### 試験のヒント:
- ✅ EC2 のメモリメトリクスには CloudWatch Agent が必要
- ✅ 「誰が変更したか」には CloudTrail
- ✅ 「どのような状態か」には Config
- ✅ Session Manager は SSH キーとオープンポートの必要性を排除する
- ✅ メトリクスフィルターはログデータからメトリクスを作成する
- ✅ Config Aggregator はマルチアカウント/リージョンのコンプライアンス用
- ✅ CloudTrail Insights は異常な API アクティビティを検出する
- ✅ Parameter Store は設定とシークレット管理用
