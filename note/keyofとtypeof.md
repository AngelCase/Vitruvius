## keyof
オブジェクト型のプロパティ名を取得する。
「型」に対して使う。
```ts
type SomeType = {
    foo: string;
    bar: string;
    baz: number;
}

const someKey: keyof SomeType; // someKey: 'foo' | 'bar' | 'baz'
```

## typeof
変数の値を型に変換する。
「変数」に対して使う。
```ts
const someObj = {
    foo: ''
}

const obj1: typeof someObj = { fo: ''} // error
const obj2: typeof someObj = { foo: ''} // not error
obj2.foo = 1 // error

// 初期値を入れない場合 
const obj!: typeof someObj;
```

## keyof typeof
`keyof`と`typeof`は併用できる。
以下は、オブジェクトのプロパティ名の`string`しか入力できない型を作る例。
```ts
const someObj1 = {
  foo: 'FOO',
  bar: 'BAR',
  baz: 'BAZ',
}

let obj1: keyof typeof someObj1; // obj1 = 'foo' | 'bar' | 'baz'
obj1 = 'foo';
```

## 例：オブジェクトの値を型として取り出す
```ts
const obj = { a: 1, b: 2, c: 3 } as const;

type type1 = typeof obj;
type type2 = keyof typeof obj;
// type4 = "a" | "b" | "c"
type type3 = (typeof obj)[keyof typeof obj];
// type3 = 1 | 2 | 3
```
`as const`をつけておかないと`type3`は`number`になってしまうので注意。

## 参考
[【TypeScript】keyofとtypeofよく忘れるのでまとめ #TypeScript - Qiita](https://qiita.com/tak001/items/bec3ab7c1bb4df52a7e7)
