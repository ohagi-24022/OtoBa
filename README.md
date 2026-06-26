# OtoBa

OtoBa は、Web 画面と LINE Bot から YouTube の曲をリクエストできる音楽チャットプレイヤーです。
PC やスマートフォンのブラウザで再生画面を開き、チャット入力や LINE メッセージから曲の検索、予約、コメント表示、デフォルト BGM の変更を行えます。

## 主な機能

- YouTube 動画の再生
- YouTube URL を送信して曲を予約
- YouTube キーワード検索から曲を選択して予約
- YouTube プレイリスト URL の一括予約
- リクエスト曲のキュー再生
- リクエストがない時のデフォルト BGM 再生
- デフォルト BGM の動画またはプレイリスト変更
- `skip` / `next` / `back` コマンドによる再生操作
- `#` から始まるコメントの画面上オーバーレイ表示
- LINE Bot Webhook 連携
- Socket.IO による Web 画面へのリアルタイム反映

## 必要なもの

- Node.js 14 以上
- npm
- YouTube Data API v3 の API キー
- LINE Messaging API のチャネルアクセストークンとチャネルシークレット

## セットアップ

依存パッケージをインストールします。

```bash
npm install
```

環境変数を設定します。

```bash
CHANNEL_ACCESS_TOKEN=LINEのチャネルアクセストークン
CHANNEL_SECRET=LINEのチャネルシークレット
YOUTUBE_API_KEY=YouTube Data API v3のAPIキー
PORT=3000
```

`PORT` は省略できます。省略した場合は `3000` 番ポートで起動します。

PowerShell で一時的に設定する例:

```powershell
$env:CHANNEL_ACCESS_TOKEN="LINEのチャネルアクセストークン"
$env:CHANNEL_SECRET="LINEのチャネルシークレット"
$env:YOUTUBE_API_KEY="YouTube Data API v3のAPIキー"
$env:PORT="3000"
```

## コマンド

アプリを起動します。

```bash
npm start
```

実際には次のコマンドが実行されます。

```bash
node server.js
```

起動後、ブラウザで次の URL を開きます。

```text
http://localhost:3000
```

## LINE Webhook

LINE Developers の Messaging API 設定で、Webhook URL に次のエンドポイントを指定します。

```text
https://公開URL/callback
```

ローカル環境で LINE Webhook を試す場合は、ngrok などでローカルサーバーを公開し、その公開 URL に `/callback` を付けて設定してください。

例:

```text
https://example.ngrok-free.app/callback
```

## 使い方

### 曲を予約する

Web 画面の入力欄、または LINE メッセージに YouTube URL を送信します。

```text
https://www.youtube.com/watch?v=xxxxxxxxxxx
```

YouTube の検索キーワードを送信すると、検索結果から曲を選べます。

```text
好きな曲名
```

プレイリスト URL を送信すると、プレイリスト内の曲をまとめて予約します。

```text
https://www.youtube.com/playlist?list=PLAYLIST_ID
```

### デフォルト BGM を変更する

`default` に続けて YouTube URL、プレイリスト URL、または検索キーワードを送信します。

```text
default https://www.youtube.com/watch?v=xxxxxxxxxxx
```

```text
default https://www.youtube.com/playlist?list=PLAYLIST_ID
```

```text
default 好きなBGM
```

検索キーワードを指定した場合は、検索結果からデフォルト BGM にしたい曲を選択します。

### コメントを流す

`#` から始まるメッセージを送ると、再生画面上にコメントが表示されます。

```text
# こんにちは
```

色、サイズ、表示位置の指定もできます。

```text
# red big こんにちは
# blue small コメント
# ue 上に表示
# shita 下に表示
```

利用できる指定:

- 色: `red`, `blue`, `green`, `yellow`, `pink`
- サイズ: `big`, `small`
- 位置: `ue`, `shita`

### 再生操作

Web 画面、または LINE から次のコマンドを送信できます。

```text
skip
next
back
```

- `skip`: リクエスト曲の再生中に次の曲へ進みます
- `next`: リクエスト曲の再生中は次の曲へ進み、デフォルトプレイリスト再生中はプレイリストの次の曲へ進みます
- `back`: デフォルトプレイリスト再生中に前の曲へ戻ります

## ファイル構成

```text
.
├── package.json
├── server.js
├── README.md
└── public/
    └── index.html
```

- `server.js`: Express、Socket.IO、LINE Bot、YouTube API 連携を行うサーバー
- `public/index.html`: YouTube プレイヤー、チャット UI、コメント表示を含む Web 画面
- `package.json`: npm スクリプトと依存パッケージ定義

## 注意事項

- `YOUTUBE_API_KEY` が未設定の場合、キーワード検索やプレイリスト取得は利用できません。
- LINE Bot を利用するには `CHANNEL_ACCESS_TOKEN` と `CHANNEL_SECRET` が必要です。
- YouTube IFrame API を利用しているため、ブラウザ上で再生開始ボタンを押してから再生します。
- サーバーを公開する場合は、環境変数や API キーを公開リポジトリに直接書き込まないでください。
