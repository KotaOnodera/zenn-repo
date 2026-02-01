---
title: "Google Workspaceで実現するRSSプラットフォーム"
emoji: "📡"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - "rss"
  - "googleappscript"
  - “spreadsheet”
  - “googlechat”
  - "googleworkspace"
published: false
---

## はじめに

エンジニアの方もエンジニアではない方も、こんにちは。現在は金融業界で SRE としてGoogle CloudのプロジェクトにJoinしていましたが、最近プロジェクトが変わり、AWSに挑戦しています。
先日は[Google Cloud Partner Top Engineer 2026](
https://cloud.google.com/blog/ja/topics/partners/announcing-the-winners-of-the-google-cloud-partner-top-engineer-2026-award-program) に選出していただきました！

みなさんは普段、どのようなインプットをしていますか？私は「自分から情報を収集しなくても、自動で技術情報が目に入る仕組み」としてRSSを使っています。
RSSを利用するには主にRSSリーダーを利用することが多いかと思います。

## 対象読者
- RSSを利用してインプットしたい人
- 無料でRSSを取得するツールが欲しい人
- RSSの内容をGoogle Workspace横断で管理したい人

## RSSとは

## Google Workspaceで実装するメリット

主なメリットは以下に記載したものかなと思っています。
- 無料で実装することができる
- マルチデバイスに対応してる
- Spreadsheet ベースでRSSを簡単に管理できる
- Google Chat / Spreadsheet 内でGeminiを利用することができる
- (Googleプロダクトと組み合わせることができる)

## Googleプロダクト / シーケンス図

RSSを取得しているGoogleプロダクトとデータの流れは以下の通りです。
- Google App Script
  - RSS取得を定期実行し、Spreadsheet / Google Chatへ出力
- Spreadsheet
  - RSS配信のサイト / Google Chatの宛先などの管理
- Google Chat
  - 取得したRSS情報をもとに、該当のサイト情報を出力する宛先
  - ユーザーはGoogle Chatを見ることで、情報をインプット可能
  
![sequence](/images/rss-platform/sequesnce.png)

## Google App Script

詳細は以下のGitHubを見て下さい。セットアップ方法も記載しています。

## Spreadsheet

## Google Chat

上記のGoogle App ScriptとSpreadsheet をセットアップ完了し、実行すると以下のようにRSSの情報をもとにサイト情報が配信されます。

## おわりに
