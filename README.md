# zenn-repo

このリポジトリは、Zenn の記事/本コンテンツを zenn-cli で管理するための作業用リポジトリです。

## セットアップ

依存関係をインストールします（package-lock.json があるので通常は `npm ci` 推奨）。

```sh
npm ci
```

## zenn-cli コマンド一覧

このリポジトリでは、ローカル依存関係の zenn-cli を `npx` 経由で実行する想定です。

### 初期化（初回のみ）

コンテンツ管理用のディレクトリ（例: `articles/`, `books/`）を作成します。

```sh
npx zenn init
```

### プレビュー

コンテンツをローカルサーバーでプレビューします（デフォルト: ポート `8000`）。

```sh
npx zenn preview
```

主なオプション:

- `--port PORT` / `-p PORT`: 起動するポート（例: `3000`）
- `--no-watch`: ホットリロード無効化
- `--open`: 起動時にブラウザを開く
- `--host`: バインドするホスト名を指定

例:

```sh
npx zenn preview --port 3000 --open
```

### 記事を作成

```sh
npx zenn new:article
```

主なオプション:

- `--slug SLUG`: 記事スラッグ（`a-z0-9` と `-` と `_` の 12〜50 文字）
- `--title TITLE`: 記事タイトル
- `--type TYPE`: `tech`（技術記事）/ `idea`（アイデア記事）
- `--emoji EMOJI`: アイキャッチとして使われる「1文字」
- `--published`: 公開設定（`true`/`false`。デフォルト `false`）
- `--publication-name`: Publication に紐付ける場合のみ指定
- `--machine-readable`: 作成成功時にファイル名のみを出力

例:

```sh
npx zenn new:article --slug enjoy-zenn-with-client --title 'タイトル' --type idea --emoji X
```

### 本を作成

```sh
npx zenn new:book
```

主なオプション:

- `--slug SLUG`: 本スラッグ（`a-z0-9` と `-` と `_` の 12〜50 文字）
- `--title TITLE`: 本タイトル
- `--published BOOL`: 公開設定（`true`/`false`。デフォルト `false`）
- `--summary SUMMARY`: 紹介文（有料でも公開される）
- `--price PRICE`: 価格（有料の場合 `200〜5000`。デフォルト `0`）

例:

```sh
npx zenn new:book --slug enjoy-zenn-with-client
```

### 一覧表示

記事の一覧:

```sh
npx zenn list:articles --format tsv
```

本の一覧:

```sh
npx zenn list:books --format json
```

`--format` は `tsv` または `json` をサポートしています。

### バージョン確認 / ヘルプ

```sh
npx zenn --version
npx zenn --help
```

## 参考

- https://zenn.dev/zenn/articles/zenn-cli-guide
