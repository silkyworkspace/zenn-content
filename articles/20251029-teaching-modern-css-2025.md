---
title: "【CSS】サラッと使いこなす！モダンCSS 8選"
emoji: "🫰🏼"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["css", "frontend", "webデザイン", "レスポンシブ", "モダンCSS"]
published: true
---
## はじめに
最近のCSSは、ちょっとした指定で驚くほど便利に書けるようになっています。
この記事では、実務でも使えるモダンCSSをサラッと理解して、今日から使える形で紹介します。

## 1. `:has()`（親要素を子の状態で変えられる）
これまでCSSでは「子要素を親に応じて変える」ことはできても、  
「**親を子の状態で変える**」ことはできませんでした。  
`:has()`を使うとそれが可能になります！

**例1：子要素に画像を含む親要素に指定する**
```html
<!-- 例1 -->
<div class="card">
    <img src="sample.jpg" alt="画像" width="100">
    <p>画像付きカード（青枠になる）</p>
</div>

<div class="card">
    <p>画像なしカード（通常表示）</p>
</div>
```

```css
/* 例1: 画像を含むカードのスタイルを変更 */
  .card:has(img) {
    border: 3px solid blue;
    background-color: #f0f8ff;
  }
```
![:has()の例1](/images/modern-css-has01.webp) 

**例2：子要素のチェックボックスが選択されている親要素に指定**
```html
<!-- 例2 -->
<label>
    <input type="checkbox"> チェックボックス
</label>
```

```css
/* 例2: チェックされたチェックボックスを含むラベルの色を変更 */
label:has(input:checked) {
color: green;
font-weight: bold;
}
```
![:has()の例2](/images/modern-css-has02.webp)

## 2. `@container`（コンテナの幅でデザインを変える）
これまでは「画面の幅」でしかレスポンシブができませんでした。
でも実際には、部品の入っている“箱（コンテナ）”の幅で切り替えたい場面が多いですよね。
それを叶えるのが `@container` です。
**例：コンテナの幅によって部品の配置を切り替える**
```html
<!-- 例 -->
<h2>コンテナクエリ</h2>
<div class="container">
    <div class="card">
        <h3>レスポンシブカード</h3>
        <div class="card-content">
            <div class="image-box"></div>
            <div class="text-box">
                <p>コンテナの幅によって背景色やレイアウトが変わります</p>
                <ul>
                    <li>500px未満: グレー、縦並び</li>
                    <li>500px以上: 水色、横並び</li>
                    <li>700px以上: 緑色、大きい余白</li>
                </ul>
            </div>
        </div>
    </div>
</div>
```
```css
/* コンテナを定義 */
.container {
    container-type: inline-size; /* 必須！コンテナ化する宣言 */
    container-name: card-container; /* コンテナに名前をつける(なくてもよい) */
    border: 2px solid #333;
    padding: 20px;
    margin: 20px;
    resize: horizontal;
    overflow: auto;
}

.card {
    background: #f0f0f0;
    padding: 15px;
    border-radius: 8px;
}

.card h3 {
    margin: 0 0 10px 0;
    font-size: 1.2rem;
}

.card-content {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

/* コンテナが500px以上の時 */
@container  card-container (min-width: 500px) {
    .card {
        background: lightblue;
    }

    .card-content {
        flex-direction: row;
        align-items: center;
    }

    .card h2 {
        font-size: 1.5rem;
    }
}

/* コンテナが700px以上の時 */
@container card-container (min-width: 700px) {
    .card {
        background: lightgreen;
        padding: 30px;
    }

    .card h2 {
        font-size: 2rem;
    }
}

.image-box {
    width: 100px;
    height: 100px;
    background: #ddd;
    flex-shrink: 0;
}

.text-box {
    flex: 1; /* 残りのスペースを全て使う */
}
```
![@container](/images/modern-css-container-query.gif)

## 3. `aspect-ratio`（比率を固定できる）
以前は`img`などをトリミングしたい場合、`width`と`height`を指定してましたが、
`aspect-ratio` があれば1行で完結です。
```css
img {
  aspect-ratio: 16 / 9;
  object-fit: cover;
  width: 100%;
  border-radius: 8px;
}
```
📷 よく使う場面
- サムネイル画像
- YouTube埋め込み
- カードレイアウトの見た目統一

## 4. `clamp()`（最小・理想・最大をまとめて指定）
「これ以上小さくならず、これ以上大きくならないけど、その間は自動で伸び縮みしてね」という指定
フォントサイズや余白を、画面幅に応じて自然に変えたいときに便利！
昔は `@media` で細かく指定していましたが、`clamp()` なら一行で完結です。

**例1： フォントサイズの自動調整（流体タイポグラフィ）**
```css
h1 {
  font-size: clamp(20px, 5vw, 36px);
}
```
↑ 読み方
最小値：20px
理想値：5vw
最大値：36px
画面が狭いときは20pxまで小さく、大きいときも36px以上にはならない！

**例2： レスポンシブなカードやコンテナ幅**
固定幅でもなく、100%でもない「中間の柔軟さ」を出すとき。
```css
.card {
  width: clamp(280px, 60%, 800px);
}
```
⚠️ .cardがフレックスの子要素だった場合は、280px未満まで縮むことがあります。
（`flex-shrink: 1`が初期値であるため、必要に応じて縮んでしまうから）
このパターンで最小値 280px を本当に守りたい場合は、`min-width`を入れます。
```css
.card {
  width: clamp(280px, 60%, 800px);
  min-width: 280px; /* ← これで 280px を下回らない */
}
```
**例3： ヒーローエリアなどの高さ指定**
ファーストビューなどで「スマホでは低め」「PCでは高め」にしたいとき。
```css
.hero {
  height: clamp(60vh, 80vh, 100vh);
}
```

## 5. `:is() / :where()`（複数セレクタをまとめる）
複数セレクタをまとめて書くための省略記法です。
```css
/* .card h2, .card p, .card li と書くのと同じ意味 */
.card :is(h2, p, li) {
  color: #333;
}
```
:::message
`:is()`は、中の一番強いセレクタの特異性になることに注意です。
例えば、`:is(.class, #id)` と書くと #id と同じ強さ になるので、強すぎて他のスタイルが上書きできなくなることもあります。

`:where()`は、構文は同じですが、特異性が常に0なのが特徴で、リセットCSSなどに最適。
:::

## 6. `position: sticky`（親の範囲内で固定）
スクロールしても一定位置に固定されるけど、**親の範囲を超えない。**
JSなしで簡単に「固定見出し」が作れます。
![stickyの例](/images/modern-css-sticky.gif)
📌 使い所
- セクションタイトルを固定
- テーブルのヘッダー固定

:::message
- position: sticky は**親要素（スクロールコンテナ）の中で固定**
- position: fixed は**ビューポート（画面）に対して固定**
:::

## 7. `@media (prefers-color-scheme)`(OSテーマに合わせて色を切り替え)
ユーザーのOS設定に合わせて自動でライト／ダークを切り替え。
```css
:root {
  --bg: white;
  --fg: black;
}

/* ダークテーマの時はカラーを変更 */
@media (prefers-color-scheme: dark) {
  :root {
    --bg: #111;
    --fg: #f3f3f3;
  }
}

body {
  background: var(--bg);
  color: var(--fg);
}
```

## 8. `inset`(top/right/bottom/leftを1行で指定)
inset は、「top / right / bottom / left」 をまとめて書けるショートハンド（省略記法） です。
CSSの中でも「位置指定をスッキリ書ける」モダンなプロパティです。

**例：要素を中央配置**
```html
<section class="container">
    <div class="inset-box">
        <h2>insetで中央配置します</h2>
        <p>この文章はダミーです。文字の大きさ、量、字間、行間等を確認するために入れています。この文章はダミーです。文字の大きさ、量、字間、行間等を確認するために入れています。この文章はダミーです。文字の大きさ、</p>
    </div>
</section>
```
```css
.container {
    position: relative;
    height: 100dvh;
    background-color: #515151;
}

.inset-box {
    position: absolute;
    inset: 0;
    margin: auto;
    width: 300px;
    height: 300px;
    background-color: #2bd6dc;
}
```
![insetの例](/images/modern-css-inset.webp)

## ポイント整理
| やりたいこと | 使うCSS | ポイント |
|---------------|----------|-----------|
| 子要素の状態で親を変えたい | `:has()` | JSを使わず「親→子」の関係を逆転できる |
| コンポーネント単位でレスポンシブ | `@container` | ビューポートではなく“箱の幅”で切り替え |
| 比率を固定したい | `aspect-ratio` | 画像やカードを均一に整える |
| フォントや余白をなめらかに変化 | `clamp()` | 最小・理想・最大の3つを一行で指定 |
| セレクタをまとめたい | `:is()` / `:where()` | コードを短く・読みやすくできる |
| スクロールで固定表示したい | `position: sticky` | 親の範囲内で固定できる |
| OSテーマに合わせて色を切り替え | `@media (prefers-color-scheme)` | ダークモード対応もCSSだけでOK |
| 位置指定をまとめて書きたい | `inset` | top/right/bottom/leftを1行で指定 |


## まとめ
モダンCSSは、見た目を整えるだけでなく、
「コードをシンプルに」「メンテしやすく」してくれる頼もしい存在です。
「これ、CSSでできるかも？」と思ったら、今日紹介した機能をぜひ思い出してください🎨