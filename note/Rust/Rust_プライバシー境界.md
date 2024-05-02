モジュールツリーの要素を示すにはパスを使う。
絶対パスの場合`crate`から始め、
相対パスは`self`、`super`、または今のモジュール内の識別子から始める。

Rustのプライバシーは以下のようになっている：
- 親は子の非公開要素を使えない
- 子は祖先の要素を使える
  - 子は自分の定義された文脈を見ることができるため

## pub
`pub`キーワードをつけることで公開できる。
```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub fn eat_at_restaurant() {
    // Absolute path
    // 絶対パス
    crate::front_of_house::hosting::add_to_waitlist();

    // Relative path
    // 相対パス
    front_of_house::hosting::add_to_waitlist();
}
```
`front_of_house`に`pub`がついていないが、`eat_at_restaurant`と兄弟なので参照可能。

## super
`super`を使えば親モジュールから始まる相対パスを表現できる。
```rust
fn serve_order() {}

mod back_of_house {
    fn fix_incorrect_order() {
        cook_order();
        super::serve_order();
    }

    fn cook_order() {}
}
```
`fix_incorrect_order`関数は`back_of_house`モジュールの中にいるので
`super`で`back_of_house`の親モジュールの`crate`に行ける。

## 構造体
構造体に`pub`を付けると、構造体は公開されるが
フィールドそれぞれを後悔するか否かは個別に決められる。
```rust
mod back_of_house {
    pub struct Breakfast {
        pub toast: String,
        seasonal_fruit: String,
    }

    impl Breakfast {
        pub fn summer(toast: &str) -> Breakfast {
            Breakfast {
                toast: String::from(toast),
                seasonal_fruit: String::from("peaches"),
            }
        }
    }
}

pub fn eat_at_restaurant() {
    // 夏 (summer) にライ麦 (Rye) パン付き朝食を注文
    let mut meal = back_of_house::Breakfast::summer("Rye");
    // やっぱり別のパンにする
    meal.toast = String::from("Wheat");
    println!("I'd like {} toast please", meal.toast);

    // 下の行のコメントを外すとコンパイルできない。食事についてくる
    // 季節のフルーツを知ることも修正することも許されていないので
    // meal.seasonal_fruit = String::from("blueberries");
}
```

## enum
enumを公開するとすべて公開される。
```rust
mod back_of_house {
    pub enum Appetizer {
        Soup,
        Salad,
    }
}

pub fn eat_at_restaurant() {
    let order1 = back_of_house::Appetizer::Soup;
    let order2 = back_of_house::Appetizer::Salad;
}
```
