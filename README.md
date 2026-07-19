# Vultr使い方完全ガイド：アカウント登録からサーバー構築まで初心者向け手順解説（東京・大阪リージョン、全プラン料金表、$300無料クーポンまとめ）

はじめてVultr（ヴァルチャー）を開こうとして、英語の管理画面を見て「うわ、これ自分でやれるかな」と一瞬ためらった人、少なくないはず。この記事では、そういう人向けに **vultr 使い方** の全体像を一気通貫で整理する。登録、支払い、サーバー立ち上げ、 SSH 接続、削除まで、順を追っていけば誰でも15分くらいで1台動かせる。

> **Vultr とは**：33カ国・32リージョン（東京 NRT1/NRT2、大阪を含む）にデータセンターを持つクラウド型 VPS で、時間単位の従量課金・月額 $2.50 からのエントリープラン・GPU やベアメタルまでを同一コンソールで扱う、開発者向けパブリッククラウド。

## なぜVultrなのか：国内DC＋時間課金＋幅広いプラン

選ぶ理由は人それぞれだけど、日本在住の筆者が使っていて一番実感するのは「東京と大阪に国内 DC がある」こと。東京は 2026 年 3 月に第 2 データセンター（NRT2）が開設され、NVMe SSD 全プラン対応でディスク I/O が旧 SSD 比で最大 3 倍速くなった。国内向けサービスを載せるなら遅延の観点で頭一つ抜けている。

次に「時間課金」。AWS と同じで使った分だけ。違うのは、最小プランが $2.50/月から始まることで、ブログ1本動かすのに月数千円も払わなくて済む。テスト用にインスタンスを立てて、確認が終わったら destroy して課金ストップ。これが Vultr の基本リズム。

3つ目は品揃え。Cloud Compute（共有 vCPU）、Optimized Cloud Compute（専用 vCPU）、Cloud GPU、Bare Metal、Managed Database、Object Storage、Kubernetes（VKE）まで、コンソール1つで切り替えられる。DigitalOcean や Linode と比べてもカテゴリの幅は広い。👉 [Vultr の全カテゴリを公式で確認する](https://www.vultr.com/pricing/?ref=9738262-9J)

## vultr 使い方：アカウント登録からサーバー削除までの手順

手順は全部で6つ。1つずつ書いていく。

1. **アカウントを作成する**：公式サイト右上の「Sign Up」をクリック。メールアドレスとパスワードを入力し、登録メールの認証リンクを開く。続いて氏名・住所を英語表記（ローマ字）で入力し、クレジットカードまたは PayPal を登録する。
2. **クレジットを追加する**：コンソール右上の「Billing」→「Make Payment」から金額を選んで入金。初回は後述の Match キャンペーンで入金額が2倍になる（上限 $100）。購入金額が少なければこの段階で $25 くらいあれば十分。
3. **サーバーをデプロイする**：左メニュー「Products」→右上「+ Deploy Server」→「Cloud Compute」を選ぶ。次にリージョン（Tokyo または Osaka）、OS（Ubuntu 22.04 LTS が無難）、プラン（まずは $6/月の 1vCPU/1GB で試すのが定番）を選択。
4. **SSH 鍵を登録する**：デプロイ時に SSH Keys 欄で「Add New Key」を押し、ローカルで `ssh-keygen` で作った公開鍵（`~/.ssh/id_rsa.pub` の中身）を貼り付け。これでパスワード入力なしで安全にログインできる。
5. **SSH で接続する**：サーバー一覧に IP アドレスが表示されたら `ssh root@xxx.xxx.xxx.xxx` で入る。初回はフィンガープリント確認に `yes`。入ったらまず `apt update && apt upgrade -y`、続いて一般ユーザー作成と root ログイン禁止を設定するのが定石。
6. **使わなくなったら destroy する**：課金を止めるにはコンソールでサーバーを選び「Server Destroy」をクリック。スナップショットを取らずに破棄するとデータは復元不可。スナップショットは GB あたり $0.05/月で保管できる。

要点を言うと「デプロイはボタン3回、課金停止は destroy 一発」。これさえ覚えておけば失敗しても痛手が小さい。👉 [Vultr アカウントを作成して無料クレジットを受け取る](https://www.vultr.com/?ref=9738262-9J)

## 全プラン料金表：用途別にどれを選ぶか

Vultr の料金体系は「カテゴリ × サイズ」のマトリックス。下の表は 2026 年時点で公式 Pricing ページに掲載されている代表的な構成を抜粋したもの。すべて時間課金・オンデマンド価格。東京リージョンでも同一（DC による価格差なし）。

### Cloud Compute（共有 vCPU・個人〜小規模向け）

| プラン区分 | vCPU | メモリ | ストレージ | 転送量 | 月額 | 購入 |
|---|---|---|---|---|---|---|
| Regular Performance（IPv6 Only） | 1 | 0.5GB | 10GB | 0.5TB | $2.50 |  [この構成でデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#cloud-compute-regular) |
| Regular Performance | 1 | 1GB | 25GB | 1TB | $5 |  [この構成でデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#cloud-compute-regular) |
| Regular Performance | 1 | 2GB | 55GB | 2TB | $10 |  [この構成でデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#cloud-compute-regular) |
| Regular Performance | 2 | 4GB | 80GB | 3TB | $20 |  [この構成でデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#cloud-compute-regular) |
| Regular Performance | 4 | 8GB | 160GB | 4TB | $40 |  [この構成でデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#cloud-compute-regular) |
| High Performance（AMD/Intel） | 1 | 1GB | 25GB NVMe | 2TB | $6 |  [この構成でデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#cloud-compute-high-performance) |
| High Performance | 2 | 4GB | 100GB NVMe | 5TB | $24 |  [この構成でデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#cloud-compute-high-performance) |
| High Performance | 4 | 8GB | 180GB NVMe | 6TB | $48 |  [この構成でデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#cloud-compute-high-performance) |
| High Frequency（3GHz+ Intel） | 1 | 1GB | 32GB NVMe | 1TB | $6 |  [この構成でデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#cloud-compute-high-frequency) |
| High Frequency | 2 | 4GB | 128GB NVMe | 3TB | $24 |  [この構成でデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#cloud-compute-high-frequency) |
| High Frequency | 4 | 16GB | 384GB NVMe | 5TB | $96 |  [この構成でデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#cloud-compute-high-frequency) |

Regular は旧世代 Intel CPU＋通常 SSD で最安。High Performance は最新 AMD EPYC または Intel Xeon ＋ NVMe。High Frequency は 3GHz 以上の Xeon で単核性能重視。迷ったら High Performance の $6/月構成から。

### Optimized Cloud Compute（専用 vCPU・本番運用向け）

| プラン区分 | vCPU | メモリ | NVMe | 転送量 | 月額 | 購入 |
|---|---|---|---|---|---|---|
| General Purpose | 1 | 4GB | 30GB | 4TB | $30 |  [このプランでデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#optimized-cloud-compute) |
| General Purpose | 2 | 8GB | 50GB | 5TB | $60 |  [このプランでデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#optimized-cloud-compute) |
| General Purpose | 4 | 16GB | 80GB | 6TB | $120 |  [このプランでデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#optimized-cloud-compute) |
| CPU Optimized | 1 | 2GB | 25GB | 4TB | $28 |  [このプランでデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#optimized-cloud-compute) |
| CPU Optimized | 4 | 8GB | 75GB | 6TB | $80 |  [このプランでデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#optimized-cloud-compute) |
| Memory Optimized | 1 | 8GB | 50GB | 5TB | $40 |  [このプランでデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#optimized-cloud-compute) |
| Memory Optimized | 4 | 32GB | 200GB | 8TB | $160 |  [このプランでデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#optimized-cloud-compute) |
| Storage Optimized | 1 | 8GB | 150GB | 4TB | $75 |  [このプランでデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#optimized-cloud-compute) |
| Storage Optimized | 4 | 32GB | 640GB | 7TB | $250 |  [このプランでデプロイ](https://www.vultr.com/pricing/?ref=9738262-9J#optimized-cloud-compute) |

専用 vCPU なので隣人の影響を受けない。Web・API サーバーは General Purpose、MySQL などは Memory Optimized、大容量 DB は Storage Optimized という選び方。

### Cloud GPU（AI/ML ワークロード向け）

| GPU 種別 | GPU メモリ | vCPU | RAM | ストレージ | 月額 | 購入 |
|---|---|---|---|---|---|---|
| NVIDIA GH200（36か月予約） | 96GB | 72 | 480GB | 4,800GB | $2,913 |  [Cloud GPU を確認](https://www.vultr.com/pricing/?ref=9738262-9J#cloud-gpu) |
| NVIDIA H100（8 GPU 構成） | 640GB | 224 | 2,048GB | 32,640GB | $13,432 |  [Cloud GPU を確認](https://www.vultr.com/pricing/?ref=9738262-9J#cloud-gpu) |
| NVIDIA A100 PCIe 80GB | 80GB | ― | ― | ― | $2.397/hr |  [Cloud GPU を確認](https://www.vultr.com/pricing/?ref=9738262-9J#cloud-gpu) |

GPU は価格が大きいので、まずは時間単位で試してから月額予約を検討するのが基本。L40S なら $1.671/hr から。 👉 [Vultr の GPU カテゴリを公式で確認する](https://www.vultr.com/pricing/?ref=9738262-9J#cloud-gpu)

### Bare Metal（物理占有・実質専用サーバー）

| 構成目安 | 月額目安 | 購入 |
|---|---|---|
| 入門構成（4コア前後／16〜32GB／NVMe） | $120 〜 |  [Bare Metal を確認](https://www.vultr.com/pricing/?ref=9738262-9J#bare-metal) |
| 中規模構成（8〜16コア／64〜128GB） | $300 〜 |  [Bare Metal を確認](https://www.vultr.com/pricing/?ref=9738262-9J#bare-metal) |

ベアメタルは初期費用無料で物理占有サーバーが使えるが、コスパが良すぎて売り切れやすい。欲しいスペックを見つけたらその日のうちに押さえるのが吉。

一言でまとめると：**個人開発なら High Performance $6/月、本番 web サーバーなら General Purpose $30/月、DB なら Memory Optimized $40/月から**。

## 東京・大阪リージョンの選び方

日本向けサービスなら東京か大阪を選ぶ。両方とも国内 DC なので遅延は10〜30ms程度で収まる。筆者の場合、メインは東京（NRT2 優先・新 SSD で速い）、バックアップ用途や在庫切れ回避のために大阪を1台置く、という使い方をしている。

実測だと scp で 11〜12MB/s が出る報告もあり、回線速度は十分。ただし、1サーバーごとに無料転送量が割り当てられていて、超過分は 1GB あたり $0.01。 блогやAPI程度ならまず超えない。

DC によって提供インスタンスが異なる場合があるので、希少な構成（ベアメタルや特定 GPU）を使いたい場合は在庫状況を都度コンソールで確認する。

## 無料クレジットとクーポン：登録前に知っておきたい

Vultr は新規ユーザー向けに複数の入金ボーナスを出している。2026年7月時点で公式 Coupons ページに掲載されているもの：

- **$300 無料クレジット**：プロモコード `FLY300VULTR`（30日間有効）
- **$250 無料クレジット**：プロモコード `250VULTRFLY`（30日間有効）
- **$200 無料クレジット**：プロモコード `FLYTWOHUNDRED`（30日間有効）
- **入金マッチ**：初回入金をドル対ドルで最大 $100 までマッチ（12か月有効）

登録フローの中でプロモコード欄に入力するか、紹介リンク経由で登録すると自動適用される。キャンペーンは併用不可なので、額が大きい `$300` の方がお得。30日で切れるので、まずは試してみて、継続判断してからカードで本入金する流れが定石。👉 [Vultr で $300 無料クレジットを受け取って試す](https://www.vultr.com/coupons/?ref=9738262-9J)

補足：入金マッチ（Match キャンペーン）は「支払った $100 に対して $100 のボーナスが付与され、合計残高 $200 になる」仕組み。ボーナス分と実資金は 50/50 で消費される。既存ユーザーは対象外。

## 他社 VPS との比較：どこが違うか

DigitalOcean・Linode（現 Akamai）と比べた場合、Vultr の特徴は3点。

1点目は **品揃えの幅**。GPU からベアメタルまで1つのコンソールで扱えるのは3社の中で Vultr だけ。DigitalOcean は GPU を提供しておらず、Linode はベアメタルが限定的。
2点目は **データセンター数**。33リージョンで、日本は東京2拠点＋大阪。Linode は東京1拠点のみ、DigitalOcean は東京1拠点。
3点目は **Vultr Agent など新機能の投入速度**。2025年10月には AMD EPYC 搭載の VX1 Cloud Compute を発表し、ハイパースケーラーの Arm プランと比較して最大82%高いコスパをうたっている。

一方で弱みもある。管理画面が英語のみ、サポートも英語ベース、決済がドル建て。日本語 UI と円決済を重視するなら ConoHa や XSERVER VPS などの国内 VPS が無難。用途と優先順位で切り替えるのが現実的。

## ユーザー評価と第三者レビュー：実際どう見られているか

信頼できる第三者評価を2つ拾った。

**G2（B2Bレビュープラットフォーム）** では、ユーザーから「デプロイの速さと料金の透明性が良い」「サーバー管理がシンプルで迷わない」といったコメントが繰り返し寄せられている。

**Trustpilot** ではスコアにばらつきがあり、料金体系やデプロイの速さを褒める声がある一方で「カスタマーサポートの応答が遅い」「アカウント停止の扱いに不満」という指摘も並ぶ。課金周りのトラブルは事前にカード情報を正確に入れておくことで回避できる部分もある。

**vpsbenchmarks.com** の DigitalOcean / Linode / Vultr 3社比較では、CPU・ディスク性能で Vulcan が安定して上位にランクイン。価格あたりの性能比は3社とも近いが、Vultr は幅広いプランで選択肢が豊富という評価。

実運用の感想を一言で言うと「速くて安くて幅広い。ただし問い合わせは英語で、自分で解決する姿勢が必要」。

## よくある質問（FAQ）

**Q. Vultr に日本（東京）リージョンはありますか？**
A. はい。東京は NRT1 と NRT2 の2拠点、加えて大阪 DC もある。2026年3月に NRT2 が開設され、NVMe SSD 全プラン対応で旧 SSD 比最大3倍のディスク I/O になった。

**Q. 管理画面は日本語に対応していますか？**
A. 対応していない。インターフェースは英語のみ。英語が苦手な場合は Chrome の Google 翻訳プラグインを入れるか、Vultr Agent のチャット機能でドキュメントを日本語で検索するのが現実的な対策。

**Q. 料金の通貨と税金はどうなりますか？**
A. ドル建て。為替レートとカード会社の交換手数料（通常 2.2% 前後）の影響を受ける。契約者住所に応じた税金（日本在住なら消費税）が別途加算される。

**Q. 無料クーポンはありますか？**
A. ある。プロモコード `FLY300VULTR` で $300（30日間）、`250VULTRFLY` で $250（30日間）、`FLYTWOHUNDRED` で $200（30日間）。加えて初回入金マッチキャンペーンが最大 $100 まで適用される（12か月有効）。併用不可。

**Q. 転送量に対する課金はありますか？**
A. あるが、ほぼ課金されない。1アカウントあたり毎月 2TB の無料枠に加え、各サーバーごとに TB 単位の無料枠が付く。超過分は 1GB あたり $0.01。

**Q. ベアメタル（専用サーバー）は使えますか？**
A. はい。初期費用無料・時間課金で物理占有サーバーが使える。ただしコスパが良すぎて売り切れやすく、在庫が戻るのを待つ必要がある場合がある。

**Q. クレジットカード以外の支払い方法はありますか？**
A. PayPal・Alipay・ビットコインに対応。ただしクーポン適用や自動更新の観点ではクレジットカードが最も確実。

**Q. AWS からの移行は現実的ですか？**
A. 現実的。最小プランが AWS の数分の一の価格帯で、EC2 互換の使い勝手がある。コンテナ・Kubernetes（VKE）・Object Storage もあるので、中小規模のワークロードなら移行コストを回収できる。

## まとめ：vultr 使い方 の押さえどころ

ここまでの話を短くまとめる。Vultr は①東京・大阪 DC で国内遅延が小さい、②時間課金で小さく始められる、③VPS から GPU・ベアメタルまで同じコンソールで扱える、④新規なら $300 無料クレジットで実質リスクゼロで試せる。逆に注意する点は、管理画面が英語・決済がドル建て・サポートも英語ベース。これらを割り切れる人には、現時点で最もコスパと自由度が良い選択肢の一つ。

まずは $300 クーポンで1台立てて、destroy までのサイクルを1回体験してみる。それが **vultr 使い方** を身につける最短の近道。

👉 [Vultr 公式ページで $300 無料クレジットを受け取って始める](https://www.vultr.com/?ref=9738262-9J)
