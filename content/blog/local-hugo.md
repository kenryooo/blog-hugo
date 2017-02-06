+++
date = "2017-01-05T12:00:00+09:00"
draft = false
title = "ローカル環境で Hugo を動かす"

+++

### Hugo とは
Go 製の静的サイトジェネレータ。  
静的ページの生成が高速で、シンプルなところが好み。

### インストール  
mac なら 

```bash
$ brew install hugo
```

Go 環境が設定済みなら下記でもいい

```bash
$ go get github.com/spf13/hugo
``` 

### プロジェクト作成  

```bash
$ hugo new site <your_site>
$ cd <your_site> # ここがプロジェクトのルートディレクトリになる
```

### テーマの適用
`<your_site>` 配下の `themes` というディレクトリ配下に配置する。


#### すべてのテーマをまとめてインストールする場合

```bash
$ git clone --depth 1 --recursive https://github.com/spf13/hugoThemes.git themes
```

#### テーマを個別に選んでからインストールする場合

[こちら](http://themes.gohugo.io/)からテーマを選んで、個別のリポジトリから `git clone` する。ここでは [cocoa](https://github.com/nishanths/cocoa-hugo-theme) を例に進める。

```bash
$ git clone https://github.com/nishanths/cocoa-hugo-theme.git themes/cocoa
$ hugo -vw -t cocoa serve  # サーバを起動 http://localhost:1313/)

```

参考  

* [Installation](https://github.com/spf13/hugoThemes#installation)

#### Theme のカスタマイズ
カスタマイズするには `<your_site>/themes/<theme>/` 配下を直接編集せずに、<your_site> 直下にディレクトリ・ファイルを追加する。追加するファイルは `<your_site>/themes/<theme>/exampleSite` からコピーする。

まず `exampleSite` の `config.toml` をコピーして、カスタマイズしていくと楽ちん。あとは、[Configuring Hugo](http://gohugo.io/overview/configuration/)のページを見ながらカスタマイズしていくとよい。


ディレクトリ構造はこんな感じ

```bash
$ pwd
<your_work_directory>/<your_site>/themes
$ tree
.
└── cocoa
    ├── LICENSE
    ├── README.md
    ├── archetypes
    │   └── default.md
    ├── exampleSite    # ここの配下から必要なものをコピーする
    │   ├── LICENSE
    │   ├── README.md
    │   ├── config.toml
    │   ├── content
    │   │   ├── about-this-site.html
    │   │   ├── blog
    │   │   │   ├── lesmiserables-hugo.md
    │   │   │   ├── metamorphosis-kafka.md
    │   │   │   └── use-at-instead-of-head.md
    │   │   ├── fixed
    │   │   │   ├── about.md
    │   │   │   ├── code.md
    │   │   │   └── colophon.md
    │   │   ├── posts.html
    │   │   └── writing.html
    │   └── static
    │       ├── css
    │       │   └── override.css
    │       └── img
    │           └── leaf.ico
    ├── images
    │   ├── screenshot.png
    │   └── tn.png
    ├── layouts
    │   ├── 404.html
    │   ├── _default
    │   │   ├── list.html
    │   │   └── single.html
    │   ├── blog
    │   │   └── single.html
    │   ├── fixed
    │   │   └── single.html
    │   ├── index.html
    │   └── partials
    │       ├── footer.html
    │       ├── footer_scripts.html
    │       ├── head.html
    │       ├── head_includes.html
    │       ├── header.html
    │       ├── li.html
    │       ├── meta.html
    │       └── page-heading.html
    ├── static
    │   ├── css
    │   │   ├── main.css
    │   │   ├── pygments.css
    │   │   └── reset.css
    │   └── img
    │       └── favicon.ico
    └── theme.toml
```

