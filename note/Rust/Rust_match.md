## `_`というプレースホルダー
`_`というパターンはどんな値にでもマッチする。
```rust
let some_u8_value = 0u8;
match some_u8_value {
    1 => println!("one"),
    3 => println!("three"),
    5 => println!("five"),
    7 => println!("seven"),
    _ => (),
}
```
他のアームよりも先に記述してしまうと、必ずマッチしてしまう。
```rust
let some_u8_value = 0u8;
match some_u8_value {
    _ => (),
    1 => println!("one"),
    3 => println!("three"),
    5 => println!("five"),
    7 => println!("seven"),
}
```
ただし、この場合他のアームのところに「到達しないパターンです」とwarningが出る。

また、マッチさせたいパターンが1つだけの場合、`if let`記法でより簡潔に書ける。
[[Rust_if_let]]

## 関連
[[Rust_列挙型]]
