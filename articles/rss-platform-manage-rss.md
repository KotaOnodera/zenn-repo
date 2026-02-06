---
title: "Google Workspaceで実現するRSSプラットフォーム"
emoji: "🦖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - "rss"
  - "googleappscript"
  - "spreadsheet"
  - "googlechat"
  - "googleworkspace"
published: false
publication_name: "nttdata_tech"
---

## はじめに

エンジニアの方もエンジニアではない方も、こんにちは。これまでは金融業界で SRE としてGoogle CloudのプロジェクトにJoinしていましたが、最近プロジェクトが変わり、AWSに挑戦しています。
先日は[Google Cloud Partner Top Engineer 2026](
https://cloud.google.com/blog/ja/topics/partners/announcing-the-winners-of-the-google-cloud-partner-top-engineer-2026-award-program) に選出していただきました！

みなさんは普段、どのようなインプットをしていますか？
私は「自分から情報を収集しなくても、自動で技術情報が目に入る仕組み」としてRSSを使っています。RSSを利用するには主にRSSリーダーを利用することが多いかと思います。
私はRSS取得 / 配信 / 閲覧をGoogle Workspace内のサービスで完結して実装しています。RSS取得 / 配信 / 閲覧する環境のことを本記事では「RSSプラットフォーム」と位置づけ、紹介していこうと思います。
本記事の構成を取ることで、無料でRSSプラットフォームを構築することができ、**快適なインプット環境**を構築することができます。また、Google Workspace内で完結しているので、Google Workspaceのメリットを存分に享受できるのも魅力の1つです。Google Workspaceの魅力については以前執筆した[こちらの記事](https://zenn.dev/nttdata_tech/articles/gws-using-cloudflare)で語っていますので、ぜひご一読してもらえればと思います。

### RSSプラットフォームUXイメージ
最初に本記事で紹介するRSSプラットフォームが "どんなもんか" の想像がしやすいようにイメージを紹介します。
本記事を参考にRSSプラットフォームを構築すると**スマホで**以下のように、配信されたRSSを閲覧することが可能です。画像では「Qiitaから配信される`Google Cloud`タグのRSS」「Google CloudのRelease Noteから配信されるRSS」「Zennから配信される`GAS`タグのRSS」がGoogle Chatに配信され、スマホから閲覧できています。後述しますが、**もちろんPCやタブレットからでも閲覧可能**です。
![google-chat-phone](/images/rss-platform/google-chat-phone.png)

とにかく実装したい人は以下のGitHubにコードを公開しているので、記事はすっ飛ばして実装してみてください。
https://github.com/KotaOnodera/manage-rss

## 対象読者

- RSSを利用してインプットしたい人
- 無料でRSSを取得するツールが欲しい人
- RSSの内容をGoogle Workspace横断で管理したい人

## RSSとは

最初にRSSについて少し触れます。すでにご存知の方は飛ばしていただいて構いません！
RSS (Really Simple Syndication / Rich Site Summary) は、Webサイトの更新情報 (新着記事など) を配信するための仕組みです。
多くの技術ブログやニュースサイトは「RSSフィード」を公開しており、RSSリーダーや自作ツールがそのフィードURLを定期的に取得することで、最新の更新を一覧で追えるようになります。

一般的なRSSフィードはXML形式で、次のような情報が「記事 (item) 」として並びます。

- タイトル (title)
- リンク (link)
- 公開日時 (pubDate / updated)
- 概要 (description / summary)

サイトを個別に巡回しなくても更新を取りこぼしにくく、複数サイトのインプットを1か所に集約できるのが利点です。
なお、厳密には「RSS」とは別仕様のAtomフィードもありますが、実運用ではRSS/Atomをまとめて「RSS」と呼ぶことが多いです。

## Google Workspaceで実装するメリット

主なメリットは以下に記載したものかなと思っています。

- 無料で実装することができる
  - Googleアカウントを持っていれば本記事で紹介するGoogleプロダクトを利用することができ、無料で実装できる
  - 注意点にも挙げますが、Google Apps Scriptは無料枠の中で利用する必要あり
- マルチデバイスに対応してる
  - Google Chatで配信されたRSSを閲覧するため、スマホ / PC / タブレットに対応
- Spreadsheet ベースでRSSを簡単に管理できる
  - SpreadsheetをRSSのすべてを管理するDBとして利用しているため、Spreadsheetを操作することでRSSを管理可能
  - Spreadsheetがマルチデバイス対応しているため、「いついかなるとき」でもRSSを管理可能
- (Google Chat / Spreadsheet 内でGeminiを利用することができる)
  - Google Workspaceを有料で契約している人のみではあるが、各プロダクト内からGeminiを呼び出し可能
  - Google Chat内の単語を検索したり、配信されたRSSの情報をもとに分析したり
- Googleプロダクトと組み合わせることができる
  - (私は実践できていませんが) NotebookLMにSpreadsheetを読み込ませてみたり
  - GAS内でVertexAIを呼び出して、処理の一部でAIを利用したり
  - テックブログを要約する機能をGoogle Chatから呼び出せるようにしてみたり (RSSプラットフォーム第2弾の記事で紹介予定です)

## Googleプロダクト / シーケンス図

RSSを取得しているGoogleプロダクトとデータの流れは以下の通りです。

- Google Apps Script
  - RSS取得を定期実行し、Spreadsheet / Google Chatへ出力
- Spreadsheet
  - RSS配信のサイト / Google Chatの宛先などの管理
- Google Chat
  - 取得したRSS情報をもとに、該当のサイト情報を出力する宛先
  - ユーザーはGoogle Chatを見ることで、情報をインプット可能
  
:::message alert
**注意事項**

- Google Apps Script (GAS)にはクォータ (実行数・実行時間など) の上限があります。取得対象サイト数やトリガー実行頻度を増やしすぎると、途中でエラーになったり実行がスキップされる可能性があります。
  - 例: 時間主導トリガーの実行回数、スクリプト実行時間、`UrlFetchApp` の呼び出し回数 など
  - 公式ドキュメント:  https://developers.google.com/apps-script/guides/services/quotas?hl=ja

:::

![sequence](/images/rss-platform/sequesnce.png)

## Google Apps Script

詳細は以下のGitHubで紹介しています。セットアップ方法も記載していますので、参照しながらセットアップ / 実装してみてください。
スクリプトプロパティを設定して実行すれば動きます。
https://github.com/KotaOnodera/manage-rss

## Spreadsheet

Spreadsheet（例: `rss-manager`）は「どのサイトのRSSを」「どの頻度で」「どのChatスペースに」流すかを管理するためのマスタ／出力先として使います。
ここでは、添付スクショの各シートを簡単に解説します。

### RSS配信マスタ （サイト一覧）

![spreadsheet-rss-master](/images/rss-platform/spreadsheet-rss-master.png)

配信対象のRSSサイトを管理するシートです。主に次の情報を持ちます。

- 対象: 配信する／しないのON/OFF
- 頻度: `毎時` / `日次` など、取得間隔
- サイト名: 表示用の名称
- 通知タグ: Chat投稿時に付けるタグ（例: `#SRE` など）
- 最終更新日時: 直近に取得したタイムスタンプ（重複投稿防止に利用）
- Webhook URL: 投稿先のGoogle Chat Incoming Webhook
- アイコンURL: 投稿に使用するサイトアイコン
- サイトカテゴリ / タグ: 後述の「Chatマスタ」と突合する分類情報

(列名は例です。実装に合わせて増減します。)

### RSS取得結果 （テックブログ一覧）

![spreadsheet-techblog-list](/images/rss-platform/spreadsheet-techblog-list.png)

GASが取得したRSSの結果を蓄積するシートです。
新着記事がこの一覧に追記され、Chat通知の元データになります。

- 記事タイトル
- 記事URL
- カテゴリ
- 取得日時 / 公開日時 など

このシートを見れば「直近どんな記事を拾っているか」「取得が止まっていないか」をSpreadsheet上で確認できます。

### Chatマスタ （スペースとWebhook）

![spreadsheet-chat-master](/images/rss-platform/spreadsheet-chat-master.png)

配信先のGoogle ChatスペースとIncoming Webhook URLを管理するシートです。

- スペース名: 管理用の名称（例: `10_RSS_General`）
- Webhook URL: スペースに紐づくIncoming Webhook
- タグ: このスペースで扱うテーマ（例: `#SRE` / `#Google Cloud` など）

「RSS配信マスタ」のカテゴリ / タグと紐づけることで、サイトごとに投稿先を切り替えられるようになります。

### サイトアイコン一覧

![spreadsheet-site-icon](/images/rss-platform/spreadsheet-site-icon.png)

サイト名とアイコンURLの対応表です。
Chat投稿を見たときに視認性が上がり、「どのサイト由来の投稿か」が直感的に分かるようになります。

## Google Chat

上記のGoogle Apps ScriptとSpreadsheet をセットアップ完了し、実行すると以下のようにRSSの情報をもとにサイト情報が配信されます。
![google-chat-pc](/images/rss-platform/google-chat-pc.png)
*Google Chat PC版*

Google Chatへの配信なのでスマホでもGoogle Chatのアプリを入れれば、配信されたRSSを見ることができます。
![google-chat-phone](/images/rss-platform/google-chat-phone.png)
*Google Chat Phone版*

## おわりに

本記事では、Google Apps Script / Spreadsheet / Google Chat のみで完結する「RSSプラットフォーム」を紹介しました。
RSSリーダーを使わずとも、**Spreadsheetで配信対象や頻度を管理し、Google Chatに自動配信してスマホ/PCで読む**ところまでを、無料 (クォータ内) で構築できます。

運用のコツは「最初は小さく始めて、育てる」ことです。

- まずは重要な数サイトだけを登録して、頻度は `毎時` 程度から始める
- 配信先 (スペース) をタグで分けて、ノイズを減らす
- GASのクォータ (実行数・実行時間など) に当たらない範囲で、対象サイトや頻度を増やす

この構成を土台にすると、NotebookLMやGeminiと組み合わせて「要約して配信する」「タグ付け・分類を自動化する」など、さらにインプット体験を改善していけます。
まずは本記事の構成で“自分にとって見やすいRSS環境”を作ってみてください。
