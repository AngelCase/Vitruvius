## 例
IPアドレスのバージョン4とバージョン6を扱う。
```rust
enum IpAddrKind {
    V4,
    V6,
}

let four = IpAddrKind::V4;
let six = IpAddrKind::V6;
```

## 列挙子に直接データを格納できる
列挙子に格納するデータの型、数は異なっていてもよい。
```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

let home = IpAddr::V4(127, 0, 0, 1);
let loopback = IpAddr::V6(String::from("::1"));
```
様々な型を埋め込める。
```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```

## パターンマッチング
`match`を使ってOSごとに異なる出力をさせる。
```rust
enum OS {
    Windows(u32, String),
    Mac(u32, String),
    Linux(u32, String),
}

fn main() {
    let linux = OS::Linux(1991, String::from("Linus"));
    print_os_info(linux);
    let windows = OS::Windows(1985, String::from("MicroSoft"));
    print_os_info(windows);
    let mac = OS::Mac(2001, String::from("Apple"));
    print_os_info(mac);
}

fn print_os_info(os: OS) {
    match os {
        OS::Windows(year, who) => {
            println!("Windows : First release in {} by {}", year, who);
        }
        OS::Mac(year, who) => {
            println!("macOS : First release in {} by {}", year, who);
        }
        OS::Linux(year, who) => {
            println!("Linux : First release in {} by {}", year, who);
        }
    }
}
```

## メソッド定義
それぞれのメッセージを出力する例。
構造体のように`impl`を使う。
```rust
#[derive(Debug)]
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}

impl Message {
    fn call(&self) {
        println!("{:#?}", self);
    }
}

fn main() {
    let m = Message::Write(String::from("hello"));
    m.call();
}
```

## 関連
[[Rust_match]]
