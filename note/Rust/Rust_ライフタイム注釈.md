## 概要
ライフタイム注釈は、ライフタイムを変えるのではなく
複数の参照のライフタイムの関係を示すのに使われる。

ライフタイム注釈は`'a`のようにアポストロフィーで始まり、
通常全部小文字で短い名前である。
```rust
&'a i32

&'a mut i32
```

## 例
例として、文字列への参照を受け取り、より長い方を返す関数を実装する。
以下のコードはコンパイルエラーになる。
```rust
fn get_longest(x: &str, y: &str) -> &str {
	if x.len() < y.len() {
		y
	} else {
		x
	}
}
```
なぜなら、返り値が`x`と`y`のどちらのライフタイムを使えばよいかわからないため。

ライフタイム注釈を使うことで、
以下のように引数と戻り値が同じライフタイムを持つよう制約できる：
```rust
let st1 = String::from("hello");
let st2 = String::from("a");

let res1 = get_longest(&st1, &st2);
println!("{}", res1);
...
fn get_longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() < y.len() {
        y
    } else {
        x
    }
}
```
結果として、引数のうち短い方のライフタイムを返り値に適用することを
コンパイラに指示できる。

以下のような例はコンパイルエラーとなる：
```rust
let st3 = String::from("hello");
let res2;
{
	let st4 = String::from("a");
	// コンパイルエラー！
	res2 = get_longest(&st3, &st4);
}
println!("{}", res2);
```
`st4`のライフタイムが使われ、
その範囲はカーリーブラケット内部なのでコンパイルエラーとなる。

## ダングリングポインタ
ライフタイム注釈をしていても、
ローカル変数への参照を返すとダングリングポインタとなるため
コンパイルエラーとなる：
```rust
fn dummy1<'a>() -> &'a str {
    let s = String::from("demo");
    &s
}

fn dummy2<'a>() -> &'a i32 {
    let x = 10;
    &x
}
```

## 構造体定義のライフタイム注釈
構造体に参照を持たせる場合、
構造体定義の全ての参照にライフタイム注釈をつける必要がある。
```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}

fn main() {
    // 僕をイシュマエルとお呼び。何年か前・・・
    let novel = String::from("Call me Ishmael. Some years ago...");
    //                                                  "'.'が見つかりませんでした"
    let first_sentence = novel.split('.').next().expect("Could not find a '.'");
    let i = ImportantExcerpt {
        part: first_sentence,
    };
}
```

## ライフタイム省略規則
Rustの歴史上、あるパターンだと全く同じライフタイム注釈を書くことになることが発見された。
そこで、Rustチームはそのパターンの場合ライフタイムを明示的に書かなくて済むようにした。

## 静的ライフタイム
`'static`はプログラムの全期間生存できることを意味する。
文字列リテラルはすべて`'static`ライフタイムになる。
```rust
let s: &'static str = "I have a static lifetime.";
```
このテキストはプログラムのバイナリに直接格納され、常に利用可能。
ゆえに、全文字列リテラルのライフタイムは常に`'static`。

## ジェネリックな型引数、トレイト境界、ライフタイムを一度に
最長の文字列を返しつつ、アナウンスを出力する関数`longest_with_an_announcement`。
```rust
fn longest_with_an_announcement<'a, T>(
    x: &'a str,
    y: &'a str,
    ann: T,
) -> &'a str
where
    T: Display,
{
    //       "アナウンス！ {}"
    println!("Announcement! {}", ann);
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```
