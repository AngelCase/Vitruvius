`use`で、モジュールを関数のスコープに持ち込める。

## 慣例
関数を持ち込む場合、
関数自体ではなく、関数の親モジュールを`use`する。
```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use self::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
}
```

一方、構造体やenum、その他の要素を持ち込むときはフルパスを書くのが慣例。
```rust
use std::collections::HashMap;

fn main() {
    let mut map = HashMap::new();
    map.insert(1, 2);
}
```

## as
同じ名前の要素を`use`しようとすると
どっちを意味しているのかRustが判別できなくなる。
そんな時は、親モジュールを`use`するか、
`as`でエイリアスを指定すればよい。
```rust
use std::fmt::Result;
use std::io::Result as IoResult;

fn function1() -> Result {
    // --snip--
    Ok(())
}

fn function2() -> IoResult<()> {
    // --snip--
    Ok(())
}
```

## pub use
`pub use`と書くことで、
要素をスコープに持ち込むだけでなく、
その要素があたかも自分たちのスコープで定義されていたかのように
外部のコードに公開できる。

このテクニックは再公開と呼ばれる。

例では`fuga`モジュールで定義されているかのように`func_hoge`を公開している。
```rust
mod hoge {
    pub fn func_hoge() {
        println!("hello")
    }
}

mod fuga {
    pub use crate::hoge::func_hoge;
}

fn main() {
    fuga::func_hoge();
}
```

## ネストしたパス
以下のコードは、
```rust
use std::cmp::Ordering;
use std::io;
```
次のようにまとめられる：
```rust
use std::{cmp::Ordering, io};
```

同様に以下のコードは、
```rust
use std::io;
use std::io::Write;
```
次のように`self`を使って表現できる：
```rust
use std::io::{self, Write};
```

## glob演算子
すべての公開要素をスコープに持ち込む場合、glob演算子`*`が使える。
```rust
use std::collections::*;
```
ただし使いどころに注意。なにがスコープにある値なのかわかりにくくなる。
テスト時に、テスト対象を持ち込むためにしばしば用いられる。

## 参考
[useキーワードでパスをスコープに持ち込む - The Rust Programming Language 日本語版 (rust-jp.rs)](https://doc.rust-jp.rs/book-ja/ch07-04-bringing-paths-into-scope-with-the-use-keyword.html)
