`while true`と同じだが、条件を確認しないぶん若干高速。

```rust
fn main() {
    let mut count = 0u32;

    println!("Let's count until infinity!");

    // 無限ループ
    loop {
        count += 1;
        println!("{}", count);

        if count == 5 {
            println!("OK, that's enough");

            // ループを抜ける。
            break;
        }
    }
}
```

## 参考
[loop - Rust By Example 日本語版 (rust-jp.rs)](https://doc.rust-jp.rs/rust-by-example-ja/flow_control/loop.html)
