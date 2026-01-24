---
title: "Cloudflareでドメインを取得して半額のコストでGoogle Workspaceを契約しよう"
emoji: "⛅️"
type: "idea" # tech: 技術記事 / idea: アイデア
topics:
  - "googleworkspace"
  - "cloudflare"
published: false
---

## はじめに

エンジニアの方もエンジニアではない方も、こんにちは。現在は金融業界で SRE としてGoogle CloudのプロジェクトにJoinしていましたが、最近プロジェクトが変わり、AWSに挑戦しています。
先日は[Google Cloud Partner Top Engineer 2026](
https://cloud.google.com/blog/ja/topics/partners/announcing-the-winners-of-the-google-cloud-partner-top-engineer-2026-award-program) に選出していただきました！

みなさんは、Google Workspace (以下GWS) を契約していますか？GWSを契約し、各種機能を利用するにあたり、独自ドメインを所得・登録する必要があります。独自ドメインを取得する方法はたくさんありますが、私は (金額や運用も含めて) 最もコストがかからないのは**Cloudflareでドメインを取得する方法**だと思っています。
私はCloudflareで `.org`ドメインを年間$10で契約してGWSは月額1,900円で契約しています。**なぜ、Cloudflareでドメインを取得することになったのか、ドメイン取得からGWSの契約の手順を簡単に解説しようと思います**。

### なぜGoogle Workspaceを契約すべきなのか
まず、そもそもなぜGWSを個人で契約しようとしたのか、GWSを契約すると何が良いのかも簡単に言及しておきます。私はエンジニアなので、エンジニア視点でのGWSの良い面について言及しますが、非エンジニアの方でもオススメできるものです！
(GWSの良さを知っている人は読み飛ばしてください！)

元々、[Google AI Pro](https://one.google.com/intl/ja_jp/about/google-ai-plans/)を契約していました。こちらの契約は月額2,900円です。契約単体で見ると、1,000円近くコストカットできます。また、契約した当時にGWSのアルファ機能としてGoogle Workspace Flows (現在は[Google Workspace Studioというサービス名に変更されています](https://dev.classmethod.jp/articles/trying-google-workspace-flows-alpha/))というサービスがリリースされ、使ってみたいと思いGWSに移行しようと思い立ちました。
Google AI Proを契約していたので、Google系のサービスは以前から課金して利用していました。Gemini / NotebookLM / Google Drive etc...などなどを利用しており、特にNotebookLMにはお世話になりました。(「[NotebookLM × Gemini でGoogle Cloud Professional資格を2ヶ月で制覇する](https://zenn.dev/nttdata_tech/articles/c6261a67c277cc)」NotebookLMに関してはこのような記事を書いているのでご興味あれば読んでみてください。)Google AI ProからGWSへ移行しましたが、使用感としてはあまり変わらない印象です。

GWSの良さを挙げると以下かなと個人的には思っています。

- ビジネスで必須のサービスからAI系のサービスまでオールインワンで利用できる
- 特にGeminiとの親和性が高く、各種サービスからGeminiを利用でき、GWS横断でGeminiを呼び出せる
- GWSのアルファ機能を利用できる (上述したような枠での機能利用)

> GWSの良さ
> 特にGeminiとの親和性、NotebookLMという強力なツール、GWSでビジネスもできるし、開発もできるというオールインワンさ、が良いと思う。
> 特にGeminiをWorkspace内で横断で使えるのはとてもよい。
> これはエンジニアだけでなく、非エンジニアにもとても良いツールだと思う。
> 補足的ではあるけど、最近あったGWS関連の法令遵守の話もしてもいいかも。断定的な表現は避けること。
> 自分としてはアルファ機能を使いたかった。Flowsを先んじて使ってみたかった。後、コストも安くできそうだった。
> RSSの配信もできるし、Slackの個人契約をやめて、オールインワンにしたかった。最近だとGoogle ChatもGeminiの検索対象となったので、使い勝手が良い。https://x.com/hashimoto_no14/status/2014129789926347032?s=46

### なぜCloudflareでドメインを取得すべきなのか
>
> - ドメインが安い (原価で売っている)
> - DNSレコード管理が楽
> - 無料枠でその他の機能も一緒に使える

## 想定読者

- 個人でGoogle Workspaceを利用したい人
  - 非エンジニアの人でビジネスを始める人
  - エンジニアの人でGeminiにどっぷり浸かりたい人
  - NotebookLMの効力を高めたい人
- Cloudflareの機能を利用してみたい人

## 概要

## Cloudflareでドメインを取得
> たくさん記事があるので、ここは割愛する。

## Google WorkspaceをCloudflareのドメインで契約する方法

## おわりに
> Cloudflareを利用することで「コストが半額以下になる」以上の恩恵を受けることが可能
