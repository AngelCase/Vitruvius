## 概要
以下のような、値が`3`の時だけ処理をしたい場合を考える。
```rust
let some_u8_value = Some(0u8);
match some_u8_value {
    Some(3) => println!("three"),
    _ => (),
}
```
`if let`でもっと簡潔に書ける。
```rust
let some_u8_value = Some(0u8);
if let Some(3) = some_u8_value {
    println!("three");
}
```

## else
`if let`における`else`は`match`式の`_`の場合に入るコードブロックと同じになる。
つまり、以下の`match`式と、
```rust
let mut count = 0;
match coin {
    // {:?}州のクォーターコイン
    Coin::Quarter(state) => println!("State quarter from {:?}!", state),
    _ => count += 1,
}
```
以下の`if let else`は等価。
```rust
let mut count = 0;
if let Coin::Quarter(state) = coin {
    println!("State quarter from {:?}!", state);
} else {
    count += 1;
}
```