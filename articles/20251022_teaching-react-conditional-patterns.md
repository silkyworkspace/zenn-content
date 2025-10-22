---
title: "Reactでよく使う条件分岐チートシート（&& / || / ?: / ?./??）"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [“react”, “javascript”, “frontend”, “pattern”]
published: true
---

## はじめに
Reactでコンポーネントを書くとき、
「この要素はログイン時だけ出したい」
「値がなければデフォルトを表示したい」
――そんな場面は日常茶飯事です。

ところが、JavaScript特有の **falsy値** の扱いを正しく理解していないと、
意図せず「0」や「空文字」が表示されたり、アプリが落ちたりすることも…。

この記事では、Reactでよく使う条件分岐（`&&`、`||`、`?:`、`?.`、`??` など）を
**実務的な例と注意点つき**でわかりやすく整理します。  
「あれ、ここ何で表示されないんだっけ？」となった時に、この記事をチートシートとして使ってください✨

## 前提知識：falsyな値とは
JavaScriptでは、Boolean文脈で`false`として評価される値が**8つ**あります：

```javascript
false     // 真偽値のfalse
0         // 数値のゼロ
-0        // 負のゼロ
0n        // BigIntのゼロ
""        // 空文字列
null      // null
undefined // undefined
NaN       // Not a Number
```

**これ以外はすべてtruthy**（`true`として評価される）
```javascript
"0"       // 文字列の"0" → truthy
"false"   // 文字列の"false" → truthy
[]        // 空配列 → truthy
{}        // 空オブジェクト → truthy
```

## Reactでよく使う条件分岐パターン

### 1. 論理AND演算子（&&）
**用途**: 条件が`true`の時だけ要素をレンダリング

```jsx
function UserGreeting({ isLoggedIn, username }) {
  return (
    <div>
      {isLoggedIn && <p>ようこそ、{username}さん</p>}
    </div>
  );
}
```

**注意点**: 左辺がfalsyな値（`0`や`''`）の場合、その値自体がレンダリングされる
```jsx
// 悪い例：itemsが0の時、画面に「0」が表示される
{items.length && <ItemList items={items} />}

// 良い例：明示的にbooleanに変換
{items.length > 0 && <ItemList items={items} />}
{Boolean(items.length) && <ItemList items={items} />}
```
:::message
`&&`の左は boolean にすると良い
:::

### 2. 論理OR演算子（||）
**用途**: フォールバック値の設定（左辺がfalsyな値の時に右辺を返す）

```jsx
function UserProfile({ user }) {
  return (
    <div>
      <img src={user.avatar || '/default-avatar.png'} />
      <p>{user.name || 'ゲストユーザー'}</p>
    </div>
  );
}
// user.nameが空文字列、null、undefinedなどのfalsyであれば'ゲストユーザー'を表示
```

**注意点**: `0`や`''`もfalsyとして扱われるため、意図しない動作になることがある
```jsx
function ProductStock({ stock }) {
  // stockが0の時も「在庫なし」と表示されてしまう
  return <p>{stock || '在庫なし'}</p>;
  
  // 0も有効な値として扱いたい場合は ?? を使う
  return <p>{stock ?? '在庫なし'}</p>;
}
```
:::message
0も有効な値として扱いたい場合は **Null合体演算子（??）** を使う
Null合体演算子（??）は、nullまたはundefinedの時だけフォールバック値を使用
:::


### 3. 三項演算子（? :）
**用途**: 条件によって異なる要素をレンダリング

```jsx
function LoginButton({ isLoggedIn, onLogin, onLogout }) {
  return (
    <button onClick={isLoggedIn ? onLogout : onLogin}>
      {isLoggedIn ? 'ログアウト' : 'ログイン'}
    </button>
  );
}
```

**複雑な場合の例**:
```jsx
function StatusBadge({ status }) {
  return (
    <span className={
      status === 'success' ? 'badge-green' :
      status === 'error' ? 'badge-red' :
      'badge-gray'
    }>
      {status}
    </span>
  );
}
```

### 4. オプショナルチェーン（?.）
**用途**: ネストしたプロパティへの安全なアクセス
`?.`の左が空値（undefined/null）を判定して、空値の場合はundefinedを返す
```jsx
function UserDetail({ user }) {
  return (
    <div>
      <p>郵便番号: {user?.address?.zipCode}</p>
      <p>会社名: {user?.company?.name}</p>
      {/* userやaddressがundefinedでもエラーにならない → undefinedを返す*/}
    </div>
  );
}
```

**配列やメソッドでも使える**:
```jsx
function CommentList({ post }) {
  return (
    <div>
      <p>コメント数: {post?.comments?.length ?? 0}</p>
      {post?.comments?.map((comment) => (
        <Comment key={comment.id} {...comment} />
      ))}
    </div>
  );
}
```

### 5. Null合体演算子（??）
**用途**: `null`または`undefined`の時**だけ**フォールバック値を使用

```jsx
function ProductPrice({ product }) {
  // priceが0の場合も表示したい
  const price = product.price ?? '価格未設定';
  
  return <p>価格: ¥{price}</p>;
}
```

**||との違い**:
```jsx
function Example({ count }) {
  // countが0の時
  const result1 = count || 10;  // 10（0はfalsyなので右辺が返る）
  const result2 = count ?? 10;  // 0（0はnullでもundefinedでもない）
  
  return <p>カウント: {count ?? 'データなし'}</p>;
}
```

| 演算子 | チェックする値 | 使い分け |
|--------|---------------|----------|
| `\|\|` | すべてのfalsyな値（`false`, `0`, `""`, `null`, `undefined`, `NaN`） | 空文字や0もフォールバック対象にしたい時 |
| `??` | `null`と`undefined`のみ | 0や空文字を有効な値として扱いたい時 |

### 6. 早期リターン（Early Return）
**用途**: 複雑な条件分岐をシンプルに

```jsx
function UserProfile({ user }) {
  if (!user) {
    return <p>ユーザーが見つかりません</p>;
  }
  
  if (user.isBlocked) {
    return <p>このユーザーはブロックされています</p>;
  }
  
  return (
    <div>
      <h2>{user.name}</h2>
      <p>{user.bio}</p>
    </div>
  );
}
```

### 7. 複数条件の組み合わせ
```jsx
function PostCard({ post, isAuthor, isAdmin }) {
  return (
    <div>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
      
      {/* 作者または管理者の場合のみ編集ボタン表示 */}
      {(isAuthor || isAdmin) && (
        <button>編集</button>
      )}
      
      {/* 公開済みかつ承認済みの場合 */}
      {post.isPublished && post.isApproved && (
        <span className="badge">公開中</span>
      )}
    </div>
  );
}
```
## ポイント整理
| 演算子 | 名前 | 使い分け |
|--------|----------|----------|
| `&&` | 論理AND演算子 | trueの時だけ表示（左辺の値に注意） |
| `\|\|` | 論理OR演算子 | フォールバック（すべてのfalsyが対象） |
| `? :` | 三項演算子 | if：状況で出し分けたいときに便利 |
| `?.` | オプショナルチェーン | ネストしたプロパティへの安全なアクセス |
| `??` | Null合体演算子 | null/undefinedだけフォールバック（0や空文字は有効値として扱う） |

## まとめ
Reactでの条件分岐は、シンプルに見えて意外と奥が深いテーマです。  
特に `&&` や `||` は **JavaScriptのfalsy値の扱い** を正しく理解していないと、思わぬバグや表示崩れを引き起こします。

「値がある／ない」や「状態に応じた表示」を実装する場面は、React開発では毎日のように登場します。  
この基本パターンを押さえておけば、どんな条件分岐にもスムーズに対応できるはずです💡  
ぜひ日々のコーディングで活用してください💪