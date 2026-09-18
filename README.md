# latex-docker-vscode

Docker と VSCode の Dev Container を使って、ローカルに \LaTeX 環境を構築することなく \LaTeX 文書をビルドできるようにするためのテンプレートリポジトリです。

[being24/latex-docker](https://github.com/being24/latex-docker) の Docker イメージと [being24/latex-template-ja](https://github.com/being24/latex-template-ja) をベースに、個人用途に合わせて構成を整理しています。イメージ自体の詳細（同梱パッケージなど）は上記リポジトリの README も参照してください。

## 特長

- Dev Container を開くだけで `uplatex` + `dvipdfmx` によるビルド環境が揃う（ローカルに \TeX Live をインストール不要）
- VSCode 拡張 [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop) がコンテナ内に自動インストールされ、保存時プレビューやシンタックスハイライトが使える
- `make` / `latexmk` による CLI ビルドにも対応（Docker 経由・ローカル \TeX Live 経由の両対応）
- [latexdiff](https://ctan.org/pkg/latexdiff) を使った、直前の commit との差分 PDF 生成スクリプトを同梱
- [textlint](https://textlint.github.io/) による日本語の技術文書向け文章チェック設定を同梱

## 動作要件

- Docker（sudo なしで実行できるように設定しておく）
- Visual Studio Code
  - 拡張機能 [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)（`ms-vscode-remote.remote-containers`）
  - LaTeX Workshop 拡張はコンテナ側にインストールされるため、ホスト側に入れる必要はない（入れていても問題ない）
- Git
- （Windows の場合）WSL2 上に Docker と VSCode の連携環境を用意しておくことを推奨

CLI から直接ビルドしたい場合は、上記に加えて `make` コマンドが使えれば十分です（`USE_DOCKER=no` を指定すればローカルの \TeX Live を利用することもできます）。

## Getting Started

1. このリポジトリをテンプレートとして新規リポジトリを作成する、または clone する。
2. VSCode でフォルダを開く。
3. 右下に表示される通知、またはコマンドパレットから `Dev Containers: Reopen in Container` を実行する。
   - `.devcontainer/Dockerfile` に基づいてコンテナイメージがビルドされ、LaTeX Workshop などの拡張機能が自動インストールされる。
   - 初回はイメージの取得・ビルドに時間がかかる。
4. コンテナが起動したら `main.tex` を開き、コマンドパレットから `LaTeX Workshop: Build LaTeX project` を実行するか、エディタ右上のビルドボタンを押す。
   - `main.pdf` が生成され、VSCode 内のタブでプレビューできる。
5. 自分の文書を書く場合は `main.tex` / `setting.tex` の内容を編集する（後述）。

## フォルダ構成

```
.
├── .devcontainer/
│   ├── Dockerfile             # コンテナイメージ定義（being24/latex-docker をベースに拡張）
│   ├── devcontainer.json      # Dockerfile をビルドして使う設定（既定）
│   └── org_devcontainer.json  # ビルドせず公式イメージをそのまま使う場合の参考設定
├── .vscode/
│   ├── settings.json          # LaTeX Workshop のビルドレシピなどエディタ設定
│   └── latex.code-snippets    # 図・表挿入用のユーザースニペット
├── figure/                    # 画像ファイルの格納先
├── main.tex                   # 文書本体（サンプル文書）
├── setting.tex                # プリアンブル・自作コマンドなど共通設定
├── .latexmkrc                 # latexmk の設定（uplatex + dvipdfmx を使用）
├── Makefile                   # CLI から Docker 経由 / ローカルでビルドするためのタスク集
├── mkdiff.sh                  # 直前の commit との差分ファイル（diff.tex）を生成するスクリプト
├── package.json / .textlintrc # textlint による日本語文章チェックの設定
└── LICENSE
```

### `.devcontainer`

- `devcontainer.json` が既定の設定で、`Dockerfile` をビルドしてコンテナイメージを作成する。
- 独自の `ENV` を追加したい場合などは `Dockerfile` に追記する。
- 単にオリジナルの Docker イメージ（`ghcr.io/being24/latex-docker`）をそのまま使いたい場合は、`org_devcontainer.json` の内容を `devcontainer.json` にコピーすればビルド手順を省略できる。

### `.vscode`

- `settings.json`
  - LaTeX Workshop のビルド設定。通常ビルド用の `compile` レシピと、差分 PDF 生成用の `compile-diff` レシピを定義している。
  - `latex-workshop.latex.autoClean.run` により、ビルド後に中間ファイル（`.aux` など）を自動削除する設定になっている。
- `latex.code-snippets`
  - `fig1` / `fig2` / `table` と入力すると、図（1 枚 / 2 枚並び）や表のテンプレートを挿入できる。

### `main.tex` / `setting.tex`

- `main.tex` はサンプル文書であり、本テンプレートで使える記法（数式・図・表・ソースコード・参考文献・差分作成など）のリファレンスを兼ねている。まずはビルドして見た目を確認し、自分の文書に合わせて書き換えるとよい。
- `setting.tex` にはプリアンブル（`\usepackage` 等）と自作コマンド（`\figref` / `\tabref` / `\eref` / `\pdiff` など）をまとめている。全プロジェクト共通の設定はここに書く。
- `\TeX Live` に同梱されていないパッケージを使う場合は `.devcontainer/Dockerfile` に `tlmgr install <package>` を追記する。

## ビルド方法

### VSCode（推奨）

コマンドパレットから `LaTeX Workshop: Build LaTeX project` を実行し、レシピ `compile` を選択する。保存時に自動ビルドはしない設定（`latex-workshop.latex.autoBuild.run: never`）なので、手動でビルドする。

### Makefile（CLI）

Dev Container の中でも外でも、以下のコマンドが利用できる。

```sh
make            # main.pdf をビルド（Docker 経由。既定）
make watch      # ソースの変更を監視して自動ビルド
make clean      # 中間ファイル・PDF を削除
make lint       # textlint で文章をチェック
make fix        # textlint --fix で自動修正可能な指摘を修正
```

Docker を使わずローカルの `latexmk` を使いたい場合は `USE_DOCKER=no make` のように指定する。

## 差分 PDF の作成（latexdiff）

直前の commit と現在の `main.tex` の差分を可視化した PDF を作成できる。

- VSCode: LaTeX Workshop のコマンド `Build LaTeX project` からレシピ `compile-diff` を実行する。内部で `mkdiff.sh` → `latexdiff` → `latexmk` の順に実行され、`diff.pdf` が生成される。
- CLI から直接実行する場合は `bash mkdiff.sh` で `diff.tex` を生成した後、`latexmk diff.tex`（または `make` の仕組みを応用）でビルドする。

`diff.tex` や作業用の `archive/` ディレクトリは `.gitignore` で除外されている。

## 文章チェック（textlint）

日本語の技術文書向けルール（`textlint-rule-preset-ja-technical-writing` など）を設定済み。

```sh
npm install
npm run lint   # 指摘一覧を表示
npm run fix    # 自動修正可能な指摘を修正
```

## ライセンス

[MIT License](./LICENSE)（`being24` および `a11-green` によるコードを含む）。
