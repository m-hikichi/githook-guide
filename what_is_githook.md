# Git Hookとは

Git Hookは，Gitリポジトリで特定のイベントが生じるたびに自動で実行されるスクリプトである．


## Git Hookのインストール

Hookは，すべてのGitリポジトリの`.git/hooks`ディレクトリに格納されている．リポジトリを初期化すると，このディレクトリにサンプルスクリプトが自動的に入力される．
`.git/hooks`の中を見ると，次のファイルが見つかる．`.sample`拡張子であるため，規定では実行されないようになっている．`.sample`拡張子を削除するだけで，Hookをインストールできる．または，新しいスクリプトを最初から作成する場合は，`.sample`拡張子を除いて上記のファイル名の1つと一致する新しいファイルを追加する．スクリプトを一から作成している場合，スクリプトのファイル権限を変更する必要がある場合もある．
```
.git/hooks
├── applypatch-msg.sample
├── commit-msg.sample
├── fsmonitor-watchman.sample
├── post-update.sample
├── pre-applypatch.sample
├── pre-commit.sample
├── pre-merge-commit.sample
├── pre-push.sample
├── pre-rebase.sample
├── pre-receive.sample
├── prepare-commit-msg.sample
├── push-to-checkout.sample
└── update.sample
```


## スクリプト言語

組み込みスクリプトは，ほとんどがシェルスクリプトだが，実行可能ファイルとして実行できる限り，任意のスクリプト言語を使用できる．各スクリプトのシバン行(`#!/bin/sh`)は，ファイルの解釈方法を定義する．したがって，別の言語を使用するには，シバン行を変更して使用のインタープリターのパスを指定すればよいだけである．例えば，シェルコマンドの代わりに，pythonスクリプトを記述する場合，シバン行を`#!/usr/bin/env python`のように書き換える．


## Hookの範囲

Hookは，特定のGitリポジトリに限定されており，`git clone`を実行しても新しいリポジトリにコピーされない．また，そのリポジトリへのアクセス権を持つ人なら誰でも変更できる．