# latest

*「さっきダウンロードした画像ファイル3つをコピーしたいんだけど、拡張子は何だったっけ？」*

コマンドラインからファイルの更新日時（最終変更時刻）で簡単にファイルを選択できるツールです。
ファイル種別（kind）によるフィルタリングや、「最新／最古N件」の選択、globパターンによるファイル指定が可能です。
ドキュメント・スプレッドシート・プレゼン資料・アーカイブファイルなどのMIMEタイプを元にしたフィルタもサポートします。

## 特長

* 指定したパターンに一致するファイルの中から、**最新のN個** (または、古い方から N 個)を選択できます。
* MIMEタイプに基づき、`doc`、`xls`、`ppt`、`zip` などの種別でファイルを絞り込めます。
* 複数のファイルやglobパターンをサポート。
* ログを出さない「クワイエットモード」や、ファイルが見つからない場合にもエラーとしないオプションあり。
* 選ばれたファイルの絶対パスを標準出力に出力します。

---

## インストール

`pipx` を使って最新版をインストールできます：

```sh
pipx install http://github.com/tos-kamiya/latest
```

**依存ライブラリについて**

本ツールはファイル種別の判定に `python-magic` を利用しています。
`python-magic` はシステムの `libmagic` ライブラリに依存しているため、OSによっては別途 `libmagic` のインストールが必要になる場合があります。
詳しくは [python-magicの公式ページ](https://github.com/ahupp/python-magic) をご参照ください。

## 使い方

```sh
latest [オプション] ファイル...
```

### 使用例

* Downloads フォルダ内で最も新しい `.pdf` ファイルを表示：

  ```sh
  latest --newest 1 ~/Downloads/*.pdf
  ```

* `~/Documents` 内で最も古い `.docx` ファイルを3件表示：

  ```sh
  latest --oldest 3 ~/Documents/*.docx
  ```

* "xls" 種別（表計算ファイル。`.xlsx`、`.xls`、`.ods` を含む）で最新のファイルを表示：

  ```sh
  latest -k xls --newest 1 ~/Downloads/*
  ```

* ログ出力なしで、`ppt` 種別の最新ファイルを、ファイルが見つからなくてもエラーにせず表示：

  ```sh
  latest -q -0 -k ppt --newest 1 ~/Downloads/*
  ```

**Picturesフォルダ内の最新の画像ファイルをカレントディレクトリにコピーする例**

* **Fishシェルの場合:**

  ```fish
  cp (latest -k image --newest 1 ~/Pictures/*) .
  ```

* **Bashの場合:**

  ```bash
  cp $(latest -k image --newest 1 ~/Pictures/*) .
  ```

> `-k image` オプションは、MIMEタイプが画像（`.jpg`, `.jpeg`, `.png`, `.gif` など）と判定されるファイルすべてを対象とします。
> Bashでは `$(...)`、Fishでは `(...)` でコマンド置換を行います。

## オプション

* `-n, --newest N`
  **新しい順**で上位N件のファイルを選択します（デフォルトは1）。

* `-o, --oldest N`
  **古い順**で上位N件のファイルを選択します。

  ※ `--newest` と `--oldest` は同時に指定できません。

* `-k, --kind KIND`
  ファイル種別でフィルタします。
  `doc`, `xls`, `ppt`, `zip`, `image`, `video`, `audio`, `text` などが指定できます。

* `-q, --quiet`
  クワイエットモード。ログ出力をすべて抑制します（標準エラー出力に出力しません）。

* `-0, --allow-empty-result`
  ファイルが見つからなくてもエラーとせず正常終了します（通常はエラー終了）。

* `--version`
  バージョン情報を表示して終了します。

### 対応しているファイル種別

| 種別  | 説明                   | 含まれる拡張子例                        |
| --- | -------------------- | ------------------------------- |
| doc | MS Word, ODF テキスト    | `.doc`, `.docx`, `.odt`         |
| xls | Excel, ODF スプレッドシート  | `.xls`, `.xlsx`, `.ods`         |
| ppt | PowerPoint, ODF スライド | `.ppt`, `.pptx`, `.odp`         |
| zip | Zip/Tar/7z/アーカイブ     | `.zip`, `.tar`, `.7z`, `.rar` 等 |

上記以外のファイル種別（`image`, `audio`, `video`, `text` など）では、
MIMEタイプの「/」の前の部分（例: `image/`, `audio/` など）が一致するファイルがすべて選ばれます。

## 更新履歴

v0.2.0 `--number/-n` オプションは `--newest/-n` に名称変更。`--oldest/-n`を新規追加。

## ライセンス

MIT License
