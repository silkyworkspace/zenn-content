---
title: "【GitHub】ローカルフォルダをGitHubと連携する"
emoji: "🐈‍⬛"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["github", "git", "beginner"]
published: true
---
## はじめに
授業や実務で、「ローカルでつくプロジェクトをGitHubにアップしたい」という質問をよく受けます。  
この記事では、そんなときに役立つ基本的な手順を、実際のコマンド付きでまとめました。  
初めてGitを使う方でも、この流れで進めれば迷わず連携できます🌿

## 全体の流れ

1. ローカルリポジトリを初期化  
2. GitHubでリモートリポジトリを作成  
3. ローカルとリモートを接続
4. ファイルをコミット  
5. pushでアップロード完了！

## 手順

### 1. ローカルリポジトリを初期化 
```bash
# ローカルリポジトリ（ローカルのプロジェクトフォルダ）に移動
cd ~/Desktop/my-project

# Gitの初期化
git init

# .gitが設置されているか確認
ls -la
```
### 2. GitHubでリモートリポジトリを作成  
1. GitHubにログイン
2. Repositoriesタブを開く
2. 右上の「＋」 → **New** をクリック  
3. 必要項目を入力  
   - Repository name: 任意（例：`my-project`）  
   - Public / Private: どちらでもOK  
   - README:現段階では作成不要
4. **Create repository** ボタンを押して完了！

### 3. ローカルとリモートを接続
SSH接続、もしくは、HTTPS接続 を選択し、以下を実行
```bash
# ローカルとリモートを接続(SSHの場合)
git remote add origin git@github.com:ユーザー名/my-project.git

# ローカルとリモートを接続(HTTPSの場合)
git remote add origin https://github.com/ユーザー名/リポジトリ名.git
```
:::message
上記のコマンドは、作成したリポジトリ内の
「Quick setup — if you’ve done this kind of thing before」に記載されているコマンド。
コピーしてターミナルにペーストすると良い。
:::

```
# 接続中のリモートリポジトリを確認
git remote -v
```
```
origin  git@github.com:ユーザー名/my-project.git (fetch)
origin  git@github.com:ユーザー名/my-project.git (push)
```
:::message
- `origin` はリモートリポジトリのニックネーム。
- `(fetch)` は取得用、`(push)` は送信用のURLを意味する。  
- このように同じURLが2行表示されていれば、GitHubと正しく接続されている。
:::

### 4. ファイルをコミット 

メインブランチの名前が master になっているので main に変更する
```bash
# メインブランチの名前を「main」にする
git branch -M main
```

VSCodeを開く
```bash
# VSCodeで開く
code .
```

VSCode上で「README.md」を作成してから、以下を実行

```bash
# 変更内容をステージング
git add .

# コミット（メッセージをつけて記録）
git commit -m "first commit"

```

### 5. pushでアップロード完了！
```bash
# リモートリポジトリへ変更を反映
git push origin main
```
Githubのリポジトリを確認すると、「README.md」が反映されている。
初めてのpush成功！


## まとめ
今回は、**ローカルリポジトリを作成してGitHubのリモートリポジトリと連携する手順**を紹介しました。  
これとは逆に、**GitHub上でリポジトリを作成してからローカルに連携する方法**もあります。  
目的や作業の流れに合わせて使い分けてみてください。

📘 [【GitHub】リポジトリを作成してローカルにクローンする](https://zenn.dev/divsawa/articles/20251009_teaching-github-clone-guide)
