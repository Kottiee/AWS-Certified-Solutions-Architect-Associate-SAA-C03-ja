# IAM - 練習問題

> **⚠️ 免責事項:** これらは AWS ドキュメントに基づき、教育目的で作成された**オリジナル練習問題**です。AWS 認定試験の**実際の試験問題ではありません**。

## 試験標準問題（SAA-C03）

---

### 問題 1
Amazon EC2 インスタンス上で実行されるアプリケーションが、Amazon S3 バケット内のオブジェクトにアクセスする必要があります。このアクセスを付与する**最も安全な**方法はどれですか？

A. プログラムによるアクセスを持つ IAM ユーザーを作成し、アクセスキーを EC2 インスタンスに保存する  
B. S3 権限を持つ IAM ロールを作成し、EC2 インスタンスにアタッチする  
C. S3 バケットをパブリックにして匿名アクセスを許可する  
D. EC2 インスタンスでルートアカウント認証情報を使用する  

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- IAM ロールは自動的にローテーションされる一時的なセキュリティ認証情報を提供する
- 長期認証情報を管理したり埋め込んだりする必要がない
- 最も安全で AWS 推奨のアプローチ
- 選択肢 A: アクセスキーは長期認証情報であり、漏えい時にセキュリティリスクが高い
- 選択肢 C: セキュリティのベストプラクティスに反する
- 選択肢 D: アプリケーションでルート認証情報を使ってはいけない
- **ベストプラクティス**: EC2 インスタンスでは常に IAM ロールを使用する

**参考:** IAM ロール、EC2 インスタンスプロファイル、AWS セキュリティのベストプラクティス
</details>

---

### 問題 2
企業が外部監査人に対して、S3 内の CloudTrail ログを確認するための一時的アクセスを付与する必要があります。アクセスは 7 日後に期限切れになる必要があります。**最適な**ソリューションはどれですか？

A. 監査人用に IAM ユーザーを作成し、7 日後に削除する  
B. IAM ロールを作成し、AWS STS を使用して監査人に一時的なセキュリティ認証情報を提供する  
C. ルートアカウント認証情報を 7 日間共有する  
D. 7 日間有効な S3 の事前署名 URL を作成する  

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- AWS STS（Security Token Service）は一時的な認証情報を生成する
- IAM ロールはフェデレーションを通じて外部ユーザーが引き受け可能
- セッション期間に基づいて認証情報は自動的に期限切れになる
- 選択肢 A: 手動削除が必要で、運用負荷が発生する
- 選択肢 C: ルート認証情報の共有は厳禁
- 選択肢 D: 事前署名 URL も有効だが、IAM ロールの方が包括的
- **一時アクセスに最適**: IAM ロールと STS

**参考:** AWS STS、一時的セキュリティ認証情報、クロスアカウントアクセス
</details>

---

### 問題 3
ソリューションアーキテクトが、"production-data" という特定バケットを除くすべての S3 バケットへのアクセスを拒否する IAM ポリシーを作成しています。どのポリシー要素を使用すべきですか？

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::production-data",
        "arn:aws:s3:::production-data/*"
      ]
    },
    {
      "Effect": "Deny",
      "Action": "s3:*",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "s3:ResourceAccount": "${aws:PrincipalAccount}"
        }
      }
    }
  ]
}
```

正しいアプローチはどれですか？

A. 特定バケットのみに Allow 効果を使用する  
B. 1 つを除くすべてのバケットに Deny 効果を使用する  
C. 特定バケットに Allow を設定し、それ以外は暗黙の Deny に任せる  
D. S3 バケットにリソースベースポリシーを使用する  

<details>
<summary>解答を表示</summary>

**解答: C**

**解説:**
- IAM は明示的に許可されていない操作をデフォルトで Deny する
- "production-data" バケットへのアクセスのみを明示的に Allow する
- その他のバケットは暗黙的に拒否される
- その他に対する明示的 Deny ステートメントは不要
- **IAM 評価ロジック**: 明示的 Deny > Allow > 暗黙的 Deny
- よりシンプルなポリシー: 特定バケットへの Allow のみ

**正しいポリシー**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::production-data",
        "arn:aws:s3:::production-data/*"
      ]
    }
  ]
}
```

**参考:** IAM ポリシー評価ロジック、S3 IAM ポリシー
</details>

---

### 問題 4
企業では、すべての IAM ユーザーが S3 オブジェクトを削除する前に Multi-Factor Authentication（MFA）を使用しなければならない要件があります。どのように強制できますか？

A. S3 バケットで MFA Delete を有効化する  
B. MFA を要求する条件付き IAM ポリシーを作成する  
C. AWS Organizations の Service Control Policies を使用する  
D. MFA を要求する S3 バケットポリシーを設定する  

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- IAM ポリシー条件 `aws:MultiFactorAuthPresent` で MFA を強制できる
- `s3:DeleteObject` など特定アクションに適用可能

**ポリシー例**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:DeleteObject",
      "Resource": "arn:aws:s3:::my-bucket/*",
      "Condition": {
        "Bool": {
          "aws:MultiFactorAuthPresent": "true"
        }
      }
    }
  ]
}
```

- 選択肢 A: MFA Delete はバージョニング保護向けで、バケット所有者の MFA が必要
- 選択肢 C: SCP でも強制可能だが、IAM ポリシーの方が直接的
- 選択肢 D: 機能する場合はあるが、標準的なのは IAM ポリシー

**参考:** IAM ポリシー条件、IAM ポリシーでの MFA
</details>

---

### 問題 5
開発チームは、すべての EC2 インスタンスへの読み取り専用アクセスを必要としていますが、書き込みアクセスは営業時間（午前 9 時〜午後 5 時）のみ許可したいと考えています。どのように実装しますか？

A. 時間ベースの IAM ポリシー条件を使用する  
B. 営業時間中にポリシーを手動でアタッチ/デタッチする  
C. AWS Lambda を使って時間に応じて IAM ポリシーを変更する  
D. 2 つの IAM グループを作成し、ユーザーを移動させる  

<details>
<summary>解答を表示</summary>

**解答: A**

**解説:**
- IAM は `aws:CurrentTime` を用いた時間ベース条件をサポートしている
- ポリシー評価に日時範囲を指定できる

**ポリシー例**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:*",
      "Resource": "*",
      "Condition": {
        "DateGreaterThan": {"aws:CurrentTime": "2026-01-01T09:00:00Z"},
        "DateLessThan": {"aws:CurrentTime": "2026-01-01T17:00:00Z"}
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:Describe*",
        "ec2:Get*",
        "ec2:List*"
      ],
      "Resource": "*"
    }
  ]
}
```

- 読み取り専用アクセスは常時許可
- 書き込みアクセスは指定時間のみ許可
- 他の選択肢は手動対応や複雑な自動化が必要

**参考:** IAM ポリシー条件、時間ベースアクセス制御
</details>

---

### 問題 6
企業が、ユーザー自身のパスワード変更のみを許可し、それ以外は許可したくありません。どの AWS 管理ポリシーをアタッチすべきですか？

A. IAMFullAccess  
B. IAMUserChangePassword  
C. IAMReadOnlyAccess  
D. PowerUserAccess  

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- `IAMUserChangePassword` はセルフサービスのパスワード変更用 AWS 管理ポリシー
- 最小権限の原則に従っている
- ユーザー自身のパスワード管理のみを許可する
- 選択肢 A: 権限が広すぎる（IAM フルアクセス）
- 選択肢 C: 読み取り専用のためパスワード変更不可
- 選択肢 D: IAM 以外ほぼフルアクセス

**代替案**: カスタムポリシーを作成
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iam:ChangePassword",
        "iam:GetAccountPasswordPolicy"
      ],
      "Resource": "arn:aws:iam::*:user/${aws:username}"
    }
  ]
}
```

**参考:** AWS 管理ポリシー、セルフサービスのパスワード管理
</details>

---

### 問題 7
組織は複数の AWS アカウントを保有しています。すべてのアカウントで権限を一元管理したい場合、**最適な**ソリューションはどれですか？

A. 各アカウントで同一の IAM ポリシーを作成する  
B. Service Control Policies（SCP）を伴う AWS Organizations を使用する  
C. 各アカウントで IAM ロールを使用する  
D. アカウント間で IAM ユーザーを共有する  

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- Service Control Policies（SCP）は AWS アカウント全体の権限境界を設定する
- 組織、OU、アカウントレベルで適用可能
- マスター/管理アカウントから集中管理できる
- 利用可能な最大権限を制御し、個別 IAM ポリシーは引き続き必要
- 選択肢 A: 一元管理ではなく、保守が困難
- 選択肢 C: ロールはクロスアカウントアクセスに有効だが、集中権限管理ではない
- 選択肢 D: IAM ユーザーはアカウント間共有不可

**SCP 例**（us-east-1 以外の全リージョンを拒否）:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "*",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:RequestedRegion": "us-east-1"
        }
      }
    }
  ]
}
```

**参考:** AWS Organizations、Service Control Policies
</details>

---

### 問題 8
企業は SAML 2.0 を使用して、社員が社内 Active Directory 認証情報で AWS にアクセスできるようにしています。これはどのアクセス種別ですか？

A. IAM ユーザーアクセス  
B. ルートアカウントアクセス  
C. フェデレーテッドアクセス  
D. プログラムによるアクセス  

<details>
<summary>解答を表示</summary>

**解答: C**

**解説:**
- フェデレーションにより、既存の社内認証情報を使って AWS へアクセスできる
- SAML 2.0 は ID フェデレーションの標準
- ユーザーは社内 IdP（Active Directory など）で認証される
- IdP は IAM ロール引き受けを通じて一時的 AWS 認証情報を提供する
- 各社員向けに IAM ユーザーを作成する必要がない
- **フェデレーションの種類**: SAML 2.0、OpenID Connect、カスタム Identity Broker

**フェデレーションのフロー**:
1. ユーザーが社内 IdP で認証する
2. IdP が SAML アサーションを生成する
3. ユーザーがそのアサーションを AWS STS に提示する
4. STS が一時認証情報を返す
5. ユーザーが一時認証情報で AWS にアクセスする

**参考:** ID フェデレーション、SAML 2.0、AWS STS
</details>

---

### 問題 9
IAM ポリシーに関する記述として正しいものはどれですか？

A. ID ベースポリシーはリソースにアタッチされる  
B. リソースベースポリシーは IAM ID にアタッチされる  
C. ID ベースポリシーはユーザー、グループ、ロールの権限を定義する  
D. リソースベースポリシーは他アカウントのプリンシパルを指定できない  

<details>
<summary>解答を表示</summary>

**解答: C**

**解説:**
- **ID ベースポリシー**: ユーザー、グループ、ロールにアタッチ（誰が何をできるか）
- **リソースベースポリシー**: S3、SQS などのリソースにアタッチ（誰がこのリソースへアクセスできるか）
- 選択肢 A: 誤り。ID ベースポリシーは ID にアタッチされる
- 選択肢 B: 誤り。リソースベースポリシーはリソースにアタッチされる
- 選択肢 D: 誤り。リソースベースポリシーはクロスアカウントのプリンシパルを指定可能

**例**:
- ID ベース: IAM ユーザーポリシー、ロールポリシー
- リソースベース: S3 バケットポリシー、KMS キーポリシー、Lambda 関数ポリシー

**参考:** IAM ポリシーの種類、ID ベースとリソースベースポリシー
</details>

---

### 問題 10
Lambda 関数が DynamoDB テーブルにアクセスする必要があります。権限を付与する正しい方法はどれですか？

A. Lambda 関数用に IAM ユーザーを作成する  
B. Lambda 関数用の IAM 実行ロールを作成する  
C. Lambda 関数に直接権限を追加する  
D. DynamoDB でリソースベースポリシーを使用する  

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- Lambda 実行ロールは関数がアクセスできる対象を定義する
- 関数実行時に Lambda サービスがロールを引き受ける
- ポリシーをロールにアタッチする（例: AmazonDynamoDBFullAccess またはカスタムポリシー）
- 選択肢 A: Lambda 関数は IAM ユーザーを使用しない
- 選択肢 C: そのような仕組みはなく、IAM ロールを使用する必要がある
- 選択肢 D: DynamoDB は S3 のようなリソースベースポリシーを持たない

**Lambda 実行ロールのポリシー例**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:PutItem",
        "dynamodb:GetItem",
        "dynamodb:Query"
      ],
      "Resource": "arn:aws:dynamodb:us-east-1:123456789012:table/MyTable"
    }
  ]
}
```

**参考:** Lambda 実行ロール、サービスロール
</details>

---

### 問題 11
次の IAM ポリシー条件は何を行いますか？

```json
"Condition": {
  "IpAddress": {
    "aws:SourceIp": "203.0.113.0/24"
  }
}
```

A. 指定した IP 範囲からのアクセスのみを許可する  
B. 指定した IP 範囲からのアクセスを拒否する  
C. 指定した IP 範囲からのアクセスをログ記録する  
D. 指定した IP 範囲を経由してトラフィックをルーティングする  

<details>
<summary>解答を表示</summary>

**解答: A**

**解説:**
- この条件は、指定 IP 範囲からのリクエストにのみポリシー効果を適用する
- "Effect": "Allow" と組み合わせると、その IP からのみアクセスを許可
- "Effect": "Deny" と組み合わせると、その IP からのアクセスを拒否
- ログ記録やルーティングは行わない
- **ユースケース**: 企業ネットワーク IP へのアクセス制限

**完全なポリシー例**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": "*",
      "Condition": {
        "IpAddress": {
          "aws:SourceIp": "203.0.113.0/24"
        }
      }
    }
  ]
}
```

**参考:** IAM ポリシー条件、IP ベースアクセス制御
</details>

---

### 問題 12
企業は IAM ユーザーが t3.medium より大きい EC2 インスタンスを作成できないようにしたいと考えています。どのように強制できますか？

A. インスタンスタイプを確認する条件付き IAM ポリシーを使用する  
B. AWS Config ルールを使用する  
C. AWS Budgets を使用する  
D. EC2 インスタンス制限を使用する  

<details>
<summary>解答を表示</summary>

**解答: A**

**解説:**
- IAM ポリシー条件で EC2 インスタンスタイプを制限できる
- 条件キー: `ec2:InstanceType`

**ポリシー例**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:RunInstances",
      "Resource": "arn:aws:ec2:*:*:instance/*",
      "Condition": {
        "StringEquals": {
          "ec2:InstanceType": ["t3.micro", "t3.small", "t3.medium"]
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": "ec2:RunInstances",
      "Resource": [
        "arn:aws:ec2:*:*:subnet/*",
        "arn:aws:ec2:*:*:network-interface/*",
        "arn:aws:ec2:*:*:volume/*",
        "arn:aws:ec2:*:*:key-pair/*",
        "arn:aws:ec2:*:*:security-group/*",
        "arn:aws:ec2:*::image/*"
      ]
    }
  ]
}
```

- 選択肢 B: Config ルールは準拠状況を検出するが、操作を防止しない
- 選択肢 C: Budgets はコスト監視であり、操作制限はしない
- 選択肢 D: サービス制限はアカウント全体のクォータであり、ポリシー制限ではない

**参考:** IAM ポリシー条件、EC2 インスタンスタイプ制限
</details>

---

### 問題 13
1 人の IAM ユーザーが所属できる IAM グループの最大数はいくつですか？

A. 5  
B. 10  
C. 20  
D. 無制限  

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- IAM ユーザーは最大 10 グループまで所属可能
- これは AWS のサービス制限/クォータ
- グループは権限管理を簡素化する
- より複雑な権限が必要な場合は、複数ポリシーやロールを使用する

**IAM 制限（試験で重要なもの）**:
- アカウントあたりユーザー数: 5,000（デフォルト）
- アカウントあたりグループ数: 300
- ユーザーあたりグループ数: 10
- ユーザー/グループ/ロールあたり管理ポリシー数: 10
- インラインポリシーサイズ: ユーザー 2,048 文字、ロール 10,240 文字

**参考:** IAM クォータと制限
</details>

---

### 問題 14
開発者が誤って AWS アクセスキーを公開 GitHub リポジトリにコミットしてしまいました。**直ちに**実施すべきことは何ですか？

A. IAM ユーザーのパスワードを変更する  
B. GitHub リポジトリを削除する  
C. 露出したアクセスキーを無効化して削除する  
D. アカウントで MFA を有効化する  

<details>
<summary>解答を表示</summary>

**解答: C**

**解説:**
- 露出した認証情報は直ちに無効化/削除する必要がある
- 侵害されたキーによる不正アクセスを防止できる
- 手順: キー無効化 → 新キー作成 → 旧キー削除 → CloudTrail 確認
- 選択肢 A: パスワードはアクセスキーとは別物
- 選択肢 B: リポジトリ削除は、すでにクローン/索引化されていれば効果が薄い
- 選択肢 D: MFA はすでに露出したキーを保護できない

**インシデント対応手順**:
1. 侵害されたキーを直ちに無効化
2. CloudTrail で不正アクティビティを確認
3. 新しいアクセスキーを作成
4. 侵害されたキーを削除
5. コードリポジトリをスキャン
6. シークレット管理を導入（AWS Secrets Manager、Parameter Store）

**参考:** AWS セキュリティのベストプラクティス、インシデント対応
</details>

---

### 問題 15
信頼ポリシーとアクセス許可ポリシーの両方を持てる IAM エンティティはどれですか？

A. IAM User  
B. IAM Group  
C. IAM Role  
D. IAM Policy  

<details>
<summary>解答を表示</summary>

**解答: C**

**解説:**
- **IAM ロール**には 2 種類のポリシーがある:
  - **信頼ポリシー（Trust Policy）**: 誰がロールを引き受けられるか（principal）を定義
  - **アクセス許可ポリシー（Permissions Policy）**: ロールが何をできるか（actions）を定義
- ユーザーとグループには信頼ポリシーがない
- ポリシーはドキュメントであり、他ポリシーを持つエンティティではない

**信頼ポリシー例**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

**アクセス許可ポリシー例**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

**参考:** IAM ロール、信頼ポリシー、ロール引き受け
</details>

---

### 問題 16
企業は、IAM ユーザーを作成せずにサードパーティベンダーへ特定の S3 バケットアクセスを提供する必要があります。**最適な**アプローチはどれですか？

A. ルートアカウント認証情報を共有する  
B. External ID 付きクロスアカウント IAM ロールを作成する  
C. S3 バケットをパブリックにする  
D. S3 事前署名 URL を使用する  

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- クロスアカウントロールにより、外部アカウントがリソースへアクセスできる
- External ID は Confused Deputy 問題を防ぐ追加のセキュリティ層
- ベンダーは自社 AWS アカウントからロールを引き受ける
- IAM ユーザーを管理する必要がない

**設定手順**:
1. 自アカウントに IAM ロールを作成
2. 信頼ポリシーでベンダーの AWS アカウントを許可
3. セキュリティのため External ID を含める
4. ベンダーが STS AssumeRole でロールを引き受ける

**External ID を含む信頼ポリシー**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::VENDOR-ACCOUNT-ID:root"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "sts:ExternalId": "unique-external-id-123"
        }
      }
    }
  ]
}
```

**参考:** クロスアカウントアクセス、External ID、Confused Deputy 問題
</details>

---

### 問題 17
IAM ポリシーでアクションが明示的に許可も拒否もされていない場合のデフォルト効果は何ですか？

A. Allow  
B. Deny  
C. 承認を求める  
D. アクションをログ記録する  

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- IAM はデフォルトで **暗黙的拒否（implicit deny）** を使用する
- 明示的に許可されない限り、すべてのアクションは拒否される
- **評価ロジック**: 明示的 Deny → Allow → 暗黙的 Deny
- 明示的 Deny は Allow で上書きできない
- アクション実行には明示的 Allow が必要

**ポリシー評価フロー**:
1. Deny 評価: いずれかのポリシーに明示的 Deny があれば → DENY
2. Organizations SCP: 権限境界として適用
3. リソースベースポリシー: 評価
4. ID ベースポリシー: いずれかに Allow があれば → ALLOW
5. IAM permissions boundaries: フィルターとして適用
6. セッションポリシー: 引き受けロールに適用
7. デフォルト: DENY（明示的 Allow がない場合）

**参考:** IAM ポリシー評価ロジック
</details>

---

### 問題 18
企業が、未認証ユーザーに対して S3 オブジェクトへの 1 時間の一時アクセスを付与したいと考えています。何を使用すべきですか？

A. IAM ユーザー認証情報  
B. S3 事前署名 URL  
C. S3 バケットポリシー  
D. IAM ロール  

<details>
<summary>解答を表示</summary>

**解答: B**

**解説:**
- 事前署名 URL は S3 オブジェクトへの時間制限付きアクセスを提供する
- エンドユーザーに AWS 認証情報は不要
- URL に認証情報が含まれる
- 指定した期間後に期限切れとなる

**事前署名 URL の生成（AWS CLI）**:
```bash
aws s3 presign s3://my-bucket/object.pdf --expires-in 3600
```

**ユースケース**:
- 一時的なダウンロードリンク
- 未認証ユーザー向けアップロードフォーム
- パブリック化せずにプライベートコンテンツを共有
- リソースへの時間制限付きアクセス

- 選択肢 A: ユーザー作成が必要で、一時/匿名用途に不向き
- 選択肢 C: バケットポリシー自体は時間制限付きではない
- 選択肢 D: ロールは認証を要する

**参考:** S3 事前署名 URL、一時アクセス
</details>

---

### 問題 19
IAM permissions boundaries の目的は何ですか？

A. IAM エンティティが持てる最大権限を設定する  
B. ポリシーを超える追加権限を付与する  
C. IAM ポリシーを置き換える  
D. IAM 認証情報を暗号化する  

<details>
<summary>解答を表示</summary>

**解答: A**

**解説:**
- permissions boundaries は最大権限の上限を設定する
- 境界で許可されない権限は付与できない
- ID ポリシーで許可されても、境界で制限できる
- **ユースケース**: 権限管理を安全に委任する

**例のシナリオ**:
- 開発者に permissions boundary を設定
- 開発者は Lambda 用 IAM ロールを作成可能
- ただしロール権限は boundary を超えられない
- 権限昇格を防止できる

**有効権限** = Identity Policy ∩ Permissions Boundary

```json
// Permissions Boundary
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:*",
        "ec2:*",
        "lambda:*"
      ],
      "Resource": "*"
    }
  ]
}
```

ID ポリシーが IAM アクションを許可していても、boundary によって防止される。

**参考:** IAM Permissions Boundaries、委任管理
</details>

---

### 問題 20
次のうち、AWS ベストプラクティスに従ってルートアカウントで MFA を要求すべき操作はどれですか？（2 つ選択）

A. AWS コンソールへのログイン  
B. アカウント設定の変更  
C. EC2 インスタンスの起動  
D. S3 バケットの削除  
E. 請求情報の表示  

<details>
<summary>解答を表示</summary>

**解答: A, B**

**解説:**
- AWS ベストプラクティス: ルートアカウントでは MFA を即時有効化
- ルートアカウントは MFA で保護すべき
- 機微な操作には MFA が必要
- アカウント設定変更など一部操作は root + MFA が必要

**ルートアカウントのベストプラクティス**:
1. ✅ ルートアカウントで MFA を有効化
2. ✅ 日常業務でルートを使わない
3. ✅ ルートアクセスキーを削除（代わりに IAM ユーザーを使用）
4. ✅ 管理作業用の IAM ユーザーを作成
5. ✅ ルート必須の作業にのみルートを使用
6. ✅ ルート認証情報を安全に保管（パスワードマネージャー）

**ルートアカウントが必要なタスク**:
- アカウント設定の変更
- AWS アカウントの解約
- AWS Support プランの変更
- IAM ユーザー権限の復旧
- Reserved Instance Marketplace で販売者登録
- MFA Delete 向け S3 バケット設定
- GovCloud へのサインアップ

**参考:** ルートアカウントのベストプラクティス、MFA、AWS アカウントセキュリティ
</details>

---

## まとめ

**問題数合計**: 20  
**対象トピック**:
- IAM ロールと一時認証情報（STS）
- IAM ポリシー（ID ベース、リソースベース、条件）
- Multi-Factor Authentication（MFA）
- クロスアカウントアクセスと External ID
- ID フェデレーション（SAML 2.0）
- Service Control Policies（SCP）
- IAM ベストプラクティス
- Permissions Boundaries
- 事前署名 URL
- ルートアカウントセキュリティ

**試験対策のポイント**:
1. AWS サービスには**アクセスキーより IAM ロールを優先**
2. 一時認証情報には**STS を使用**
3. **ポリシー評価**: 明示的 Deny > Allow > 暗黙的 Deny
4. **ルートアカウント**: MFA を有効化し、日常利用しない
5. **フェデレーション**: 既存の社内認証情報を利用
6. **クロスアカウント**: サードパーティには External ID 付きロール
7. **Permissions boundaries**: 最大権限の上限を設定
8. **グループ上限**: ユーザーは最大 10 グループ
9. **事前署名 URL**: 未認証ユーザーへの一時アクセス
10. **最小権限の原則**: 必要最小限の権限のみ付与

**試験でよくあるシナリオ**:
- EC2 から S3 アクセス → IAM ロールを使用
- 一時的な外部アクセス → IAM ロール + STS
- 社内 SSO → SAML 2.0 フェデレーション
- サードパーティベンダー → External ID 付きクロスアカウントロール
- 時間制限付き匿名アクセス → 事前署名 URL
- アクション制限 → IAM ポリシー条件
- マルチアカウントガバナンス → SCP 付き AWS Organizations

**次のステップ**:
- IAM ポリシー評価ロジックを復習する
- 条件付き IAM ポリシー作成を練習する
- ID ベースとリソースベースポリシーの違いを理解する
- クロスアカウントアクセスパターンを学習する
- AWS セキュリティのベストプラクティスを復習する
