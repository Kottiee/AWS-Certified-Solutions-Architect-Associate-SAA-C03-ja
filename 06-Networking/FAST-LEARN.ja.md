
⚡ Fast Learning - ネットワーキング & コンテンツ配信

完了時間: 75-90分 | 試験配点: 約20-25%

🎯 必須理解コンセプト（5分）

ネットワーク基礎 (VPC-SING)

VPC → 仮想プライベートクラウド（専用ネットワーク）
SUBNETS → VPCをセグメントに分割（パブリック/プライベート）
INTERNET GATEWAY → インターネット接続
NAT GATEWAY → プライベートサブネットのアウトバウンド通信
ROUTE TABLES → トラフィックのルーティングルール
SECURITY GROUPS → インスタンス単位のファイアウォール（ステートフル）
NACLs → サブネット単位のファイアウォール（ステートレス）

記憶法: 「VPC Secures Internet Networks Globally」

📊 クイックリファレンステーブル

VPCコンポーネント早見表

Component	Level	State	デフォルト	Allow/Deny
Security Group	インスタンス	ステートフル	すべてのインバウンド拒否	Allow のみ
NACL	サブネット	ステートレス	すべて許可	Allow & Deny
Route Table	サブネット	N/A	ローカルルート	ルートのみ
Internet Gateway	VPC	N/A	なし	すべてのトラフィック
NAT Gateway	AZ	N/A	なし	アウトバウンドのみ

Security Group vs NACL（重要！）

Feature	Security Group	NACL
レベル	インスタンス（ENI）	サブネット
ルール	Allowのみ	Allow & Deny
状態	ステートフル（戻り通信自動許可）	ステートレス（両方向必要）
評価順序	全ルール評価	番号順
デフォルト	インバウンド拒否、アウトバウンド許可	全許可
関連付け	複数可	サブネットごとに1つ

記憶法: 「SG = Stateful Good guy（Allowのみ）、NACL = Stateless Number rules（Allow/Deny）」

🔥 試験頻出トピック

1. VPC CIDRブロック

有効範囲: /16 ～ /28
例: 10.0.0.0/16 = 65,536 IP

プライベートIP範囲（RFC1918）
├── 10.0.0.0/8
├── 172.16.0.0/12
└── 192.168.0.0/16

予約IP（各サブネット：最初4つ + 最後1つ）
10.0.0.0/24 の例:
├── 10.0.0.0   - ネットワークアドレス
├── 10.0.0.1   - VPCルーター
├── 10.0.0.2   - DNSサーバー
├── 10.0.0.3   - 予約
└── 10.0.0.255 - ブロードキャスト（未使用だが予約）

利用可能IP数 = 合計 - 5

2. パブリック vs プライベートサブネット

パブリックサブネット
├── IGWへのルートあり
├── パブリックIP付与
├── 用途: Webサーバー、LB
└── インターネットからアクセス可能

プライベートサブネット
├── IGWへの直接ルートなし
├── NAT Gatewayでアウトバウンド通信
├── 用途: DB、アプリ
└── 直接アクセス不可

重要: Route Tableで決まる！

3. VPC接続オプション

Option	用途	帯域	コスト
Internet Gateway	公開アクセス	無制限	無料
NAT Gateway	Private→Internet	45Gbps	$$$
VPC Peering	VPC間接続	制限なし	$
Transit Gateway	複数VPCハブ	50Gbps	$$
VPN	オンプレ接続	最大1.25Gbps	$
Direct Connect	専用回線	1-100Gbps	$$$
PrivateLink	プライベート接続	10Gbps	$$

4. Route 53ルーティングポリシー

Policy	用途	動作
Simple	単一	1つ返す
Weighted	A/Bテスト	割合分配
Latency	高速	最低遅延
Failover	DR	ヘルスチェック
Geolocation	地域別	ユーザー位置
Geoproximity	距離ベース	距離+バイアス
Multi-value	複数IP	複数返す

💡 よくある試験シナリオ

Scenario 1: EC2がインターネットに接続できない

チェック:
	1.	パブリックサブネットか？
	2.	パブリックIPあるか？
	3.	SGアウトバウンド許可？
	4.	NACL双方向OK？
	5.	ルート0.0.0.0/0 → IGW？

Scenario 2: プライベートサブネットからインターネット

答え: NAT Gateway + ルート設定

Scenario 3: 複数VPC接続

答え: Transit Gateway

Scenario 4: オンプレ接続
	•	速い/暗号化 → VPN
	•	高速/専用 → Direct Connect
	•	両方 → DX + VPN

Scenario 5: 特定IPブロック

答え: NACL or WAF

Scenario 6: 地域別ルーティング

答え: Geolocation

Scenario 7: 他アカウントへ公開

答え: PrivateLink

🎓 速習のコツ

VPC Flow Logs

収集: ENIのIP通信
保存: CloudWatch or S3
レベル: VPC/サブネット/ENI
用途: トラブルシュート

取得不可:
❌ メタデータ
❌ DHCP
❌ DNS
❌ Windows認証

VPC Peeringルール

✅ クロスリージョン可
✅ クロスアカウント可
❌ トランジティブ不可
❌ CIDR重複不可
❌ エッジ間ルーティング不可

CloudFront

CDN
400+エッジロケーション

用途:
├ 静的(S3)
├ 動的(API)
├ HTTPS
└ DDoS防御

オリジン:
├ S3
├ EC2
├ ALB
├ HTTP
└ MediaPackage

セキュリティ:
├ OAI
├ 署名URL
├ Geo制限
└ WAF

📝 ラピッドファクト

サブネットサイズ

CIDR	合計	使用可能	用途
/28	16	11	小
/27	32	27	小
/26	64	59	中
/25	128	123	中
/24	256	251	一般
/20	4096	4091	大
/16	65536	65531	最大

NAT比較

Feature	Gateway	Instance
管理	AWS	自分
可用性	高	スクリプト
帯域	45Gbps	依存
コスト	高	低
推奨	✅	❌

VPCエンドポイント

Interface
├ ENI
├ 多サービス
├ 有料

Gateway
├ ルート
├ S3/DynamoDBのみ
├ 無料

記憶法: Gateway = 無料、Interface = 有料

Direct Connect
	•	1/10/100Gbps
	•	構築に時間
	•	暗号化なし
	•	高スループット

🚀 5分レビュー

判断ツリー

公開 → IGW
非公開 → NAT
VPC接続 → Peering/Transit
オンプレ → VPN/DX
IPブロック → NACL/WAF
DNS → Route53

セキュリティ

✅ DBはプライベート
✅ SGを主に使用
✅ 多層防御
✅ Flow Logs有効
✅ エンドポイント使用

よくあるミス

❌ IP予約忘れ
❌ NAT Instance使用
❌ NAT配置ミス
❌ Peering誤解
❌ CIDR重複
❌ NACL片方向のみ
❌ SGでdeny
❌ S3にEndpoint未使用

🎯 クイック問題
	1.	予約IP数?
	2.	SGは?
	3.	SGで拒否?
	4.	最大CIDR?
	5.	無料Endpoint?
	6.	Peeringはtransitive?
	7.	HA NATは?
	8.	DR用Route53?

⸻

Global Accelerator vs CloudFront

機能	CloudFront	GA
目的	キャッシュ	ルーティング
用途	HTTP	TCP/UDP
キャッシュ	あり	なし
IP	変動	固定

Elastic IP
	•	固定IPv4
	•	停止後も保持
	•	再割当可能
	•	5個制限
	•	未使用は課金

⏱️ 次のステップ
	•	学習: 75-90分
	•	演習: VPC構築
	•	次: Module 07

⸻

解答:
	1.	5
	2.	ステートフル
	3.	できない
	4.	/16
	5.	Gateway
	6.	なし
	7.	NAT Gateway
	8.	Failover