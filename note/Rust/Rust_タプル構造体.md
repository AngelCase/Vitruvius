フィールドに型の指定だけがあり、名前がないタプル構造体を作れる。
```rust
struct Color(i32,i32,i32);
struct Point(i32,i32,i32);

let black = Color(0,0,0);
let origin = Point(0,0,0);

// パターンによる分解
let Color(r,g,b) = black;
println!("black: ({},{},{})", r, g, b);

// 添え字によるアクセス
println!("origin: ({},{},{})", origin.0,origin.1,origin.2);
```
