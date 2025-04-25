---
title: "Twist.nix で再現性のある Emacs 設定を作る"
emoji: "🐙"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["emacs", "nix", "twist.nix"]
published: false
---

## はじめに

Nix を知って一年ほど経ちましたが、いまだに .emacs.d は手動で clone して管理している現状を改善したいと思い立ちました。
設定ファイルは [takeokunn](https://github.com/takeokunn) が書いて自身で使っていた [el-clone](https://github.com/takeokunn/el-clone) を使ってほぼ構成を真似て作っており、0.1 秒で起動する Emacs GUI で幸せになっていたのですが、彼は .emacs.d を Nix の設定に統合し、起動速度とトレードオフで Nix のもたらす再現性を手にしました。（悪口みたいになってごめん）

俺は起動速度と再現性を両立したいんだよ、ということで小難しそうなことで有名な [Twist.nix](https://github.com/emacs-twist/twist.nix) を Emacs の設定に導入しました。
ドキュメント・Example のリポジトリが 2 年ほど前から更新されておらず、Document as code の状態になっていたため、設定に苦労しました。

これについての知見を共有します:

## Twist.nix について
Twist.nix は Nix based な Emacs 向けのパッケージマネージャです。Emacs Lisp で書かれたものでいうと、

- package.el
- elpaca
- straight
- quelpa

が担っている部分を Nix が管理してくれるという認識です。Twist.nix を利用することで、Emacs Lisp パッケージのバージョンをユーザーが管理できる上、Nix がパッケージをビルドしていることが保証されるため、パッケージがインストールされているかのチェックが不要になる（してもいいが、パッケージが入っていることは保証されるのでしても意味がないという意）ことで Emacs の起動の高速化が見込めます。


## ディレクトリ構成
