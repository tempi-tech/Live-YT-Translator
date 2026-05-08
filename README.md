# Live YT Translator 日本語ガイド

このリポジトリは、[petergpt/Live-YT-Translator](https://github.com/petergpt/Live-YT-Translator) のforkです。

このツール自体は petergpt さんがMITライセンスで公開しているものです。AGIラボ側では、GitHubに慣れていない人向けに日本語README付きのforkも用意しました。

英語版READMEは [README.en.md](README.en.md) に残しています。

元リポジトリの著作権表示とライセンスはそのまま保持しています。利用・改変・再配布を行う場合は、必ず [LICENSE](LICENSE) を確認してください。

## これは何？

Live YT Translatorは、YouTube動画をリアルタイム翻訳するChrome拡張です。

YouTube動画を開き、拡張機能から翻訳したい言語を選んでStartすると、翻訳音声と字幕オーバーレイが表示されます。

AGIラボでは、このリポジトリを「AI時代のGitHubの読み方」を学ぶ題材としても扱います。README、Chrome拡張の構成、WebRTCの流れ、安全設計、検証コマンドまでまとまっているため、Git/GitHub入門の教材としても見やすい実例です。

## できること

- YouTube動画の音声をリアルタイム翻訳する
- 翻訳字幕を動画上にオーバーレイ表示する
- 翻訳先の言語を選ぶ
- 翻訳音声のvoiceを選ぶ
- 元動画音声と翻訳音声の音量バランスを調整する
- APIキーをChrome拡張側に入れる通常モードと、ローカルブリッジを使うAdvanced modeを選べる

## 必要なもの

- Google Chrome
- OpenAI API key
- YouTube動画
- ローカルブリッジを使う場合はNode.js

## Chrome拡張として使う

1. このリポジトリをclone、またはZIPでダウンロードします。
2. Chromeで `chrome://extensions` を開きます。
3. 右上のDeveloper modeをONにします。
4. Load unpackedをクリックします。
5. このリポジトリ内の `chrome-live-translator` フォルダを選びます。
6. YouTube動画を開きます。
7. Chrome拡張のアイコンからSottoを開きます。
8. 翻訳したい言語とvoiceを選びます。
9. OpenAI API keyを入力してStartします。

## ローカルブリッジを使う

Chrome拡張にOpenAI API keyを保存したくない場合は、ローカルブリッジを使えます。

`.env.example` を参考に `.env.local` を作成し、`OPENAI_API_KEY` を設定します。

```sh
npm install
npm start
```

ブリッジ用のtokenを表示します。

```sh
npm run bridge:token
```

Chrome拡張のポップアップでAdvanced bridgeを開きます。

1. Use local bridgeをONにする
2. tokenを入力する
3. Startを押す

この方法では、OpenAI API keyはローカルのNode bridge側に置かれ、Chrome拡張にはbridge tokenを入力します。

## 検証コマンド

APIキーなしで、言語ルーティングを確認します。

```sh
npm run check:language-routing
```

OpenAI API keyを使って、Realtime翻訳用のclient secretを取得できるか確認します。

```sh
npm run check:translation-model
```

ローカルブリッジの設定を確認します。

```sh
npm run smoke:bridge
```

まとめて確認する場合は次を使います。

```sh
npm test
```

## セキュリティ面で見るポイント

このリポジトリは、コードだけでなく安全設計も読む価値があります。

- [docs/webrtc-flow.md](docs/webrtc-flow.md): WebRTCとOpenAI Realtimeの流れ
- [docs/open-source-security.md](docs/open-source-security.md): APIキー、短命client secret、ローカルブリッジ、ログ方針
- [chrome-live-translator/manifest.json](chrome-live-translator/manifest.json): Chrome拡張の権限

特にAPIキーまわりは、次の点を確認してから使うのがおすすめです。

- 本番用ではなく専用のOpenAI project keyを使う
- 利用量をOpenAI dashboardで確認する
- `.env.local` や実APIキーをGitHubへpushしない
- Chrome拡張にAPIキーを保存したくない場合はローカルブリッジを使う

## Git/GitHub学習の題材として見るなら

GitHubに慣れていない人は、まず次の順で見ると理解しやすいです。

1. `README.md` と `README.ja.md` で概要を読む
2. `chrome-live-translator/` でChrome拡張の中身を見る
3. `docs/webrtc-flow.md` で音声処理の流れを見る
4. `docs/open-source-security.md` で安全設計を見る
5. `package.json` で実行コマンドを見る
6. 検証コマンドを1つずつ実行する

GitHubは、完成品だけを見る場所ではありません。README、構成、検証コマンド、安全設計まで見ると、AIツールを「使う」だけでなく「学ぶ」材料にできます。

## 注意

このソフトウェアは実験的なものです。Realtime翻訳の品質、対応言語、voice、遅延はOpenAI Realtime APIの仕様や利用環境に依存します。

また、YouTube動画や音声の扱い、APIキーの管理、翻訳結果の利用範囲には注意してください。

## License

MIT License

Original project: [petergpt/Live-YT-Translator](https://github.com/petergpt/Live-YT-Translator)
