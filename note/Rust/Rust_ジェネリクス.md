## 関数
以下の例では、リストから最小値を探す関数`largest`でジェネリクスを使っている。
```rust
let number_list = vec![34, 50, 25, 100, 65];
println!("largest: {}", largest(number_list));
// 出力：100

let char_list = vec!['a', 'b', 'c', 'd'];
println!("largest: {}", largest(char_list));
// 出力：d
...
fn largest<T: PartialOrd + Copy>(list: Vec<T>) -> T {
    let mut largest = list[0];
    for item in list {
        if largest < item {
            largest = item;
        }
    }
    largest
}
```
なお、`largest()`のジェネリクスにおいて、比較部分でコンパイルエラーが起きないようにトレイト境界として`PartialOrd`と`Copy`を指定している。

## 構造体
```rust
struct Point<T> {
    x: T,
    y: T,
}

struct PointAnother<T, U> {
    x: T,
    y: U,
}
...
let p1 = Point { x: 1, y: 2 };
let p2 = Point { x: 1.0, y: 2.0 };

let p3 = PointAnother { x: 5, y: 10.4 };
let p4 = PointAnother { x: "Rust", y: 'a' };
```

## メソッド定義
```rust
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}

fn main() {
    let p = Point { x: 5, y: 10 };

    println!("p.x = {}", p.x());
}
```
`impl`の直後に`T`を宣言しなければならない点に注意。
これがないと、`Point<T>`の部分で「型`T`が見つからない」というエラーになる。

以下の`f32`だけにメソッド実装する場合であれば、
`f32`が何かはわかっているので`impl`の後に型を宣言しなくてよい。
```rust
impl Point<f32> {
    fn distance_from_origin(&self) -> f32 {
        (self.x.powi(2) + self.y.powi(2)).sqrt()
    }
}
```

構造体定義の型引数と、その構造体のメソッドに使う型引数は違うこともある。
以下の例だと`impl`の後の`<T, U>`とは異なる型`<V, W>`をメソッドの型引数に使い、
`self`の`Point`と引数で与えられた`Point`の`x`と`y`をそれぞれ使って合体させている。
```rust
struct Point<T, U> {
    x: T,
    y: U,
}

impl<T, U> Point<T, U> {
    fn mixup<V, W>(self, other: Point<V, W>) -> Point<T, W> {
        Point {
            x: self.x,
            y: other.y,
        }
    }
}

fn main() {
    let p1 = Point { x: 5, y: 10.4 };
    let p2 = Point { x: "Hello", y: 'c'};

    let p3 = p1.mixup(p2);

    println!("p3.x = {}, p3.y = {}", p3.x, p3.y);
}
```
`<V, W>`が`impl`の後にないのは、これらの型は`mixup`にしか関係ないから。

## パフォーマンス
Rustでは、ジェリネックな型を使っても
具体的な型を使う場合より遅くならないように実装されている。

単相化（monomorphization）によってこのパフォーマンスは実現されている。
単相化：コンパイル時に、使用されている具体的な型を入れることでジェリネックなコードを特定なコードに変換する過程

## 参考
[ジェネリックなデータ型 - The Rust Programming Language 日本語版 (rust-jp.rs)](https://doc.rust-jp.rs/book-ja/ch10-01-syntax.html)
