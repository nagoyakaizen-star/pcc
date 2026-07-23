# Q1 gitコマンドで２度目以降のコミット、プッシュする方法を教えてください。
A1 
2度目以降のコミットとプッシュは、**最初の設定（リモートリポジトリとの紐付け）がすでに終わっているため、非常にシンプル**です。

基本的には以下の**3ステップ**を順番に実行するだけです。

---

## 2度目以降の基本手順（3ステップ）

### 1. 変更したファイルをステージングに追加する

コミットしたい変更ファイルを登録します。

```bash
git add .

```

> **補足:** `.` は「変更されたすべてのファイル」を指します。特定のファイルだけを追加したい場合は `git add ファイル名` と指定します。

---

### 2. メッセージを添えてコミットする

ローカルリポジトリに変更履歴を記録します。

```bash
git commit -m "ここに変更内容のメッセージを書く"

```

> **例:** `git commit -m "ログイン画面のレイアウト修正"`

---

### 3. リモートリポジトリへプッシュする

2度目以降は、ブーム名やリポジトリ名を毎回指定しなくても `git push` だけで送信できます。

```bash
git push

```

---

## 💡 便利なショートカット（1と2をまとめる）

新規作成したファイルがなく、**既存のファイルを修正しただけ**であれば、`add` と `commit` を1つのコマンドにまとめることができます。

```bash
git commit -am "変更内容のメッセージ"

```

* `-a` (`--all`): 変更されたファイルを自動でステージングに追加する
* `-m`: コミットメッセージを指定する

※ ただし、**新しく作成したファイル（Untracked file）がある場合は `-a` では追加されない**ため、事前に `git add .` を実行するか、通常の3ステップで行うのが確実です。

---

## 状態を確認したいとき（おすすめ）

作業の途中で「今どのファイルが変更されたか」「`git add` が成功しているか」を確認したいときは、以下のコマンドをこまめに叩くと安心です。

```bash
git status

```

# error
# git push
Username for 'https://github.com': 

Password for 'https://nagoyakaizen-star@github.com':

To https://github.com/nagoyakaizen-star/OSEK.git

 ! [rejected]        main -> main (fetch first)
 
error: failed to push some refs to 'https://github.com/nagoyakaizen-star/OSEK.git'

hint: Updates were rejected because the remote contains work that you do not

hint: have locally. This is usually caused by another repository pushing to

hint: the same ref. If you want to integrate the remote changes, use

hint: 'git pull' before pushing again.

hint: See the 'Note about fast-forwards' in 'git push --help' for details.
## git pull
> git @ull

remote: Enumerating objects: 9, done.

remote: Counting objects: 100% (9/9), done.

remote: Compressing objects: 100% (5/5), done.

remote: Total 8 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)

Unpacking objects: 100% (8/8), 3.01 KiB | 39.00 KiB/s, done.

From https://github.com/nagoyakaizen-star/OSEK

   bd83bd7..5c2a405  main       -> origin/main

hint: You have divergent branches and need to specify how to reconcile them.

hint: You can do so by running one of the following commands sometime before

hint: your next pull:

hint:

hint:   git config pull.rebase false  # merge

hint:   git config pull.rebase true   # rebase

hint:   git config pull.ff only       # fast-forward only

hint:

hint: You can replace "git config" with "git config --global" to set a default

hint: preference for all repositories. You can also pass --rebase, --no-rebase,

hint: or --ff-only on the command line to override the configured default per

hint: invocation.

fatal: Need to specify how to reconcile divergent branches.

## git rebase
root@4d68184e0d21:/tmp/osek# git config pull.rebase false
root@4d68184e0d21:/tmp/osek# git config pull.rebase true
root@4d68184e0d21:/tmp/osek#   git config pull.ff only
root@4d68184e0d21:/tmp/osek# git pull
hint: Diverging branches can't be fast-forwarded, you need to either:
hint:
hint:   git merge --no-ff
hint:
hint: or:
hint:
hint:   git rebase
hint:
hint: Disable this message with "git config advice.diverging false"
fatal: Not possible to fast-forward, aborting.
root@4d68184e0d21:/tmp/osek# git rebase
Successfully rebased and updated refs/heads/main.
root@4d68184e0d21:/tmp/osek# git pull
Already up to date.
root@4d68184e0d21:/tmp/osek# git push
Username for 'https://github.com': nagoyakaizen-star
Password for 'https://nagoyakaizen-star@github.com':
Enumerating objects: 255, done.
Counting objects: 100% (255/255), done.
Delta compression using up to 12 threads
Compressing objects: 100% (138/138), done.
Writing objects: 100% (144/144), 120.65 KiB | 393.00 KiB/s, done.
Total 144 (delta 108), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (108/108), completed with 60 local objects.
To https://github.com/nagoyakaizen-star/OSEK.git
   5c2a405..ce9bc5c  main -> main
