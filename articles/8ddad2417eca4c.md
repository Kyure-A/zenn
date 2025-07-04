---
title: "zsh の predict-on を救いたい"
emoji: "🙌"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

## はじめに
皆さんは zsh を使っていますか？俺は使っています。皆さんは fish を使っているかもしれませんね。しかし、zsh は fish っぽくカスタマイズできるのです！...という話をしたいわけではないです。

zsh のコマンド補完プラグインとして有名なものとして zsh-autosuggestions があります:

https://github.com/zsh-users/zsh-autosuggestions

しかし、zsh にはビルトインで存在するコマンド補完プラグイン（？）があります。それが predict-on です:

https://github.com/zsh-users/zsh/blob/master/Functions/Zle/predict-on

predict-on は割と zsh チュートリアルみたいな記事群で .zshrc にて書かれているのを見たことがあるのですが、実用している人をあまり見たことがありません。
実際、[Google トレンド](https://trends.google.co.jp/trends/explore?date=all&q=zsh%20predict-on,zsh%20predict%20on,zsh-autosuggestions&hl=ja) を見ていただけたらわかるのですが、流行ったことがほとんどないらしいです。

この記事では、熱狂的な predict-on ユーザーである俺がどのように predict-on を活用しているかを述べ、少しでもユーザーを増やす (= 救う) ことに寄与できればと考えています

## 簡単な比較
以下のような挙動の違いから、zsh-autosuggestions のほうが使いやすいとされています。
### predict-on
  - 履歴ベースの予測（補完システムではない）
  - 入力の途中で補完されるため、侵襲的
  - 例: `git c` と入力 → 履歴から `git commit -m "fix bug"` を見つけて自動挿入

### zsh-autosuggestions
  - 履歴ベースがデフォルトだが、補完も選択可能
  - 右矢印キーで確定、無視も可能で、非侵襲的
  - 例: `git c` と入力 → グレーで `ommit -m "fix bug"` を表示（履歴から）

