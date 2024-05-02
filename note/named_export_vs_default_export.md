## 結論
named exportがおすすめ。
片方変更した後にもう片方の変更し忘れが起きにくいため。
default exportはシンプルに書けるのが長所。

## named export
```js
// tom.js
export const tom = 'Tom'
```

```js
// index.js
import { tom } from './tom'

console.log(`Hello! I'm ${tom} .`)
// Hello! I'm Tom.
```

## default export
```js
// tom.js
export default 'Tom'
```

```js
// index.js
import tom from './tom'

console.log(`Hello! I'm ${tom} .`)
// Hello! I'm Tom.
```

## 参考
[named exportとdefault exportの違いやReactでの使い分け](https://jsnotice.com/posts/2021-01-30/)