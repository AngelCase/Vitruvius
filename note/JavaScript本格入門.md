## 分割代入とスプレッド構文の組み合わせ
スプレッド構文`...`を使うことで、個々の変数に分解されなかった残りの要素をまとめて配列として切り出せる。
```js
let data = [56, 40, 26, 82, 19, 17, 73, 99];
let [x0, x1, x2, ...other] = data

console.log(x0); // 結果：56
console.log(x1); // 結果：40
console.log(x2); // 結果：26
console.log(other); // 結果：[82, 19, 17, 73, 99]
```

## 分割代入による変数の入れ替え
一時変数を用意することなく入れ替えできる。
```js
let x = 1;
let y = 2;
[x, y] = [y, x];

console.log(x, y); // 結果：2 １
```

## オブジェクトの分割代入
配列の分割代入は`[]`だが、オブジェクトの場合代入先を`{}`でくくる。
```js
let book = { title: 'Javaポケットリファレンス', publish: '技術評論社', price: 2680 };
let { price, title, memo = 'なし' } = book;

console.log(title); // 結果：Javaポケットリファレンス
console.log(price); // 結果：2680
console.log(memo); // 結果：なし
```

## 等価演算子`==`と同値演算子`===`
`==`の場合データ型を変えてでも比較しようとするため、想定外の挙動になりやすい。
そのため、基本的にはデータ型を変換することなく比較する`===`を使う。

## 配列を順に処理するなら`for ... of`を使う
連想配列には`for ... in`を使って要素を順に処理できる。
```js
var data = { apple: 150, orange: 100, banana: 120 };
for ( var key in data ) {
	console.log(key + '=' data[key]);
}
```
出力：
```
apple=150
orange=100
banana=120
```
配列でも`for ... in`を利用することは可能だが、以下のようなデメリットがあるため配列に使うことは推奨されない。
- 配列の`prototype`に関数を追加すると、配列要素だけでなく追加した関数まで列挙されてしまう
- 処理の順序が保証されない
- 仮変数には配列要素ではなくインデックス番号が格納される

代わりに`for ... of`を用いるのが良い。
```js
var data = [ 'apple', 'orange', 'banana'];
Array.prototype.hoge = function() {}
for (var value of data) {
	console.log(value);
} // 結果：apple、orange、bananaを順に出力
```

## JavaScriptの危険な構文を禁止するStrictモード
長い歴史を持つJavaScriptには、「仕様としては存在するが、現在では安全性や効率面で利用すべきでない構文」が存在する。
Strictモードはこれらを検出してエラーとして通知してくれる。
また、「非Strictモードのコードよりも高速に動作する場合がある」という副次的なメリットも存在する。
Strictモードを有効にするには、スクリプトの先頭か関数の先頭に`'use strict';`という分を追加するだけでよい。
Strictモードに対応していないブラウザも存在するが、その場合`'use strict';`はただの文字列として無視されるため、新規の開発ではStrictモードを有効にして損はない。

## Symbolオブジェクト
同じ引数で生成されても、別々に生成されたシンボルは別物として扱われる。
```js
let sym1 = Symbol('sym');
let sym2 = Symbol('sym');

console.log(typeof sym1);     // 結果：symbol
console.log(sym1.toString()); // 結果：Symbol(sym)
console.log(sym1 === sym2);   // 結果：false
```
列挙定数として利用できる。
```js
const MONDAY = Symbol();
const TUESDAY = Symbol();
```

## JavaScriptは引数の数をチェックしない
そのため以下のようなコードでエラーが出ない。
```js
function showMessage(value) {
	console.log(value);
}

showMessage(); // 結果：undefined
showMessage('山田', '鈴木'); // 結果：山田
```
なお、仮引数よりも実引数のほうが多い場合、あぶれた引数は切り捨てられるわけではなく、`arguments`オブジェクトで取得できる。

## 必須な引数を宣言する方法
デフォルト値に例外を発出する関数を指定することで、引数が指定されなかった場合例外が発生する。
```js
function required() {
	throw new Error('引数が不足しています');
}

function hoge(value = required()) {
	return value;
}

hoge();
```

## 分割代入で不要な戻り値は捨てられる
```js
function getMaxMin(...nums) {
	return [Math.max(...nums), Math.min(...nums)];
}

let [,min] = getMaxMin(10, 35, -5, 78, 0);
```

## タグ付きテンプレート文字列
テンプレート文字列では、文字列リテラルに変数を埋め込める。
```js
`string text ${expression} string text`
```
タグ付きテンプレート文字列では、変数埋め込みの際に処理を行える。
`` 関数名`テンプレート文字列` ``の形で表現する。
```js
let person = 'Mike';
let age = 28;

function myTag(strings, personExp, ageExp) {
  let str0 = strings[0]; // "That "
  let str1 = strings[1]; // " is a "
  let str2 = strings[2]; // "."

  let ageStr;
  if (ageExp > 99){
    ageStr = 'centenarian';
  } else {
    ageStr = 'youngster';
  }

  // テンプレートリテラルを用いて組み立てた文字列を返すこともできます
  return `${str0}${personExp}${str1}${ageStr}${str2}`;
}

let output = myTag`That ${ person } is a ${ age }.`;

console.log(output);
// That Mike is a youngster.
```

## クロージャ
クロージャとは一言でいうと、「自分を囲むスコープ内の変数を参照している関数」。
```js
function closure(init) {
	var counter = init;

	return function() {
		return ++counter;
	}
}

var myClosure1 = closure(1);
var myClosure2 = closure(100);

console.log(myClosure1()); // 結果：2
console.log(myClosure2()); // 結果：101
console.log(myClosure1()); // 結果：3
console.log(myClosure2()); // 結果：102
```
`closure`関数が終了しても、戻り値の匿名関数が`counter`を参照しているため`counter`は破棄されず保持される。
つまり、`counter`というプロパティと、インクリメント関数を持つオブジェクトのようなもの、とみることもできる。
ちなみに、例のような引数や戻り値が関数である関数のことを高階関数という。

## クラス
ES2015までは関数リテラルで無理やりクラスを表現していたが、ES2015から`class`が導入されクラス定義が書きやすくなった。
```js
class member {
	// コンストラクタ
	constructor(firstName, lastName) {
		this.firstName = firstName;
		this.lastName = lastName;
	}

	// メソッド
	getName() {
		return this.lastName + this.firstName;
	}
}

let m = new Member('太郎', '山田');
console.log(m.getName()); // 結果：山田太郎
```

## innerHTMLではなくtextContentを用いる
どちらも要素配下のテキストの取得・設定が行える。
違いとして、innerHTMLは与えられたテキストをHTMLとして認識するが、textContentはプレーンテキストとして埋め込むという点が異なる。
HTML文字列を埋め込んでしまうとXSS脆弱性が生まれてしまうため、textContentを用いるのが良い。

## Ajax（Asynchronous JavaScript + XML）
Ajaxとは、一言でいうと、「JavaScript（XMLHttpRequestオブジェクト）を利用してサーバ側と非同期通信を行い、受け取った結果をDOM経由でページに反映する仕組み。
従来のWebアプリ（同期通信）だと、サーバー通信のたびにユーザは結果が返されるのを待たなければならなかったが、Ajax（非同期通信）を利用すればサーバが処理中でもクライアント側で処理を継続できる。
また、必要な箇所だけ部分的に変更するため、ページ全体のリフレッシュも必要ない。

## Ajaxだからと言ってXMLが必須ではない
XMLをAjax通信に用いることは少なくなっている。その理由は以下。
- タグでくくるせいでデータが大きくなりやすい
- データを操作するためのDOMプログラミングはコードが冗長になりやすい
近年はJSON形式がよく利用される。
