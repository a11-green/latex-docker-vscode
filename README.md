# About This Repository

DockerとVSCodeでLaTeX環境を爆速で構築するためのテンプレートです。
VSCodeのdevcontainerを用いて、ローカルにLaTeX環境を構築することなく、docker上の環境でLaTeX文書をビルドできるようにしてます。
being24さんの[Dockerイメージ](https://github.com/being24/latex-docker/tree/master)と[テンプレート](https://github.com/being24/latex-template-ja)をもとに自分に不要なところを消したりして作成しています。
ので、詳細は上記のリポジトリのREADMEなどを参照してください。
Dockerイメージはそのまま使ってもよかったが、環境変数を埋め込めるようにDockerfileで上記イメージをベースにDockerfileでBuildするようにしています。

LaTeX Workshopの拡張もdevcontainerでインストールされるようにしています。


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
- Docker、~~Docker Compose~~
  WSL2上にインストールし、sudoなしでdockerが実行できるようにしておく。


## 手順

環境構築手順は以下の通り。



# フォルダ構成
## .devcontainer
コンテナに関する情報は`.devcontainer`フォルダの中に記載する。
- devcontainer.json  
- Dockerfile

## .vscode
VSCodeのユーザ設定およびユーザスニペットは`.vscode`フォルダの中のファイルに記載する。
- setting.json  
  エディタ及び拡張機能に関する設定。
  主にLaTeX Workshopのビルド設定などを記載している。
- latex.code-snippets  
  ユーザスニペット。

## 
- main.tex  
メインのソースコード。
- setting.tex  
プリアンブルのソースコード。