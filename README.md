# 片づけ処 つむぎ LP ― 確認用サイト

クライアントに確認していただくための、**パスワード付き公開ページ**です。

- 公開URL：<https://rockdanceai2025-sys.github.io/tsumugi-lp/>
- パスワード：`tsumugi2026`
- LPの元データ（制作用）：`rockdanceai2025-sys/city-contact-enkin` の
  ブランチ `claude/estate-buyback-lp-7sxrnj` ／ フォルダ `ihin-lp/`

## 仕組み

このリポジトリは公開設定ですが、**LPの中身は暗号化した状態で置いてあります**（`site.bin`）。

1. クライアントがURLを開くと、パスワード入力画面が出る
2. 正しいパスワードを入れたときだけ、ブラウザの中で復号されてLPが表示される
3. パスワードを知らない人は、URLを開いても入力画面しか見えない

`site.bin` を直接ダウンロードしても、パスワードなしでは中身を取り出せません
（AES-256-GCM、鍵はパスワードから PBKDF2-SHA256／25万回で生成）。

検索対策として `noindex` と `robots.txt` も入れてあるため、Google等の検索結果には出ません。

## ファイル構成

```
.
├── index.html   … パスワード入力ページ（復号して表示する処理も含む）
├── site.bin     … 暗号化したLP一式
├── robots.txt   … 検索エンジンよけ
└── .nojekyll    … GitHub Pages がファイルをそのまま配信するための設定
```

## 内容を更新するとき

LPの修正は元データ側（`city-contact-enkin` の `ihin-lp/`）で行い、
このリポジトリには暗号化したものを置き直します。

```bash
# 元データ側で
python3 tools/build-preview.py                    # preview.html を作り直す
node tools/build-encrypted.js tsumugi2026         # site.bin を作り直す
# できた site.bin をこのリポジトリに置いて push
```

Claude に依頼すれば作り直します。

## パスワードを変えるとき

Claude に「確認用ページのパスワードを〇〇に変えて」と伝えてください。
`site.bin` を作り直して差し替えます（クライアントには新しいパスワードの再連絡が必要です）。

## 確認が終わったら

このリポジトリは公開設定のため、確認が終わったら
GitHubの **Settings → General → Danger Zone → Delete this repository** で
削除するか、**Settings → Pages** で公開を止めてください。
