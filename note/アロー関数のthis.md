[JavaScript: 通常の関数とアロー関数の違いは「書き方だけ」ではない。異なる性質が10個ほどある。 - Qiita](https://qiita.com/suin/items/a44825d253d023e31e4d)

## 通常の関数のthis
通常の関数には`this`があり、それの指す先は関数を実行したタイミングで決まる。
```js
function regular() {
    return this
}

// グローバルスコープで実行した場合、thisはグローバルオブジェクトを指す
console.log(regular() === window) //=> true

// メソッドとして実行した場合、thisはオブジェクトを指す
const obj = { method: regular }
console.log(obj.method() === obj) //=> true
```

## アロー関数のthis
アロー関数の場合、`this`が指す値はアロー関数の宣言時に決定される。
アロー関数にはそれ自身の`this`がなく、関数の外の`this`を関数内で参照できる。
つまり、静的スコープ　（レキシカルスコープ）の`this`を参照する。
```js
const arrow = () => {
    return this
}
// 関数の外側のthis
const lexicalThis = this

// 関数定義したタイミングで、関数の外側のthisを参照する
console.log(arrow() === lexicalThis) //=> true

// メソッドとして実行しても、thisはメソッドが属するオブジェクトを指さない
const obj = { method: arrow }
console.log(obj.method() === obj) //=> false
console.log(obj.method() === lexicalThis) //=> true

// ちなみに上のarrowのthisの振る舞いを、通常関数で再現すると次のようになります:
function regular() {
    return lexicalThis
}
```
クラスの中だとインスタンスを参照する。
ただし、クロージャになっているため実行コンテキストで`this`の値は変わらない。
```js
class C {
  a = 1;
  autoBoundMethod = () => {
    console.log(this.a);
  };
}

const c = new C();
c.autoBoundMethod(); // 1
const { autoBoundMethod } = c;
autoBoundMethod(); // 1
// 通常のメソッドであれば、この場合は未定義になるはずです。
```