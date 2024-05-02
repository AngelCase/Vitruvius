プリミティブ型は値渡し、オブジェクト型は参照渡しとなる。
```
// 値渡し
var a = 10
     b =  a
     b =  5
console.log(b) //5
console.log(a) //10

// 参照渡し
var a = { 10 }
     b = a
     b = { 9 }
console.log(b) // { 9 }　
console.log(a) // { 9 }
```