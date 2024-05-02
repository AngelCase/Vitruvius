関数は最後の式を暗黙的に返す。
```rust
fn main() {
    let x = plus_one(5);

    println!("The value of x is: {}", x);
}

fn plus_one(x: i32) -> i32 {
    x + 1
}
```

[[Rust_式と文]]でも説明しているが、
`x + 1`を含む行の終端にセミコロンを付けるとエラーになる。
なぜなら、セミコロンを付けると式ではなく文となり、
文は値を返さないから。
