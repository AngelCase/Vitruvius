## 借用
変数の参照を渡すことを**借用**という。
例えば`s1`の参照は`&s1`のような記法となる。
関数の引数に変数を渡すとムーブされてしまうが、借用を使えばムーブは発生しないため便利。
```rust
let s1 = String::from("hello");
let len = calculate_length(&s1);
println!("The length of {} is {}", s1, len);
// 出力：The length of hello is 5
...
fn calculate_length(s: &String) -> usize {
    s.len()
}
```
## 可変な参照
参照した値を変更したければ、`mut`を付ければよい。
```rust
let mut s1 = String::from("hello");
change(&mut s1);
println!("{}", s1);
// 出力：hello_world
...
fn change(s: &mut String) {
    s.push_str("_world");
}
```

## 参照の制約
### 不変な参照は複数作れる
```rust
let s1 = String::from("hello");
let r1 = &s1;
let r2 = &s1;
println!("{} {}", r1, r2);
// 出力：hello hello
```
### 可変な参照は複数作れない
```rust
let mut s = String::from("hello");

let r1 = &mut s;
let r2 = &mut s;

println!("{}, {}", r1, r2);
// コンパイルエラー！
```
### 不変で借用しているとき、可変な借用はできない
```rust
let mut s11 = String::from("hello");
let r3 = &s11;
let r4 = &mut s11;
// コンパイルエラー！
println!("{} {}", r3, r4);
```

## ダングリングポインタ
参照しているポインタのメモリが解放されるとダングリングポインタとなるが、
Rustではそれが絶対に発生しない。
```rust
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String {
    let s = String::from("hello");

    &s
	// コンパイルエラー！
}
```
参照ではなく`String`を直接返せば問題なくなる。
```rust
fn no_dangle() -> String {
    let s = String::from("hello");

    s
}
```
