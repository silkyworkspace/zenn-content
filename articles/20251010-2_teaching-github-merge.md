---
title: "【GitHub】プルリクを作成してマージする"
emoji: "🐈"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["github", "git", "beginner"]
published: true
---

## はじめに
**プルリクエスト（Pull Request）とマージ（Merge）** の基本操作をGithubの操作画面とともに簡単にまとめます。例として、feature/add-loginブランチで作業した内容を main ブランチに反映させるまでの流れを確認します。

## GitHubのプルリクとマージをするまで流れ
### 1. プルリクエストを作成する

feature/add-loginブランチで変更を push すると、GitHub上で以下のような表示が出る。
右上の **「Compare & pull request」** をクリックしてプルリクエスト作成画面へ。
![Githubのプルリクエストボタン](/images/github-pull-request01.webp)  

### 2. プルリクエストの内容を入力する

プルリクエスト画面では、次の内容を確認・入力する。

1. **base:** が `main`、**compare:** に自分の作業ブランチ（例：`feature/add-login`）が選ばれていることを確認  
2. 変更内容の詳細を入力  
3. 「Create pull request」をクリック  
![Githubのプルリクエストボタン](/images/github-pull-request02.webp)  

**例：詳細の書き方（Markdown）**
あくまで一例ですので、プロジェクトの書き方に従ってください。
```md
## 概要
ログイン画面を新規追加しました。

## 変更内容
- `login.html` を新規作成  
- ログインフォーム（メールアドレス・パスワード入力欄、ログインボタン）を追加  
- 基本的なレイアウトとスタイルを適用（仮デザイン）

## 動作確認
- フォームの送信ボタンが正常に動作することを確認  
- 入力欄のバリデーション（必須チェック）を確認  

## 備考
現時点では見た目のみの実装。認証処理は別ブランチで対応予定。
```

### 3. プルリクエストをマージする

プルリクエストを作成すると、変更内容の差分（差分ファイル一覧）が確認できる。
問題がなければ、下部の 「Merge pull request」 をクリックしてマージしたい意志をGithubに伝える。（**まだマージが確定したわけではないことに注意！**）
![GithubのMerge-pull-requestボタン](/images/github-pull-request03.webp)  

### 4. マージを確定する
「Merge pull request」を押すと、最終確認画面が表示される。
「Confirm merge」 をクリックすると、main ブランチへ変更が反映される。
![GithubのMerge-pull-requestボタン](/images/github-pull-request04.webp)  


### 5. マージ後の状態

マージが完了すると、GitHub上の main ブランチに変更が取り込まれる。

## まとめ
ブランチで作業 → プルリクを作成 → マージで反映
という流れを押さえれば、GitHub上での共同開発がスムーズに進みます。

Github基本操作とブランチの使い方は以下を参考にしてください。
📘 [【GitHub】基本操作とブランチの使い方](https://zenn.dev/divsawa/articles/20251010_teaching-github-branch-basic)
