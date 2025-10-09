# Zenn CLI

* [📘 How to use](https://zenn.dev/zenn/articles/zenn-cli-guide)

## 操作
### 新しい記事を作成する
- slug指定なしで作成
```bash
$ npx zenn new:article
```
- slug指定して作成
```bash
$ npx zenn new:article --slug what-is-slug
# => articles/what-is-slug.md`が作成される
```
### 新しい本を作成する
```bash
$ npx zenn new:book
```
### 投稿をプレビューする
```bash
$ npx zenn preview
```