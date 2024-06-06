## object型は良くない
`object`はJavaScriptのほぼすべてのオブジェクトを指してしまうので避ける。
```ts
// NG: object 指定だと、JavaScriptに存在するほぼ全てを受け付けてしまう
// NG: 例1
const regexp: object = new RegExp(/^Hello/)

// NG: 例2
function add(point1: object, point2: object): object {
  return { x: point1.x + point2.x, y: point1.y + point2.y }
}

// OK: 具体的な型を書いている
// OK: NG例1の改善
const regexp: RegExp = new RegExp(/^Hello/)

// OK: NG例2の改善
function add(point1: { x: number; y: number }, point2: { x: number; y: number }): { x: number; y: number } {
  return { x: point1.x + point2.x, y: point1.y + point2.y }
}
```

## ts独自の文法：コンストラクタの省略記法
```ts
// コンストラクタでの省略記法
class Point {
  constructor(
    private x: number,
    private y: number
  ) {}
}
```

```ts
// 省略記法を使わない定義
class Point {
  private x: number
  private y: number
  
  constructor(x: number, y: number) {
    this.x = x
    this.y = y
  }
}
```

## 構造的部分型
tsでは、型が合致しているかの確認にプロパティの有無を確認している。

例えば、以下の例だと`showMessage`の引数に`Error`を渡しているが、
これをtsはエラーにしない。
```ts
type LogType = { message: string }

function showMessage(log: LogType): void {
  console.log(log.message)
}

showMessage({ message: 'hoge' })
showMessage(new Error('error'))
```
その理由は、`Error`が`message`というプロパティを持っているためである。
```ts
interface Error {
    name: string;
    message: string;
    stack?: string;
}
```

## インターセクション型
参考：https://typescriptbook.jp/reference/values-types-variables/intersection

オブジェクトの定義を合体できる。
```ts
type XY = {
  x: number;
  y: number;
};
 
type Z = {
  z: number;
};
 
type XYZ = XY & Z;
 
const xyz: XYZ = {
  x: 0,
  y: 1,
  z: 2,
};
```
### 使い所
必須パラメータと任意パラメータを持つ型を作りたい時に使える。
```ts
type Mandatory = Required<{
  id: number;
  name: string;
}>;
 
type Optional = Partial<{
  hobby: string;
}>;
```
以下のように合体できる。
```ts
type Parameter = Mandatory & Optional;
```

## 注意：type alias / Interface とタイプガード
`typeof`、`instanceof`、`in`はjsの演算子。
type alias、Interfaceはtsの独自の構文なので、
jsに変換すると記述が残らない。

以下のコードはエラーになる。
```ts
type HogeType = { hoge: string; }

declare function getHoge(): HogeType;

const hoge: HogeType = getHoge()

// エラー：'HogeType' only refers to a type, but is being used as a value here.
if (hoge instanceof HogeType) {
}
```
`instanceof`は、コンストラクタがオブジェクトのプロトタイプチェーンにあるかどうかを確認する。
構造を判定したければ、以下のどれかの方法が使える。
- classを使う
- `in`を使ってプロパティを持っているか判定する
- タグ付きユニオン型

## 型の拡大（Widening）の防止
以下のようなケースでWideningが発生する。
```ts
const str1 = "hoge" // "hoge"（リテラル型）
let str2 = str1     // string

// { name: string, version: number }
const obj = {
  name: 'Taro',
  age: 20,
}
```
オブジェクトは`const`でも中身は書き換えられてしまうためWideningが発生する。

これを防止するには、型注釈をつける他に
`as const`をつける方法もある。
```ts
const str1 = "hoge" as const // "hoge"（リテラル型）
let str2 = str1              // "hoge"（リテラル型）

// const obj: {
//     readonly name: "Taro";
//     readonly age: 20;
// }
const obj = {
  name: 'Taro',
  age: 20,
} as const
```

## unknown型
どんな型でも代入できるが、
`unknown`型を代入することはできない。
`any`よりはマシ。

## never型
`never`型しか代入できない。
タイプガードなどの処理の結果、到達不能だと推論されると`never`型になる。

使い所として、条件分岐を全て網羅しているのかに使える。
```ts
type Extension = "js" | "ts" | "json";

function printLang(ext: Extension): void {
  switch (ext) {
    case "js":
      console.log("JavaScript");
      break;
    case "ts":
      console.log("TypeScript");
      break;
    default:
      // "json"を網羅していないのでエラーが出る
      // エラー：Type 'string' is not assignable to type 'never'.
      const exhaustivenessCheck: never = ext;
      break;
  }
}
```

## satisfies
参考：https://zenn.dev/monicle/articles/62974d8e9f704d
型推論の結果を保持しつつ、型チェックできる。

例えば、以下の例だとエラーになる。
```ts
const user: Record<string, number | string> = {
  name: "Taro",
  age: 20
};

console.log(user.age + 5)
```
理由は`user.age`の型が`number | string`となっているため。

以下のように、`satisfies`をつけると、
`user.age`の型は`number`になってくれる（型推論の結果の保持）。
```ts
const user = {
  name: "Taro",
  age: 20
} satisfies Record<string, number | string>;

console.log(user.age + 5)
```
`age: true`とかにするとちゃんとエラーになってくれる（型チェック）。

また、`as const`を併用することも可能。
`as const satisfies Record<string, number | string>;`
