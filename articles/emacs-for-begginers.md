---
title: "入門 Emacs 2024"
emoji: "🐙"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["emacs"]
published: false
---

## はじめに
学校の友人に「Vim キーバインドは使いたいけど Vim の設定は面倒くさい」という思想で VSCode を使っている人がいたため、「Spacemacs」はどうかと勧めてみたものの、素の Emacs との設定ファイルの記述方法の乖離で困っているようすでした。

この友人と同様に Emacs に入門でき損なっている人々をどうにかして Emacs ユーザーに仕立て上げたいという考えでこの記事を書きはじめます。

なるべくわかりやすい説明を心がけるので、Emacs をちゃんと使っている方にとっては厳密性にかける記述があるかもですが、大目に見てください (泣)

## Emacs のインストール
インストールはよしなにしてほしいものですが、一応 Windows ユーザーの方は WSL 上でインストールしてほしいです。 (Windows 版の Emacs は正直使えたもんではないので)

新しめの Windows ならば WSLg があるので GUI アプリとしての利用は問題ないとおもいます。


```shell
> emacs --version   
GNU Emacs 29.1
Copyright (C) 2023 Free Software Foundation, Inc.
GNU Emacs comes with ABSOLUTELY NO WARRANTY.
You may redistribute copies of GNU Emacs
under the terms of the GNU General Public License.
For more information about these matters, see the file named COPYING.
```

## Emacs のファイル周り
Emacs の設定ファイルやキャッシュファイルなどが入るディレクトリ (`user-emacs-directory`) は `~/.emacs.d/` となっています。
キャッシュファイルを除き、デフォルト設定においてユーザーが意識すべきものは以下の 2 つしかありません！:

```
.
└── .emacs.d/
    ├── init.el
    └── elpa/
        └── いろいろなパッケージ
```

### `init.el`
Emacs の設定を記述するファイルです。
### `elpa/`
Emacs にインストールしたパッケージ（プラグイン）が置かれるフォルダです。Node.js における `node_modules` だと思ってくださればわかりやすいと思います。


## 設定ファイルの作成
先述したとおり Emacs の設定は `~/.emacs.d/init.el` に記述します。

`init.el` は本質的には Emacs Lisp ファイル (`.el` は Emacs Lisp の拡張子) です。そのため、Emacs Lisp で記述したプログラムがすべて動きます。

### パッケージマネージャ
デフォルトの Emacs Lisp 構文ではパッケージの管理が難しいので、以下のようなパッケージマネージャのいずれかが用いられることが多いです (これらのパッケージマネージャでインストールしたパッケージが `~/.emacs.d/elpa/` におかれます)。

パッケージマネージャにはいろいろと派閥があります:
- `package.el`
Emacs にビルトインされているパッケージマネージャですが、これをそのまま使うことはほとんどないと思っていいです。
- `use-package`
Emacs 29.1 からビルトインされているパッケージマネージャですが、それ以前からも半ばデファクトスタンダードとして利用されています。
- `leaf.el`
`use-package` の代替を目指したパッケージであり、記法が同じといってもいいです。また、`use-package` の欠点を解消したパッケージでもあります。
作者が日本人であるため、主に日本の Emacs シーンではデファクトスタンダードとして利用されています。本記事では `leaf.el` を利用する前提で話を進めていきます。

### init.el の作成
以下のコードを `~/.emacs.d/init.el` に貼り付けるだけで、`leaf.el` を用いたパッケージ管理をはじめることができます。
```emacs-lisp
;; https://github.com/conao3/leaf.el#install から一部引用
(eval-and-compile
  ;; パッケージのホスト元を指定 (Arch Linux のソフトウェアリポジトリみたいなもの)
  (customize-set-variable
   'package-archives '(("org"   . "https://orgmode.org/elpa/")
                       ("melpa" . "https://melpa.org/packages/")
                       ("gnu"   . "https://elpa.gnu.org/packages/")))
  ;; package.el を初期化 (leaf.el をインストールするときのみ呼ぶだけで、今後 package.el を生で触ることはないです)
  (package-initialize)
  ;; leaf.el がインストールされていなければ leaf.el をインストールする
  (unless (package-installed-p 'leaf)
    (package-refresh-contents)
    (package-install 'leaf))

  ;; leaf.el 関連のパッケージの宣言 (leaf.el の記法については後述します)
  (leaf leaf-keywords
    :ensure t
    :init
    ;; optional packages if you want to use :hydra, :el-get, :blackout,,,
    (leaf hydra :ensure t)
    (leaf el-get :ensure t)
    (leaf blackout :ensure t)

    :config
    ;; initialize leaf-keywords.el
    (leaf-keywords-init)))

;; これの下に設定を書く

(provide 'init)
```

## `leaf.el` の使い方
`leaf.el` はパッケージ関連の設定をまとめて設定ファイルの可読性を上げることにも貢献します。
この形で設定ファイルを記述します。現時点では以下に述べる 2 つのキーワードについて理解していれば問題ありません。

```emacs-lisp
(leaf example
  :ensure t
  :config
  ;; ここにパッケージ関連の設定を記述
  )
```

### `:ensure` キーワード
このキーワードは引数として真偽値をひとつ取ります。

ここで Emacs Lisp のお勉強ですが、Emacs Lisp において真偽値は以下のようになります:
- 偽となる値は `nil`
- 真となる値は `nil` ではない値 (真偽値としては `t` を指定したほうがいいですが)

Emacs の起動時に `init.el` は読み込まれるわけですが、`:ensure` キーワードの引数に `t` が指定されていれば、読み込まれたときにパッケージが読み込まれていなければ自動で読み込んでくれます。`nil` なら読み込まなくていいという指定になります。

### `:config` キーワード
このキーワードは任意の数の引数を取ることができます。ここには Emacs Lisp で記述したプログラムならなんでもかけますが、そのパッケージに関連する設定を書くのが適切です。


### その他のキーワード
その他のキーワードについては [leaf-keywords.el](https://github.com/conao3/leaf-keywords.el) のドキュメントがわかりやすいです。
とはいえ、最初のうちは `:config` 内にすべて書く運用で問題ないですから、`leaf.el` についてなんとなく理解してきたくらいで見てみるといいと思います。

