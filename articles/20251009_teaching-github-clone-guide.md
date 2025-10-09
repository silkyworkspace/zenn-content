---
title: "GitHubでリポジトリを作成してローカルにクローンする"
emoji: "🐈"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["github", "git", "beginner"]
published: true
---
## はじめに
授業や実務でよく質問を受ける「GitHubでリポジトリを作成して、ローカルにクローンするまでの流れ」をまとめました。  
Gitの基本操作を整理したい方にもおすすめです。

## 1. GitHubでリポジトリを作成する
1. GitHubにログイン
2. Repositoriesタブを開く
2. 右上の「＋」 → **New** をクリック  
3. 必要項目を入力  
   - Repository name: 任意（例：`zenn-practice`）  
   - Public / Private: どちらでもOK  
   - README: 任意でチェックして作成  
4. **Create repository** ボタンを押して完了！
  
> - Githubのリポジトリは、**リモートリポジトリ** という。
> - `README.md` を同時に作っておくと、最初から説明文が表示される。

## 2. リポジトリURLをコピーする
作成後のページで、右上の「<> Code」ボタンをクリック。

- 初心者の方は **HTTPS** を選択してコピー（設定が不要で簡単）
- 開発環境でSSH鍵を設定している方は **SSH** を選択してコピー
```
例（HTTPS）：
https://github.com/ユーザー名/zenn-practice.git

例（SSH）：
git@github.com:ユーザー名/zenn-practice.git
```

## 3. ローカルにクローンする
ここでは、クローン先をデスクトップに作成する例を紹介。  
ターミナルを開き、以下の手順で進める。
```bash
# 現在のフォルダを確認
pwd

# フォルダ内の内容を確認
ls -la

# デスクトップへ移動
cd Desktop

# GitHubリポジトリをクローン（コピ-したURLをペースト）
git clone git@github.com:ユーザー名/zenn-practice.git
```
これで、ローカルに同名のフォルダが作成される。
完了したら、以下のコマンドでローカルフォルダに Git が設置されているか確認する。
```bash
cd zenn-practice
ls -la
```
`.git` というフォルダが表示されていれば、リポジトリのクローンが成功している。
```bash
total 8
drwxr-xr-x   4 divsawa  staff   128 10  9 23:23 .
drwx------@ 43 divsawa  staff  1376 10  9 23:23 ..
drwxr-xr-x  12 divsawa  staff   384 10  9 23:23 .git # ← gitの設置を確認
-rw-r--r--   1 divsawa  staff    15 10  9 23:23 README.md
``` 
> - .git フォルダは Git の管理情報を保存する隠しフォルダ。
> この中に履歴や設定などがすべて記録されている。
> - ローカルにクローンしたリポジトリは、**ローカルリポジトリ** という。

## 4. VSCodeで開く

クローンしたフォルダを VSCode で開く。

```bash
# クローンしたフォルダに移動
cd zenn-practice

# VSCodeで開く
code .
```

## 5. リモート接続を確認する

クローンしたフォルダが正しくGitHubとつながっているか確認する。
VSCode上のターミナルを開き、以下のコマンドを実行。

```bash
# 接続中のリモートリポジトリを確認
git remote -v
```
```
origin  git@github.com:ユーザー名/zenn-practice.git (fetch)
origin  git@github.com:ユーザー名/zenn-practice.git (push)
```

> - `origin` はリモートリポジトリのニックネーム。
> - `(fetch)` は取得用、`(push)` は送信用のURLを意味する。  
> このように同じURLが2行表示されていれば、GitHubと正しく接続されている。

## 6. 変更をコミットしてGitHubに反映する
VSCode上の「README.md」の内容に変更を加える。
```md
# zenn-practice

GitHubの操作練習用リポジトリです。  
リポジトリ作成からクローン、コミット、pushまでの基本操作を確認します。
```
VSCode上のターミナルを開き、以下のコマンドを実行。
```
# 変更内容をステージング
git add README.md

# コミット（メッセージをつけて記録）
git commit -m "README.md追記"

# リモートリポジトリへ変更を反映
git push origin main
```

Githubのリモートリポジトリ内のREADME.mdが変更されている。
![リモートリポジトリに変更を反映](/images/github-modify-readme.webp)

## 7. まとめ

ここまでで、GitHubとローカル環境をつなぐ基本の流れを学びました。

1. GitHubでリポジトリを作成  
2. URLをコピー（HTTPSまたはSSH）  
3. ローカルにクローン  
4. VSCodeで開く  
5. 接続確認  
6. 変更をコミットしてGitHubに反映  

これが、Gitを使った開発の「最初の一歩」です🎉

---

今回は **mainブランチに直接変更を加えました** が、  
実務では通常、**作業用のブランチを切ってから** mainブランチに反映します。  

次の記事では、  
GitHubでの基本操作や、ブランチを使った安全な開発の流れについて解説します👇

📘 [GitHubでの基本操作とブランチの使い方（準備中）](/articles/20251010_teaching-github-branch-basic)
