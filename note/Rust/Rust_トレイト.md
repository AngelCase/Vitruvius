## 概要
トレイトは複数の型に対して共通の機能などを持たせたいときに使う。
他の言語におけるインターフェースと似た機能。
```rust
trait Fruits {
    fn price(&self) -> u32;
}
```

このトレイトを実装する型は、メソッドの本体に独自の振る舞いを提供する必要がある。
```rust
struct Apple;
impl Fruits for Apple {
    fn price(&self) -> u32 {
        10
    }
}

struct Banana;
impl Fruits for Banana {
    fn price(&self) -> u32 {
        5
    }
}
```
関数のデフォルト実装を用意しておくこともできる：
```rust
trait Message {
    fn message(&self) -> String {
        String::from("Message")
    }
}

struct NewsArticle {
    headline: String,
    location: String,
    author: String,
    content: String,
}

impl Summary for NewsArticle {}
```

## トレイトを実装していることを要求する
引数にトレイトを実装していることを要求する際には`impl Trait`構文が使える。
以下の例では`Summary`トレイトを実装していることを要求している：
```rust
fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
```
`impl Trait`構文は、トレイト境界構文の糖衣構文である。
以下の例は上の例と等価：
```rust
fn get_price<T: Fruits>(fruits: T) {
	println!("price is {}", fruits.price());
}
```
## 複数のトレイトを実装していることを要求する
以下のような`+`構文で行える：
```rust
pub fn notify(item: &(impl Summary + Display)) {
```
ジェネリック型につけたトレイト境界に対しても使える：
```rust
pub fn notify<T: Summary + Display>(item: &T) {
```

## where句
トレイト境界が増えてくると関数のシグニチャが読みにくくなる。
そのため、Rustには以下のように書く代わりに：
```rust
fn some_function<T: Display + Clone, U: Clone + Debug>(t: &T, u: &U) -> i32 {
```
`where`句を使って以下のように書ける：
```rust
fn some_function<T, U>(t: &T, u: &U) -> i32
    where T: Display + Clone,
          U: Clone + Debug
{
```

## トレイトを実装している型を返す
戻り値型のところで`impl Trait`構文を使えば、
あるトレイトを実装する何らかの型を返せる。
```rust
fn returns_summarizable() -> impl Summary {
    Tweet {
        username: String::from("horse_ebooks"),
        content: String::from(
            "of course, as you probably already know, people",
        ),
        reply: false,
        retweet: false,
    }
}
```
ただし、`impl Trait`は一種類の型を返す場合にのみ使える。
そのため以下のコードはコンパイルできない。
```rust
fn returns_summarizable(switch: bool) -> impl Summary {
    if switch {
        NewsArticle {
            headline: String::from(
                "Penguins win the Stanley Cup Championship!",
            ),
            location: String::from("Pittsburgh, PA, USA"),
            author: String::from("Iceburgh"),
            content: String::from(
                "The Pittsburgh Penguins once again are the best \
                 hockey team in the NHL.",
            ),
        }
    } else {
        Tweet {
            username: String::from("horse_ebooks"),
            content: String::from(
                "of course, as you probably already know, people",
            ),
            reply: false,
            retweet: false,
        }
    }
}
```

## トレイト境界を利用したメソッド実装の条件分け
以下の例では、`Display`トレイトと`PartialOrd`トレイトを実装しているときのみ、
`cmp_display`メソッドを実装する。
```rust
struct Pair<T> {
    x: T,
    y: T,
}

impl<T> Pair<T> {
    fn new(x: T, y: T) -> Self {
        Self { x, y }
    }
}

impl<T: Display + PartialOrd> Pair<T> {
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
            println!("The largest member is y = {}", self.y);
        }
    }
}
```
トレイトを実装するすべての型にトレイトを実装することができ、これをブランケット実装という。
標準ライブラリでは以下のように`Display`トレイトを実装するすべての型に`ToString`トレイトを実装している。
```rust
impl<T: Display> ToString for T {
    // --snip--
}
```
