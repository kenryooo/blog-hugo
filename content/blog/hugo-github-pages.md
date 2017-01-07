+++
date = "2017-01-07T14:09:59+09:00"
title = "Hugo で作ったページを Github Pages に公開する"

+++

[前回](blog/ローカル環境で-hugo-を動かす/) Hugo で作成した静的ファイルを、 Github Pages に公開する。公式の [Hosting on GitHub Pages](http://gohugo.io/tutorials/github-pages-blog/) を見ればできるが、手順を多少変えたので備忘録として公開しておく。

まず Github Pages での公開方法にはざっくり2種類あって、どちらでも公開可能なのだが、ここでは User Pages を使った方法で進める。

---

Github Pages ざっくり分類

__Project Pages__

* `gh-pages` という orphan ブランチ(履歴の無いブランチ)を作成すると公開される。
* master ブランチ直下に `docs` ディレクトリを作成して、静的ファイルを配置すると公開される。参考: [2016年新機能! GitHubのmasterブランチをWebページとして公開する手順](http://qiita.com/tonkotsuboy_com/items/f98667b89228b98bc096)


__User/Organization Pages__

* `<your_name>/<your_name>.github.io` というリポジトリを作成すると master ブランチが公開される。

---

### リポジトリ作成

公開用リポジトリ(User Pages)とソース管理用のリポジトリの2つを用意する。

`<your_name>/blog-hugo`

* ソース管理用リポジトリ。リポジトリ名は自由。
* `<your_site>/public` 以外の管理をする。

`<your_name>/<your_name>.github.io`

* 静的サイト公開用リポジトリ。User Pages なのでリポジトリ名はこの通りに作る。
* `<your_site>/public` 配下を管理する。

.gitignore に themes を除外するように設定しておく。  
※ 自分はやらなかったけど、個別の theme のリポジトリを submodule にしておくのがいいかも。


### ソース公開用リポジトリの作業

事前にローカルで作成済みの、プロジェクトのルートディレクトリ直下で行う。

```bash
$ cd <your_site>  # プロジェクトのルートディレクトリ
$ git init
$ git remote add origin git@github.com:<your_name>/blog-hugo.git
$ git pull origin master
$ rm -rf public  # public は submodule としてあとで追加するのでいったん削除
$ git add -A
$ git commit -m "[add] Hugo template."
$ git push origin master
```


### サイト公開用リポジトリの作業

git の submodule として追加する。

```bash
$ cd <your_site>  # プロジェクトのルートディレクトリ
$ git submodule add -b master git@github.com:<your_name>/<your_name>.github.io.git public
$ hugo  # 静的サイトが public 配下に再生成される
```

### サイト公開時のデプロイ自動化

上記で再生成した public 配下のページのデプロイを自動化する。これも公式ページのスクリプトがあるので最新版はそちらを参考にされたし。

```bash
$ vi deploy.sh
#!/bin/bash

echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

# Build the project.
hugo # if using a theme, replace by `hugo -t <yourtheme>`

# Go To Public folder
cd public
# Add changes to git.
git add -A

# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"

# Push source and build repos.
git push origin master

# Come Back
cd ..

$ chmod +x deploy.sh
$ ./deploy.sh "Your optional commit message"
```

deploy.sh も忘れずに push しておく。

```bash
$ git add deploy.sh
$ git commit deploy.sh -m "[add] deploy script."
$ git push origin master
```

### 参考にしたサイト
* [Hugo Part 2 - Hugo で github にブログを立ち上げる](http://blog.syati.info/post/create_hugo_2/)
