---
title: "Cloudflareでドメインを取得して個人でGoogle Workspaceを契約しよう"
emoji: "⛅️"
type: "idea" # tech: 技術記事 / idea: アイデア
topics:
  - "googleworkspace"
  - "cloudflare"
  - "domain"
  - "個人開発"
published: false
publication_name: "nttdata_tech"
---

## はじめに

エンジニアの方もエンジニアではない方も、こんにちは。現在は金融業界で SRE としてGoogle CloudのプロジェクトにJoinしていましたが、最近プロジェクトが変わり、AWSに挑戦しています。
先日は[Google Cloud Partner Top Engineer 2026](
https://cloud.google.com/blog/ja/topics/partners/announcing-the-winners-of-the-google-cloud-partner-top-engineer-2026-award-program) に選出していただきました！

みなさんは、Google Workspace (以下GWS) を契約していますか？GWSを契約し、各種機能を利用するにあたり、独自ドメインを取得・登録する必要があります。独自ドメインを取得する方法はたくさんありますが、私は (金額や運用も含めて) 最もコストがかからないのは**Cloudflareでドメインを取得する方法**だと思っています (あくまでも個人の見解です)。
私はCloudflareで `.org`ドメインを年間$10で契約して、GWSは月額1,900円で契約しています。**なぜ、Cloudflareでドメインを取得することになったのか、ドメイン取得からGWSの契約の手順を簡単に解説しようと思います**。

## 想定読者

- 個人でGoogle Workspaceを利用したく、費用をなるべく抑えたい人
  - ビジネスを始める人
  - Gemini/NotebookLMにどっぷり浸かりたい人
- Cloudflareの機能を利用してみたい人

### なぜGoogle Workspaceを契約すべきなのか
まず、そもそもなぜGWSを個人で契約しようとしたのか、GWSを契約すると何が良いのかも簡単に言及しておきます。私はエンジニアなので、エンジニア視点でのGWSの良い面について言及しますが、非エンジニアの方でもオススメできるものです！
(GWSの良さを知っている人は読み飛ばしてください！)

まず、以下の画像をご覧ください。
Googleのエコシステムの画像ですが、AIを中心に据えて、多種多様なサービスを展開しています。Google内の多種多様なサービス内でGeminiを利用することができるということです！
:::message
最近はGoogle PhotoやGoogle MapにもGeminiが搭載されていたり、GeminiのリソースとしてNotebookLMが利用できたりと、**Geminiで扱える範囲はさらに拡大しています**。
:::
![google-ecosystem](/images/gws-using-cloudflare/google-ecosystem.jpeg)
*Googleのエコシステム*

元々、[Google AI Pro](https://one.google.com/intl/ja_jp/about/google-ai-plans/)を契約していました。こちらの契約は月額2,900円です。私は[Standardプラン](https://workspace.google.co.jp/pricing?hl=ja)を契約している (月額契約で1,900円) のですが、契約単体で見ると1,000円近くコストカットできます。また、契約した当時にGWSのアルファ機能としてGoogle Workspace Flows (現在は[Google Workspace Studioというサービス名に変更されています](https://dev.classmethod.jp/articles/trying-google-workspace-flows-alpha/))というサービスがリリースされ、使ってみたいと思いGWSに移行しようと思い立ちました。
Google AI Proを契約していたので、Google系のサービスは以前から課金して利用していました。Gemini / NotebookLM / Google Drive etc...を利用しており、特にNotebookLMにはお世話になりました。[^1]Google AI ProからGWSへ移行しましたが、使用感としてはあまり変わらない印象です。

GWSの良さを挙げると以下かなと個人的には思っています。

- ビジネスで必須のサービスからAI系のサービスまでオールインワンで利用できる
- 特にGeminiとの親和性が高く、各種サービスからGeminiを利用でき、GWS横断でGeminiを呼び出せる
- GWSのアルファ機能を利用できる (上述したような枠での機能利用)
- Google Cloud内で組織 (Organization) を作成可能になる[^2]
  - 組織を作成可能なことにより、利用できる機能が増える

### なぜCloudflareでドメインを取得すべきなのか
詳細な解説は以下の方の説明がとてもわかりやすいので、ぜひ読んでみてください。
https://note.com/true_se_tips/n/n04578a748781
上記を踏まえつつ、私なりに感じるメリットを記載しようと思います。

- ドメインが安い (原価で売っていて、値上がりしないと明言されている)
- GWS契約時のDNSレコードの設定がとても楽ちん
- Cloudflareのその他の機能も無料枠があり、Webサイトも簡単に作れる

#### ドメインが安い (原価で売っていて、値上がりしないと明言されている)
この話はCloudflareでドメインを登録する際に、よく出てくる話ですね。Cloudflareは公式 ([ドメイン登録と管理 / Cloudflare](https://www.cloudflare.com/ja-jp/products/registrar/)) で
:::message
透明性のある登録料および更新料
Registrarは想定外の料金や不要なアドオンを排除します。登録によって請求される登録料と更新料のみをお支払いください。
:::
と明言されています。

他社との細かい料金の比較は割愛しますが、GWS経由でドメインを登録する場合は、今回取得した`.org`ドメインで最初の1年間は**年額**1,400円、1年経過後はSquarespace[^3]のサブスクリプションで更新されるそうです。これがどこのサブスクリプションを意味しているか実際に契約しないとわからないですが。[こちらのサイト](https://domains.squarespace.com/)で購入するドメインの料金になるのか、[こちらのサイト](https://ja.squarespace.com/pricing?channel=pbr&subchannel=go&campaign=pbr-go-row_japan-multi-core_general-mix&subcampaign=(price-jp_squarespace-%E4%BE%A1%E6%A0%BC_phr)&gclsrc=aw.ds&gad_source=1&gad_campaignid=23337823389&gbraid=0AAAAADxS_FIPUnW0Aj0k4NMVwZ1ASiRv0&gclid=Cj0KCQiA4eHLBhCzARIsAJ2NZoJ-oghsUkJSoATpK0LLjmDyD_IoKwYCfRtbbAnpQZ4wDpVRzxo4j6kaAq-2EALw_wcB)のサブスクリプションの価格体系になるのかわかりません。
(情報ある方いらっしゃれば情報いただきたいです)
![gws-first-year-cost](/images/gws-using-cloudflare/gws-domain-first-year-cost.png)
*契約時のドメイン購入画面の例*

![gws-domain-cost](/images/gws-using-cloudflare/gws-domain-cost.png)
*[Google サービスのお申込時にドメインを購入する | Google Workspace管理者ヘルプ](https://support.google.com/a/answer/53929?hl=ja&sjid=4769540298305338798-NC#zippy=%2Csquarespace-%E3%81%8B%E3%82%89%E8%B3%BC%E5%85%A5%E3%81%99%E3%82%8B%E3%83%A1%E3%83%AA%E3%83%83%E3%83%88)より引用*

つまり、1年目は年額1,400円ですが、2年目以降はSquarespaceに依存した価格体系になります！実際に契約してみないとわからん！！

#### GWS契約時のDNSレコードの設定がとても楽ちん
手順自体は後述しますが、GWS契約する際にTXTレコード[^4] / MXレコード[^5] / SPFレコード[^6]をCloudflareで取得したドメインに紐づける必要があります。これを**とても簡単に**実施できます。Google側で公開している手順は[こちら](https://support.google.com/a/answer/16018515?sjid=4769540298305338798-NC&visit_id=639051660988172574-4184249832&rd=1)にありますが、公開された手順など不要なくらい簡単です。Google Workspace管理者コンソールとCloudflareのダッシュボードを行き来する必要があるのですが、それもすべてGWS契約時のコンソールをポチポチすれば終わります。所要時間は5分程度です。
GWS経由でドメインを取得した場合は、この設定はすでに実施されておりこの手間が省けるのですが、この手間を省くほどに価格に魅力があるかわかりません。

#### Cloudflareのその他の機能も無料枠があり、Webサイトも簡単に作れる
この話はCloudflare自体の機能についてなので、他の方がたくさん言及されているので簡単に紹介します。
Cloudflareは上述したDNSなどのネットワークのサービスだけかと思いきや、その他にもサービスを展開しています。

- Cloudflare Pages
- Cloudflare Workers
- Cloudflare R2

個人開発などの文脈で上記のサービスをよく耳にします。どれも無料枠があり、無料枠内であればいくら使っても無料です。([公式ページ](https://www.cloudflare.com/ja-jp/plans/developer-platform/)) なので、これから個人開発をしたい方にもうってつけです。GWS内のAIを利用して開発し、Cloudflareで公開することも簡単にできてしまいます。実際にCloudflareを利用して個人開発をしているテックブログがあったので、いくつか参考として載せておきます。
https://zenn.dev/matsubokkuri/articles/cloudflare-service
こちらは開発の実績というよりも開発のガイドラインのようなブログです。参考になりますので、読んでみてください。
https://izanami.dev/post/b0f59b2e-dd6b-4352-af1d-ae14f7cec707

もちろん、ネットワーク関連のサービスにも無料枠が存在しています。今回はCloudflare Registrar経由でドメインを購入しています。(Cloudflare Registrar利用自体に料金はかかりませんが、ドメイン購入/更新に料金が発生します) その他にもトラフィック分析やパフォーマンス分析の機能は無料で利用可能です。
https://www.cloudflare.com/ja-jp/products/registrar/

## Cloudflareでドメインを取得

ここでは Cloudflare Registrar を使って独自ドメインを取得する手順を、ざっくり紹介します。
 (UIは変更されることがあるので、雰囲気が伝わればOKというスタンスで書きます)

:::message
Cloudflare Registrar は、取得できるTLD (`.com` / `.org` など) が決まっています。
希望のTLDが表示されない場合は、Cloudflare Registrarの対応外なので、別のレジストラで取得してください。
:::

### 1. Cloudflare にログインして Registrar でドメインを検索

Cloudflare にログイン後、Registrar (ドメイン登録) から取得したいドメイン名を検索します。
空きがあればそのまま購入へ進めます。

:::message
この時点で支払い方法 (クレカ等) の登録が求められます。
:::

### 2. 購入完了を確認

購入が完了すると、対象ドメインの「購入完了」画面に遷移します。

![cloudflare-domain-purchased](/images/gws-using-cloudflare/cloudflare-domain-purchased.png)
*Cloudflare - ドメイン購入完了画面*

### 3. ドメインの概要 (Overview) を確認

購入後は、ドメインの管理画面 (Overview) から、DNS 設定や各種セットアップへの導線が確認できます。

![cloudflare-domain-overview](/images/gws-using-cloudflare/cloudflare-domain-overview.png)
*Cloudflare - ドメイン概要画面*

### 4. DNS が編集できる状態になっていることを確認

Cloudflare Registrar で取得したドメインは、基本的に Cloudflare の DNS を使って運用することになります。
 (Google Workspace の所有権証明や Gmail の MX 設定も、ここにレコードが追加されます)

![cloudflare-dns-settings](/images/gws-using-cloudflare/cloudflare-dns-settings.png)
*Cloudflare - DNS設定画面*

次の章で、この DNS に対して Google Workspace のレコード (TXT / MX / SPF) を追加していきます。

## Google WorkspaceをCloudflareのドメインで契約する方法

ここからが本題です。
「Google の申込み画面でドメインを購入する」のではなく、**Cloudflareで取得した既存ドメインを使う**流れで進めます。

### 1. 管理コンソールのセットアップを開始

申込み完了後、管理コンソール (セットアップウィザード) から設定を進めます。

![gws-admin-setup-start](/images/gws-using-cloudflare/gws-admin-setup-start.png)
*Google管理コンソール - セットアップ開始画面*

### 2. ドメイン設定 (2ステップ) を確認

ドメイン設定は大きく **(1) 所有権の証明** と **(2) Gmail の有効化** の2つです。

![gws-setting-domain-first](/images/gws-using-cloudflare/gws-setting-domain-first.png)
*Google Workspace - ドメイン設定開始画面*

### 3. 所有権の証明 (TXT レコード)

画面の案内に従い、DNS プロバイダとして Cloudflare を選択します。
Cloudflare にログインしてオーソライズすると、TXT レコードが追加されます。

![gws-setting-domain-check-ownership](/images/gws-using-cloudflare/gws-setting-domain-check-ownership.png)
*Google Workspace - Cloudflare 連携による所有権証明*

Cloudflare 側では、追加される DNS レコードの確認画面が出ます。

![cloudflare-google-dns-txt-verify](/images/gws-using-cloudflare/cloudflare-google-dns-txt-verify.png)
*Cloudflare - TXTレコード追加の確認画面*

同意して進めると、Google 側でドメインの準備が進みます。

![gws-domain-confirm](/images/gws-using-cloudflare/gws-domain-confirm.png)
*Google Workspace - ドメイン確定画面*

![gws-domain-preparing](/images/gws-using-cloudflare/gws-domain-preparing.png)
*Google Workspace - ドメイン準備中画面*

所有権が確認できると次に進めるようになります。

![gws-domain-ownership-verified](/images/gws-using-cloudflare/gws-domain-ownership-verified.png)
*Google Workspace - ドメイン所有権の確認完了*

### 4. Gmail の有効化 (MX / SPF レコード)

続けて Gmail を有効化します。
ここでも Cloudflare 連携により、必要な MX レコードと SPF レコードがまとめて追加されます。

![gws-gmail-mx-setup](/images/gws-using-cloudflare/gws-gmail-mx-setup.png)
*Google Workspace - Gmail (MX) 設定画面*

Cloudflare 側の確認画面です。

![cloudflare-google-dns-mx-spf](/images/gws-using-cloudflare/cloudflare-google-dns-mx-spf.png)
*Cloudflare - MX/SPFレコード追加の確認画面*

設定が反映されると、Cloudflare の DNS レコード一覧は以下のような状態になります。

![cloudflare-dns-records-complete](/images/gws-using-cloudflare/cloudflare-dns-records-complete.png)
*Cloudflare - DNSレコード (設定完了後)*

最後に Google Workspace 側で「設定完了」になればOKです。

![gws-setting-domain-complete](/images/gws-using-cloudflare/gws-setting-domain-complete.png)
*Google Workspace - ドメイン設定完了画面*

:::message
既に Web サイト等で DNS レコードを運用している場合でも、基本的には「必要なレコードが追加されるだけ」です。
ただし、既存の MX レコードがある場合はメール配送に影響するので、どのメール基盤を使うか (Gmailに寄せるか) を先に決めておくのが安全です。
:::

### 5. 管理コンソールでユーザー/メールを確認

管理コンソールのユーザー一覧で、作成したユーザーが有効になっていれば、カスタムドメインのメールが使える状態になっています。

![gws-admin-user-list](/images/gws-using-cloudflare/gws-admin-user-list.png)
*Google管理コンソール - ユーザー一覧*

## おわりに

この記事では、**Cloudflareでドメインを取得して、そのドメインでGoogle Workspaceを契約する**流れをまとめました。
やってみると意外とシンプルで、

- ドメイン取得 (Cloudflare Registrar)
- Google Workspace申込み (既存ドメインを利用)
- Cloudflare連携で TXT / MX / SPF を自動追加

…という順番で、ほぼ画面の案内に従って進めるだけで完了します。

### コスト面のポイント

個人的にいちばん大きいのは、**ドメイン費用の見通しが立つ**点です。
Cloudflare Registrar は「登録料と更新料のみ」と明言しており、
ドメインに不要なオプションを付けられて結果的に高くなる……というのを避けやすいです。

Google Workspace の申込み画面でドメインも同時購入できるのは確かに楽ですが、
更新時の料金体系 (提供元のサブスクリプション等) によっては、2年目以降のコストが想像以上に膨らむことがあります。

「Google Workspaceの月額」と「ドメインの年額」は別物なので、
**長く使うなら、ドメインは“更新まで含めたトータル”で考える**のがオススメです。

### コスト以外のメリット

Cloudflareを選ぶと、今回触れた DNS 以外にも

- Pages/Workers/R2 など、個人開発で便利な機能
- DNS/SSL/TLS 周りの管理がしやすい

といった恩恵があり、単なる「ドメイン屋さん」以上の価値があります。

### 運用上の注意(最低限ここだけ)

- すでに別のメール基盤を使っているドメインに対して MX を切り替えると、メール配送に影響が出ます (切り替えタイミングは慎重に)
- 記事にスクショを載せる場合、MX/TXT自体は公開情報ですが、管理画面上のメールアドレス等の個人情報が写り込むのでマスク推奨です

Cloudflareを利用することで様々な恩恵 (運用のしやすさ、拡張性) を受けられるので、個人でもGoogle Workspaceを使いたい方は、ぜひこの構成を検討してみてください。

[^1]: 「[NotebookLM × Gemini でGoogle Cloud Professional資格を2ヶ月で制覇する](https://zenn.dev/nttdata_tech/articles/c6261a67c277cc)」NotebookLMに関してはこのような記事を書いているのでご興味あれば読んでみてください。
[^2]: [以前書いたテックブログ](https://zenn.dev/terrako/articles/fb7cb47203ef32)ではCloud Runに直接IAPをアタッチする機能を用いたのですが、この機能は組織が有効化されていないと利用できない機能でした。
[^3]: SquarespaceはGoogle Domainの事業を継承したサービスになっています。
[^4]: **TXTレコード**は、ドメイン (例: `example.com`) に任意の文字列を紐づけるDNSレコードです。Google Workspaceでは主に「ドメイン所有権の証明」で使われ、指定されたトークン文字列をTXTとして追加することで、Googleがそのドメインを操作できることを確認します。
[^5]: **MXレコード**は、そのドメイン宛のメールを「どのメールサーバーで受け取るか」を指定するDNSレコードです。Gmailを使う場合は、Googleが指定する複数のMX (優先度付き) を設定することで、`@your-domain` 宛メールがGmailに届くようになります。
[^6]: **SPF  (Sender Policy Framework)** は、送信元ドメインのなりすましを減らすための仕組みで、「このドメインからメールを送ってよい送信元」を宣言します。多くの環境では **TXTレコードとして `v=spf1 ...` の形式**で設定します (SPFは複数作らず、基本は1本に統合します) 。
