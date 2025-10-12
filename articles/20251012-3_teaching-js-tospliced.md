---
title: "【Javascript】toSpliced()で破壊的メソッドを置き換える"
emoji: "🕊️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["javascript", "array", "ES2023"]
published: true
---
## はじめに

配列操作でおなじみの `push`, `pop`, `shift`, `unshift`。  
でもこれらは「破壊的メソッド」であり、元の配列を直接書き換えてしまいます。

ReactやVueなど、**状態をイミュータブルに保ちたい場面**では使いづらいですよね。

そんな悩みを解決するのが、ES2023で追加された **`toSpliced()`** です。  
これひとつでほとんどの破壊的メソッドを**安全に非破壊で再現**できます。

## `toSpliced()`の基本
引数の数によって挙動が変わることがポイント！
```js
// 引数が1つの場合 → 「削除開始index番号」から最後まで削除
arr.toSpliced(-1) // 末尾要素を削除

// 引数が2つの場合 → 「削除開始index番号」と「削除数」を指定
arr.toSpliced(0, 1) // 先頭要素を削除

// 引数が3つの場合 → 「削除開始index番号」と「削除数」と「挿入する値（複数可）」を指定
arr.toSpliced(-1, 1, "追加要素です") // 末尾を削除して末尾に"追加要素です"を追加

```

```js
// 例
const arr = ['A', 'B', 'C'];
const newArr = arr.toSpliced(1, 1, 'Z');
console.log(newArr); // ["A", "Z", "C"]
console.log(arr);    // ["A", "B", "C"] ←元は変わらない！
```
`splice()`と違って、新しい配列を返すだけで元の配列はそのまま（非破壊的メソッド）

## 各メソッドをtoSplicedで書き換えてみよう
### `push()`（末尾に追加）
```js
const arr = [1, 2, 3];
const newArr = arr.toSpliced(arr.length, 0, 4);
console.log(newArr); // [1, 2, 3, 4]
```

### `pop()`（末尾を削除）
```js
const arr = [1, 2, 3];
const newArr = arr.toSpliced(-1);
console.log(newArr); // [1, 2]
```
### `unshift()`（先頭に追加）
```js
const arr = [1, 2, 3];
const newArr = arr.toSpliced(0, 0, 0);
console.log(newArr); // [0, 1, 2, 3]
```

### `shift()`（先頭を削除）
```js
const arr = [1, 2, 3];
const newArr = arr.toSpliced(0, 1);
console.log(newArr); // [2, 3]
```

## まとめ

| 操作 | 従来の書き方 | 非破壊的に書くなら（toSpliced版） |
|------|----------------|--------------------------------|
| push（末尾に追加） | `arr.push(x)` | `arr.toSpliced(arr.length, 0, x)` |
| pop（末尾を削除） | `arr.pop()` | `arr.toSpliced(-1)` |
| unshift（先頭に追加） | `arr.unshift(x)` | `arr.toSpliced(0, 0, x)` |
| shift（先頭を削除） | `arr.shift()` | `arr.toSpliced(0, 1)` |

toSpliced()を覚えておくだけで、「配列を安全に扱う力」が一気に上がります。
ES2023で追加されたこの小さなメソッドが、これからの配列操作の標準になるかもしれません✨