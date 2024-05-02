条件式は`bool`型である必要がある。
以下のコードはコンパイルエラーとなる：
```rust
fn main() {
    let number = 3;

    if number {
        println!("number was three");     // 数値は3です
    }
}
```
Rustでは論理値以外の値が、自動的に論理値に変換されることはない。

`if`は式なので、`let`文の右辺に持ってくることができる。
```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    // numberの値は、{}です
    println!("The value of number is: {}", number);
	// The value of number is: 5
}
```
`if`の各アームの結果になる可能性がある値は同じ型である必要がある。
次のコードはコンパイルエラーになる：
```rust
fn main() {
    let condition = true;

    let number = if condition { 5 } else { "six" };

    println!("The value of number is: {}", number);
}
```
