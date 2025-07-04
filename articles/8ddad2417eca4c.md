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

## 簡単な比較
以下のような挙動の違いから、zsh-autosuggestions のほうが使いやすいとされています。
### predict-on
  - Zshの標準機能で、履歴ベースの予測（補完システムではない）
  - 例: `git c` と入力 → 履歴から `git commit -m "fix bug"` を見つけて自動挿入
  - 入力の途中で補完されるため侵襲的

### zsh-autosuggestions
  - サードパーティプラグインで、履歴ベースがデフォルト（補完も選択可）
  - 例: `git c` と入力 → グレーで `ommit -m "fix bug"` を表示（履歴から）
  - 右矢印キーで確定、無視も可能で非侵襲的
  


[Google トレンド](https://trends.google.co.jp/trends/explore?date=all&q=zsh%20predict-on,zsh%20predict%20on,zsh-autosuggestions&hl=ja) を見ていただけたらわかるのですが、流行ったことがほとんどないらしいです。
