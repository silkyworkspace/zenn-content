---
title: "【React/GitHub】Viteで作ったReactアプリを別PCで動かすまでの流れ"
emoji: "🕌"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["react", "vite", "github", "npm", "beginner"]
published: true
---

## はじめに
「職場で作ったReactプロジェクトを家でも触りたい」  
──そんなときに便利なのが **GitHub** です。  

GitHubにプロジェクトをアップしておけば、  
別のパソコンで同じコードをクローンして作業を続けることができます。  

しかし、クローンしただけではアプリは動きません。  
そんなときに必要になるのが、次のコマンドです👇

```bash
npm install
```

この記事では、
- Viteを使ったプロジェクトを別PCで動かす手順
- どうして npm install が必要なのか
を初心者にもわかりやすく解説します。

## 使用環境について

この記事のReactプロジェクトは Vite を使って構築しています。
Viteは軽量で高速なビルドツールで、開発サーバーの起動がとても速いのが特徴です。

開発中によく使うコマンドは次の通りです：
```bash
npm run dev   # 開発サーバーを起動
npm run build # 本番用にビルド
```

## 手順
それでは、早速手順を紹介します。
### 1. 職場PCでGitHubにアップする
まず、職場PCで作業しているReact（Vite）プロジェクトをGitHubにアップロードします。
```bash
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/username/my-react-app.git
git push -u origin main
```

これでGitHub上にプロジェクトが保存されます。


### 2. 家のPCでクローンして動かす

次に、家のPCでGitHubからプロジェクトを取得します。

```bash
git clone https://github.com/username/my-react-app.git
cd my-react-app
npm install # ← 重要！
npm run dev
```
これで開発サーバーが起動し、ブラウザでアプリを確認できるはずです🎉
:::message
ローカルにクローンした後は、`npm install`をするということがポイントです！
:::

## なぜ npm install が必要か？

`npm install`をすることで、Reactプロジェクトを動かすためのライブラリをダウンロード・復元ができます。

Reactプロジェクトは、アプリ本体とは別に**動かすためのライブラリ**が必要です。
これらのライブラリは **node_modules フォルダ**にまとめられています。

しかしこのフォルダは非常に大きいため、GitHubにはアップされません。
（Viteの初期設定で.gitignore によって除外されています）

その代わり、どんなライブラリが必要かを一覧にして記録している**package.json** ファイルがプロジェクト内にあります👇

```json
// package.json
{
"dependencies": {
  "react": "^19.1.1",
  "react-dom": "^19.1.1"
},
"devDependencies": {
  "vite": "^7.1.7",
  "@vitejs/plugin-react-swc": "^4.1.0"
  }
}
```
この**package.json**を見て、`npm install`は以下のような処理をします。

### npm install 処理の流れ
1. **package.json** を読み取る
2. 必要な依存関係（dependencies / devDependencies）を確認
3. npm の公式リポジトリからライブラリをダウンロード
4. **node_modules フォルダ**に展開
5. **package-lock.json** でバージョンを固定


これによって、どのPCでも同じ環境を再現できるようになります。
実際のフォルダ構成は以下の通りです。
```bash
vite-project/
├── node_modules/         ← ダウンロードされた実体
├── package.json          ← 依存の一覧（どんなライブラリを使うか）
├── package-lock.json     ← 依存の固定情報（各ライブラリの正確なバージョン）
└── ...
```

## よくあるトラブルと対処法
| 症状 | 原因 | 対策 |
|------|------|------|
| `npm run dev` が動かない | まだ `npm install` をしていない | まず `npm install` を実行 |
| モジュールが見つからない | ライブラリが未インストール | `npm install` で再構築 |
| バージョンエラー | Node.js のバージョンが異なる | 職場と家で同じバージョンを使う |

💡 Node.jsのバージョン確認は node -v でできます。
💡 npmのバージョンも、Node.jsのバージョンとセットで確認しておくと安心です。
💡 npmのバージョン確認は npm -v でできます。
一般的には Node.js のバージョンに付属する npm をそのまま使えば問題ありません。


## まとめ
| ステップ | コマンド | 内容 |
|------|------|------|
| 職場PC | `git push` | まず `npm install` を実行 |
| 家PC | `git clone` | リポジトリを取得 |
| 家PC | `npm install` | 依存ライブラリをダウンロード・復元 |
| 家PC | `npm run dev` | 開発サーバーを起動 |

React + Viteのプロジェクトを別PCで動かすときは、
「クローン → npm install → npm run dev」が基本の流れです。

単なる作業手順として覚えるのではなく、
なぜそのコマンドが必要なのかを理解しておくと、
どんな環境でも自信を持って開発できるようになります💪
