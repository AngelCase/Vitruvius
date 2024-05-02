## 基本的な構造体
```rust
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}

...

// インスタンス化
let user1 = User {
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};

println!("{}", user1.username);
let user2 = user1;

// println!("{}", user1.username);
// 構造体には所有権の概念があるためコンパイルエラー！
```
ここで`&str`文字列スライスではなく所有権のある`String`を使用しているのは意図的。
参照を保持するためにはライフタイム機能を使う必要がある。

## 可変なインスタンス
インスタンス化時に`mut`を付ければよい。
ただし、一部のフィールドのみを可変にすることはできない。
```rust
let mut user1 = User {
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};

user1.email = String::from("anotheremail@example.com");
```

## フィールド初期化省略記法
以下は`Usesr`インスタンスを生成する関数の例だが、
`email`と`username`というフィールド名と変数を繰り返しており冗長。
```rust
fn build_user(email: String, username: String) -> User {
    User {
        email: email,
        username: username,
        active: true,
        sign_in_count: 1,
    }
}
```
以下のように省略可能。
```rust
fn build_user(email: String, username: String) -> User {
    User {
        email,
        username,
        active: true,
        sign_in_count: 1,
    }
}
```

## 構造体更新記法
既存のインスタンス`user1`のフィールドを、別のインスタンス作成時に再利用したい。
```rust
let user2 = User {
    email: String::from("another@example.com"),
    username: String::from("anotherusername567"),
    active: user1.active,
    sign_in_count: user1.sign_in_count,
};
```
その場合、以下のように`..`という記法で明示的にセットされてない残りのフィールドが設定される。
```rust
let user2 = User {
    email: String::from("another@example.com"),
    username: String::from("anotherusername567"),
    ..user1
};
```

## 構造体にメソッドや関連関数を定義する
矩形のインスタンスを作成して、その面積を出力する。
```rust
#[derive(Debug)] // Debugトレイトを自動実装、これによりprintlnできる
struct Rectangle {
    width: u32,
    height: u32,
}
impl Rectangle {
    fn create(width: u32, height: u32) -> Self {
        Self { width, height }
    }
    // 参照にしないとムーブになって以降インスタンスが使えなくなってしまう
    fn area(&self) {
        println!("{}", self.width * self.height);
    }

...
let rect = Rectangle::create(20, 20);
println!("{:#?}", rect);
rect.area();
println!("{:#?}", rect);
// area()でselfを参照にしているため、コンパイルできる
}
```
インスタンスを作成する関数`create()`は`self`をとらないため関連関数と呼ばれる。
面積を計算する関数`area()`はメソッドである。

メソッドと関数は概念としては異なり、
メソッドはオブジェクト指向において、オブジェクトの操作を定義したものである。
一方、関数は処理のまとまりのことを指す。

呼び出し方が異なり、メソッドは`.`で、関連関数は`::`で呼び出せる。

## 参考
[構造体を定義し、インスタンス化する - The Rust Programming Language 日本語版 (rust-jp.rs)](https://doc.rust-jp.rs/book-ja/ch05-01-defining-structs.html)
