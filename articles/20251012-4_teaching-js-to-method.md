---
title: "【Javascript】toReversed / toSorted / toSpliced / with 新しい非破壊メソッドまとめ"
emoji: "🕊️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["javascript", "array", "es2023"]
published: true
---
## はじめに
ES2023で追加された `toSpliced()` が話題になっていますが、  
実はそれ以外にも便利な“非破壊メソッド”が追加されています。

これまでの `reverse()` や `sort()`、`splice()` は元の配列を直接変更してしまう「破壊的メソッド」でしたが、ついにES2023で、**安全に扱える「非破壊版」** が登場しました🎉

この記事では、実務でよく使う `toReversed()` / `toSorted()` / `toSpliced()` / `with()` の使い方をまとめます。

## なぜ「非破壊」が重要なのか

これまでの配列メソッドは、操作すると**元の配列そのものが変わってしまう**ことが問題でした。

```js
const arr = [1, 2, 3];
const reversed = arr.reverse();
console.log(reversed); // [3, 2, 1]
console.log(arr);      // [3, 2, 1] ← 元の配列も変わってしまう！
```
ReactやVueなど、イミュータブルな状態管理が必要な場面では、この仕様が思わぬバグの原因になっていました。

そこで登場したのが、ES2023の “to系メソッド” です。

##  新しく追加されたメソッド一覧
| メソッド | 役割 | 元のメソッド | 例 |
|-----------|------|---------------|----------------|
| `toReversed()` | 配列を逆順にした新しい配列を返す | `reverse()` | `[1,2,3].toReversed()` → `[3,2,1]` |
| `toSorted()` | ソートした新しい配列を返す | `sort()` | `[3,1,2].toSorted()` → `[1,2,3]` |
| `toSpliced()` | 要素を非破壊的に追加・削除 | `splice()` | `[1,2,3].toSpliced(1,1,"X")` → `[1,"X",3]` |
| `with()` | 指定位置の要素を置き換える | （新規） | `[1,2,3].with(1,"Z")` → `[1,"Z",3]` |

## `toSorted()`
sort() の非破壊版。比較関数も使える。

```js
const arr = [3, 1, 2];
const sorted = arr.toSorted((a, b) => a - b);

console.log(sorted); // [1, 2, 3]
console.log(arr);    // [3, 1, 2] ← 元の配列は変わらない
```
💡`toSorted()` は単純な昇順・降順以外にも、オブジェクト配列のソートにも安全に使える。
```js
const users = [{ id: 3 }, { id: 1 }, { id: 2 }];
const sorted = users.toSorted((a, b) => a.id - b.id);
console.log(sorted); // [{id:1},{id:2},{id:3}]
```

## `toSpliced()`
splice() の非破壊版。追加・削除・置換を新しい配列として返す。

```js
const arr = ['A', 'B', 'C'];

// 1つ目の要素を削除
console.log(arr.toSpliced(0, 1)); // ["B", "C"]

// 末尾を削除
console.log(arr.toSpliced(-1)); // ["A", "B"]

// 真ん中を差し替え
console.log(arr.toSpliced(1, 1, 'Z')); // ["A", "Z", "C"]
```
💡ポイント：引数の数で挙動が変わる
```js
// 引数が1つ → 指定indexから最後まで削除
arr.toSpliced(-1)

// 引数が2つ → 削除位置と削除数を指定
arr.toSpliced(0, 1)

// 引数が3つ以上 → 削除位置・削除数・挿入要素
arr.toSpliced(1, 1, "追加要素")
```

## `with()`
新しく追加された「部分上書き用」のメソッド。指定インデックスの要素を置き換えた新しい配列を返す。
```js
const arr = ["A", "B", "C"];
const newArr = arr.with(1, "Z");

console.log(newArr); // ["A", "Z", "C"]
console.log(arr);    // ["A", "B", "C"] ← 元の配列は変更なし
```
ReactのsetState()で一部要素を更新したいときにとても便利。

## まとめ
| 操作 | 従来のメソッド | 非破壊的に書くなら |
|------|----------------|----------------|
| 並び替え | `sort()` | `toSorted()` |
| 反転 | `reverse()` | `toReversed()` |
| 追加・削除 | `splice()` | `toSpliced()` |
| 上書き | （なし） | `with()` |

`toSpliced()` が注目されがちですが、
実は `toReversed()` や `toSorted()`、`with()` も実務で役立つメソッドです。

とくにReactなどのUIフレームワークでは、「非破壊的に書けるかどうか」が安定性に直結します。

これからは `to〇〇()`系メソッドで、安全・快適な配列操作を始めましょう🪄


📘 [【Javascript】toSpliced()で破壊的メソッドを置き換える](https://zenn.dev/divsawa/articles/20251012-3_teaching-js-tospliced)