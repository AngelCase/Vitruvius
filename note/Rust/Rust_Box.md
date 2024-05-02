## 概要
Rustにおけるもっとも素直なスマートポインタがボックス。
ボックスはスタック領域に存在するデータをヒープ領域に持ってきて、そのデータに対するポインタを保持する。
![[Rust_boxポインタ_before.png]]
![[Rust_boxポインタ_after.png]]
```rust
let t1 = (10, String::from("hello"));

let mut b1 = Box::new(t1);
(*b1).1 += "world";
println!("{:?}", b1);
// 出力：(10, "helloworld")
```