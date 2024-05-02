## 概要
`?`は`Return`や`Option`を返す式の後ろにつけられる。
たとえば`Option`の場合だと、評価結果が`None`だった場合その場で`None`を返し関数を終了する。

以下の例では配列の存在しない要素にアクセスしようとするため、`sum_option()`は`None`を返す：
```rust
let a = [0, 1];
let res3 = sum_option(&a);
match res3 {
    Some(x) => println!("Total is: {}", x),
    None => println!("Out of index !!"),
}
// 出力："Out of index !!"
...
fn sum_option(a: &[i32]) -> Option<i32> {
    let a0 = a.get(0)?;
    let a1 = a.get(1)?;
    let a2 = a.get(2)?;
    Some(a0 + a1 + a2)
}
```