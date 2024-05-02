## 概要
Rustには、他の言語のようなnull機能がない。
`Option`は以下のような実装となっている：
```rust
pub enum Option<T> {
    None,
    Some(T),
}
```

## 使用例
以下の例では、ゼロ割算が起きないように、割る数がゼロの場合は`None`を返すようにしている：
```rust
fn main() {
    let res1 = division_option(5.0, 0.0);
    match res1 {
        Some(x) => println!("Result: {:.3}", x),
        None => println!("Not allowed !!"),
    }
}

fn division_option(x: f64, y: f64) -> Option<f64> {
    if y == 0.0 {
        None
    } else {
        Some(x / y)
    }
}
```
`Some(x)`にマッチした場合、`x`は`Some`に含まれる値に束縛されている。

## Noneを使う場合型を教える必要がある
`Some`の場合、型は推論できるため教える必要はない。
一方、`None`の場合、
`None`値を見ただけだと`Some`列挙子が保持する型をコンパイラが推論できないため
教える必要がある。
```rust
let some_number = Some(5);
let some_string = Some("a string");

let absent_number: Option<i32> = None;
```
