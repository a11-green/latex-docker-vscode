# About This Repository

DockerとVSCodeでLaTeX環境を爆速で構築するためのテンプレート。
[nukopy さんのリポジトリ](https://github.com/nukopy/latex-in-docker-on-vscode)をもとに作成。

VSCodeのdevcontainerを用いて、ローカルにLaTeX環境を構築することなく、docker上の環境でLaTeX文書をビルドできるようにする。

以下の Docker イメージをベースとしている。

- GitHub: [nukopy/ubuntu-texlive-ja](https://github.com/nukopy/ubuntu-texlive-ja)
- Docker Hub: [nukopy/ubuntu-texlive-ja](https://hub.docker.com/repository/docker/nukopy/ubuntu-texlive-ja)

TeXLiveのバージョンは2022。  
ビルドにはlatexmkを用いている。
LaTeX Workshopの拡張もdevcontainerでインストールされるようにしている。

なお、ライセンスはMITライセンスに準拠します。

# Getting Started

## 環境
- WSL（WSL 2）  
  ここではWindows11にWSL2（Ubuntu）をインストールしていることを前提とする。
- Visual Studio Code (VSCode)  
  Windowsに普通にインストールしておく。  
  また以下のExtensionsをインストールしておく。
  - Remote - Containers  
  なお、LaTeX Workshopはdevcontainerでインストールするため、入れていなくてもいい（入れていてもいい）。
- Git   
  いれてなくても使えなくないが、入れておいたほうが楽。
- Docker、Docker Compose  
  WSL2上にインストールし、sudoなしでdockerが実行できるようにしておく。


## 手順

環境構築手順は以下の通り。

1. 本リポジトリの `Use this template` ボタンをクリックし、新しくリポジトリを作成する
2. 作成したリポジトリを clone し、VSCode で開く

   ```sh
   git clone [Your Repository URL]
   cd [Your Repository Name]
   ```

3. cmd + shift + P（`Show All Commands` ショートカット）で **"Remote-Containers: Reopen in Container"** を検索し実行すると、Docker コンテナのビルドが実行され、リモート環境を開いた状態でエディタが起動する
   - 別途パッケージが必要な場合、`.devcontainer/Dockerfile` を編集してからビルドする
4. PDF プレビューを開く：`src/main.tex` を開いた状態にし、cmd + shift + P で **"LaTeX Workshop: View LaTeX PDF file"** を検索し実行すると、画面右側に LaTeX 文書から生成された PDF が表示される（エディタ右上の虫眼鏡ボタンを押してもプレビューを表示できる）。

# フォルダ構成
## .devcontainer
コンテナに関する情報は`.devcontainer`フォルダの中に記載する。
- devcontainer.json  
- Dockerfile
- docker-dompose.yml

## .vscode
VSCodeのユーザ設定およびユーザスニペットは`.vscode`フォルダの中のファイルに記載する。
- setting.json  
  エディタ及び拡張機能に関する設定。
  主にLaTeX Workshopのビルド設定などを記載している。
- latex.code-snippets  
  ユーザスニペット。

## src
LaTeXのソースコード。
プリアンブルは`setting.tex`に分けて記載している。
`main.tex`は