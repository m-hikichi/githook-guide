# Git Hookとは

Git Hookは、Gitリポジトリで特定のイベントが生じたときに自動的に実行されるスクリプトです。Gitによって提供される機能であり、特定のイベントにフックして、ユーザー定義のアクションを実行することができます。たとえば、Gitコミットの前に実行するスクリプトを書いたり、Git pushの前に実行するスクリプトを書いたりすることができます。


## Git Hookの種類

Git Hookには、以下の種類があります。

- `pre-commit`：コミットの前に実行されるスクリプト
- `pre-push`：プッシュの前に実行されるスクリプト
- `post-commit`：コミットが完了した後に実行されるスクリプト
- `post-merge`：マージが完了した後に実行されるスクリプト
- `pre-rebase`：リベースの前に実行されるスクリプト
- `post-checkout`：チェックアウトが完了した後に実行されるスクリプト

これら以外にも、多くの種類のGit Hookがあります。各Hookには、実行されるタイミングや、実行時に渡される引数など、それぞれ異なる機能を持っています。


## Git Hookのインストール

Hookは、すべてのGitリポジトリの`.git/hooks`ディレクトリに格納されています。リポジトリを初期化すると、このディレクトリにサンプルスクリプトが自動的に入力されます。`.git/hooks`のディレクトリの中には，`.sample`拡張子がついている以下のファイルが格納されています．
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
これらは、Hookのサンプルスクリプトであり、デフォルトでは実行されません。Hookをインストールするには、`.sample`拡張子を削除するだけで良いです。または、新しいスクリプトを作成する場合は、`.sample`拡張子を除いたファイル名と同じ名前の新しいファイルを`.git/hooks`ディレクトリに追加することでHookをインストールできます．<br>
新しいスクリプトを作成する場合，スクリプトのファイル権限を変更する必要がある場合があります．ファイル権限を変更するには，`chmod`コマンドを使用します．例えば，`pre-commit`フックをインストールする場合，以下の手順を行います．
1. `.git/hook`ディレクトリに移動します．
```bash
cd .git/hooks
```
2. `pre-commit.sample`ファイルを`pre-commit`という名前でコピーします．
```bash
cp pre-commit.sample pre-commit
```
3. `pre-commit`ファイルの実行権限を与えます．権限を与えることで，`pre-commit`フックが実行されるようになります．ファイルの権限を変更しない場合，意図した動作をしない可能性があります．
```bash
chmod +x pre-commit
```
以上の手順を実行することで，`pre-commit`フックが有効になります．必要に応じて，スクリプトを編集してカスタマイズすることができます．

### Git Hookの一時無効化手順

`--no-verify`オプションを使用することで，Git Hookを一時敵に無効にし，commitやpushを実行できます．<br>
```
git commit --no-verify -m "commit message"
```
注）hookは通常，コードの品質や安全性を維持するために導入されているため，不適切な理由で無効にすることは推奨されません．hookを無効にする理由が正当であることを確認してください．


## スクリプト言語

Git Hookスクリプトは、通常、シェルスクリプトで書かれていますが、実行可能ファイルであればどのようなスクリプト言語でも使用できます。ただし，スクリプトファイルの先頭にある「シバン行」と呼ばれる特別な行を指定する必要があります．<br>
シバン行は，ファイルの解釈方法を定義するために使用されます．例えば，シェルスクリプトの場合は「`#!/bin/bash`」という行を先頭に記述します．このシバン行によって，スクリプトを解釈するインタープリタが指定されます．異なるスクリプト言語を使用する場合は，シバン行を変更して，使用するインタープリタのパスを指定します．<br>
例えば，Pythonで書かれたスクリプトをHookに使用したい場合は，以下のようにシバン行を指定します．
```bash
#!/usr/bin/env python
```
これにより，Pythonのインタープリタがスクリプトを解釈するようになります．シバン行は，スクリプトの先頭に記述することで，自動的に使用するインタープリタを指定できるため，スクリプトの実行方法を簡単かつ一貫性のあるものにすることができます．


## Git Hookの範囲

Git Hookは，特定のGitリポジトリ内でのみ有効で，`git clone`を実行しても新しいリポジトリにコピーされません．しかし，Hookの設定をチーム内に共有し，ルール化することができます．
### 共有するGit Hookの作成手順
1. リポジトリルートに`.githooks`ディレクトリを作成します．このディレクトリはGit Hookスクリプトファイルを置く場所となります．
```bash
mkdir .githooks
```
2. `.githooks`ディレクトリにGit Hookスクリプトファイルを作成します．Git Hookの種類に応じて，スクリプトファイルを作成します．例えば，`pre-commit`フックを設定したい場合は，`.githooks/pre-commit`というファイルを作成します．
```bash
touch .githooks/pre-commit
```
3. Git Hookスクリプトファイルに実行権限を付与します．
```bash
chmod +x .githooks/*
```

### Git Hookの参照先の変更

Git Hookの参照先デフォルトパスは`.git/hooks`ですが，`core.hooksPath`を設定することで`.githooks`ディレクトリに変更することができます．
1. Git Hookの参照先を`.githooks`に変更します．
```bash
git config --local core.hooksPath .githooks
```
2. Git Hookの参照先を確認するには，以下のコマンドを実行します．表示されるリストの中にて`core.hookspath=.githooks`となっていれば，参照先が`.githooks`に変更されていることを確認できます．
```bash
git config --local --list
```
以上の設定を行うことで，リポジトリ内で共有されているHookスクリプトを使うことができます．プロジェクトに参加するメンバーは，最初に上記のコマンドを実行する必要がありますが，以後はプロジェクト内で作られGit管理されているHookスクリプトが適用されます．これにより，Hookスクリプト自体のメンテナンス性も上がります．

#### Git Hookの参照先をデフォルトに戻す
Git Hookの参照先をデフォルトに戻す場合は，以下のコマンドを実行します．
```bash
git config --local core.hooksPath .git/hooks
```