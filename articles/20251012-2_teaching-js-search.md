---
title: "【Javascript】配列の検索で迷わないためのチートシート"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["javascript", "array", "cheatsheet", "beginner"]
published: true
---
## はじめに
配列の検索って、メソッドが多くて混乱しませんか？  
`filter`も`find`も`some`も、それぞれ微妙に違ってややこしい…。  
この記事はそんな「ちょっと整理したいな」「思い出したいな」というときに  
パッと見返せる**検索メソッドのチートシート**です。  
使い方をざっと振り返って、頭をスッキリ整えていきましょう🗒️

## 早見表
| メソッド | 目的 | 戻り値 | 見つからなかった場合 | 検索タイプ |
|-----------|------|----------|----------------------|-------------|
| `filter()` | 条件を満たす**すべての要素**を取得 | 配列 | 空配列 `[]` | コールバック関数 |
| `find()` | 条件を満たす**最初の要素**を取得 | 要素 | `undefined` | コールバック関数 |
| `findIndex()` | 条件を満たす**最初のインデックス**を取得 | 数値 | `-1` | コールバック関数 |
| `some()` | 条件を満たす要素が**1つでもあるか**判定 | 真偽値 | – | コールバック関数 |
| `every()` | **すべて**の要素が条件を満たすか判定 | 真偽値 | – | コールバック関数 |
| `includes()` | 値が配列に**含まれるか**判定 | 真偽値 | – | 値 |
| `indexOf()` | 最初に一致した要素の**インデックス**を取得 | 数値 | `-1` | 値 |
| `lastIndexOf()` | 最後に一致した要素の**インデックス**を取得 | 数値 | `-1` | 値 |


## コールバック関数で検索するメソッド
```javascript
// 検索対象の配列
const users = [
    { id: 1, name: "Alice", active: true,},
    { id: 2, name: "Bob", active: false,},
    { id: 3, name: "John", active: true,},
];
```
### 条件を満たす「すべての要素を配列で」取得　`filter()`
```javascript
const activeUsers = users.filter(u => u.active);
console.log({activeUsers});
/*
   [
    { id: 1, name: "Alice", active: true,},
    { id: 3, name: "John", active: true,}
    ]
 */
```
**【応用】`filter()`で検索 + `map()`で配列操作**
```javascript
// 特定条件を満たすユーザーのIDだけを抜き出す
const activeUserIds = activeUsers.map(u => u.id); // filter してから map
console.log({activeUserIds}); //[1, 3]
```

### 条件を満たす「最初の要素」を取得　`find()`
*見つからなければ `undefined` を返す
```javascript
const activeUser = users.find(u => u.active);
console.log({activeUser}); // { id: 1, name: "Alice", active: true,}
```

### 条件を満たす「最初の要素のインデックス」を取得　`findIndex()`
*見つからなければ `-1` を返す
```javascript
const activeUserIndex = users.findIndex(u => u.active);
console.log({activeUserIndex}); // 0
```

### 要素の中に「条件を満たすものが1つでもあるか」を真偽値で返す　`some()`
```javascript
const hasActive = users.some(u => u.active);
console.log({hasActive}); // true
```

### 「すべての要素が条件を満たしているか」を真偽値で返す　`every()`
```javascript
const hasInActiveAll = users.every(u => !u.active);
console.log({hasInActiveAll}); // 条件を満たさない要素もあるのでfalseが返る
```


## キーワード（文字列）で検索するメソッド

```javascript
// 検索対象の配列
const fruits = ["apple", "orange", "banana", "orange"];
```

### キーワードが「含まれているか」を真偽値で返す　`includes()`
```javascript
const includesOrange = fruits.includes("orange");
console.log({includesOrange}); // true
```

### 最初に一致した要素の「インデックス」を返す　`indexOf()`
*見つからなければ `-1` を返す
```javascript
const indexOfOrange = fruits.indexOf("orange");
console.log({indexOfOrange}); // 1
```

### 最後に一致した要素の「インデックス（位置）」を返す　`lastIndexOf()`
*見つからなければ `-1` を返す
```javascript
const lastIndexOfOrange = fruits.lastIndexOf("orange");
console.log({lastIndexOfOrange}); // 3
```

## まとめ
配列の検索メソッドはたくさんありますが、  
ポイントは **「何を取りたいか」** で使い分けることです。

- **条件に合うものを全部取りたい** → `filter()`  
- **1件だけ見つけたい** → `find()`  
- **存在するかだけ知りたい** → `some()` / `includes()`  
- **すべて条件を満たすか確認したい** → `every()`  
- **位置（インデックス）が欲しい** → `findIndex()` / `indexOf()` / `lastIndexOf()`

それぞれのメソッドの「目的」と「戻り値」を意識すると、もう迷わなくなります。  
この記事が、思い出したいときにパッと見返せるチートシートとして役立てば嬉しいです📋