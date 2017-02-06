+++
date = "2017-02-06T23:11:41+09:00"
title = "zplug を導入する"

+++

年末年始休みに macOS Sierra にアップデートして、そのついでに zsh 環境を見直して [zplug](https://github.com/zplug/zplug) を導入したのでその覚え書き。

## 環境
* macOS Sierra 10.12.1
* zsh 5.3.1 (x86_64-apple-darwin16.3.0)
 
## zplug
そろそろターミナル環境を見直すかーって機運の高まりを感じたので、いろいろ調べてみた。

* oh-my-zsh, Prezto は自分には不要なものが多い
* Antigen は重い
* zgen は Antigen に引きずられ過ぎ

んで、色々調べてたらで [zplug](https://github.com/zplug/zplug) というものがあると知って開発するポリシーが自分好みだったので採用してみた。

## .zshrc
下の設定を .zshrc に書いて問題なく動いている。

```zsh
# ----------------------------------
#  zplug configuration
# ----------------------------------
export ZPLUG_HOME=~/.zplug
[[ -d $ZPLUG_HOME ]] || curl -sL zplug.sh/installer | zsh

if [[ -d $ZPLUG_HOME ]]; then
  source $ZPLUG_HOME/init.zsh
fi

zplug "zplug/zplug"

# theme
zplug "yous/lime"

# command
zplug "stedolan/jq", \
  from:gh-r, \
  as:command, \
  rename-to:jq
zplug "b4b4r07/emoji-cli", \
  on:"stedolan/jq"
zplug "mrowa44/emojify", as:command
zplug "motemen/ghq", \
  as:command, \
  from:gh-r
zplug "peco/peco", as:command, from:gh-r
zplug "b4b4r07/peco-tmux.sh", \
  as:command, \
  on:"peco/peco", \
  use:"peco-tmux.sh", \
  rename-to:"peco-tmux"
zplug "junegunn/fzf-bin", \
  as:command, \
  from:gh-r, \
  rename-to:fzf
zplug "junegunn/fzf", \
  as:command, \
  on:"junegunn/fzf-bin", \
  use:"bin/fzf-tmux"

# 拡張
zplug "b4b4r07/enhancd", use:init.sh
zplug "mollifier/anyframe"
zplug "zsh-users/zsh-syntax-highlighting", defer:2
zplug "zsh-users/zsh-completions"

zplug check || zplug install
# zplug load --verbose
zplug load

# Lime theme settings
export LIME_DIR_DISPLAY_COMPONENTS=2

```

## 導入してみて
* シンプルで導入も簡単
* jq, ghq, peco のようなコマンド類は homebrew じゃなくて zplug で管理するようにした (brew / go get するものが減ってスッキリ)
* zplug 関係ないけど一緒に導入した enhancd が最高だった
* 環境依存かもしれないけど、起動がもうワンテンポ早くならないかな


## 参考
[公式の日本語ドキュメント](https://github.com/zplug/zplug/blob/master/doc/guide/ja/README.md)

[おい、Antigen もいいけど zplug 使えよ](http://qiita.com/b4b4r07/items/cd326cd31e01955b788b)

[ターミナルのディレクトリ移動を高速化する](http://qiita.com/b4b4r07/items/2cf90da00a4c2c7b7e60)




