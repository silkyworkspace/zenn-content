---
title: "【GitHub】安全に過去のコミットへ戻す方法 revert vs reset"
emoji: "🐈‍⬛"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["git", "github", "revert", "reset"]
published: true
---

## はじめに
Gitで**mainブランチを過去のコミットに戻したい**とき、
`revert` と `reset` のどちらを使うかによって履歴の扱いが大きく異なります。  

この記事では、  
- **安全に戻す（履歴を残す）** `revert`  
- **強制的に戻す（履歴を消す）** `reset`  

の2つの手法および手順をわかりやすく整理します🧭

## revert：履歴を残したまま戻す（安全策）
「mainを**そのコミットの状態に戻す**」= 今後の履歴を**書き換えない**安全な方法。
チーム開発では基本的にこちらを使うのが安心。

### 手順

1. **mainブランチに切り替える**
    ```bash
    git checkout main
    ```
2. **コミット履歴を1行で一覧表示**
    ```bash
    git log --oneline
    ```
    ```bash
    // 履歴が見れる
    a3c9b17  Fix typo in footer
    201016b  Update hero section
    1f9e3a0  Add header navigation
    履歴のID（ハッシュ）をコピーしておきます。
    ```
3. **指定した範囲のコミットを「打ち消す」準備をする**(201016b のコミットの戻る例)
    ```bash
    git revert --no-commit 201016b..HEAD
    ```
    💡ポイント  
    - `201016b..HEAD` は「201016b以降のすべての変更を取り消す」範囲指定 
    - `--no-commit` を付けることで、自動コミットをせず、**まとめて1つのコミット**にできる。これを省略すると、コミットごとに「Revertコミット」が大量に作成されてしまう。
4. **打ち消し変更を1つの新しいコミットとして記録**
    ```bash
    git commit -m "Revert to 201016b"
    ```
5. **mainブランチをpush**
    ```bash
    git push origin main
    ```
**手順まとめ**
```bash
# 1. mainブランチに切り替える
git checkout main

# 2. コミット履歴を確認
git log --oneline

# 3. 戻したいコミットを確認（例: 201016b）
git revert --no-commit 201016b..HEAD

# 4. 1つのコミットとしてまとめる
git commit -m "Revert to 201016b"

# 5. push
git push origin main
```
> VSCodeのGUIで実施する場合
> 画面下部の**Git Graph**をクリック、→ Git Graphが表示されるので、戻したい時点のコミットメッセージ上で右クリック → `revert`を選択


## reset：履歴を残さずに巻き戻す（危険）
reset は履歴を書き換えて強制的に過去へ戻す方法。
個人開発や試験的リポジトリでのみ使うようにする⚠️

### 手順
1. **mainブランチに切り替える**
    ```bash
    git checkout main
    ```
2. **コミット履歴を1行で一覧表示**
    ```bash
    git log --oneline
    ```
    ```bash
    // 履歴が見れる
    a3c9b17  Fix typo in footer
    201016b  Update hero section
    1f9e3a0  Add header navigation
    履歴のID（ハッシュ）をコピーしておきます。
    ```
3. 念のためバックアップブランチを作成（任意）
    ```bash
    git branch backup/before-reset-$(date +%y%m%d)
    ```
4. **指定コミットへ完全に巻き戻す**
    ```bash
    git reset --hard 201016b
    ```
5. リモートも上書き（共有リポジトリは要注意）
    ```bash
    git push origin main --force-with-lease
    ```

**手順まとめ**
```bash
# 1. mainブランチに切り替え
git checkout main

# 2. コミット履歴を確認
git log --oneline

# 3. 念のためバックアップブランチを作成（任意）
git branch backup/before-reset-$(date +%y%m%d)

# 4. 指定コミットへ完全に巻き戻す
git reset --hard 201016b

# 5. リモートも上書き（共有リポジトリは要注意）
git push origin main --force-with-lease
```
**注意点**
- `--hard` は ワークツリーとステージを完全に上書きする。
未追跡ファイルは残るので、必要なら `git clean -fd`。
- `--force-with-lease` を使えば、他人のpushを誤って上書きしにくくなる。
- 万一のときは `git reflog` で過去の状態を確認・復旧できる。

## revert と reset 使い分け
| 操作 | 履歴 | 安全性 | 例コマンド例 |
|-----------|------|---------------|----------------|
| `revert` | 残す | 安全 | `git revert --no-commit <hash>..HEAD` |
| `reset` | 消す | 注意 | `git reset --hard <hash>` |

## まとめ
安全に巻き戻したいときはまず `revert`、
どうしても履歴ごと戻す必要があるときだけ `reset` を使いましょう。

特にチームで作業している場合、`revert`を選ぶのがベストです👍
