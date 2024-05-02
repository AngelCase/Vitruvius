## 概要
`Result`は以下のような実装となっている：
```rust
pub enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

## 使用例
以下の例ではゼロ割算が起きるときは`Err`を返す：
```rust
pub fn run() {
    let res2 = division_result(5.0, 1.0);
    match res2 {
        Ok(x) => println!("Result: {:.3}", x),
        Err(e) => println!("{}", e),
    }
}

fn division_result(x: f64, y: f64) -> Result<f64, String> {
    if y == 0.0 {
        Err(String::from("Not allowed !!"))
    } else {
        Ok(x / y)
    }
}
```