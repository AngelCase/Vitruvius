![[Rust_package_crate_module.png]]
## パッケージ
ある機能群を提供する1つ以上のクレート。
`Cargo.toml`という、クレートのビルドのやり方を説明するファイルを持つ。

## クレート
- バイナリクレート：実行可能ファイル
  - 何個でも存在できる
- ライブラリクレート
  - 1つまでしか存在できない
[公開されているCrate](https://crates.io/)
### クレートルート
Rustコンパイラの開始点となり、クレートのルートモジュールを作るソースファイル。
Cargoは`src/main.rs`をパッケージと同じ名前のバイナリクレートのクレートルートとみなす。
同様に、`src/lib.rs`をパッケージと同じ名前のライブラリクレートのクレートルートとみなす。
`src/bin`ディレクトリにファイルを置くことで、複数のバイナリクレートを持てる。

## モジュール
クレート内のコードをグループ化し、可読性と再利用性を上げるのに役立つ。
モジュールには`mod`キーワードを付ける。
```rust
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}

        fn seat_at_table() {}
    }

    mod serving {
        fn take_order() {}

        fn serve_order() {}

        fn take_payment() {}
    }
}
```

## 例
以下のような階層構造を作る：
```
src
├── main.rs
├── vars
│  ├── sub_a.rs
│  └── sub_b.rs
└── vars.rs
```
### main.rs
```rust
mod vars;

fn main() {
    println!("Hello, world!");
    vars::run();
    vars::sub_a::func_a();
}

```
ここでのポイントは以下：
- moduleである`vars`の関数`run()`を呼び出している
  - 呼び出せるのは`vars.rs`で`pub`の指定があるから
- submoduleである`sub_a`の関数を呼び出している
  - 呼び出せるのは`vars.rs`で`pub`の指定があるから
### vars.rs
```rust
pub mod sub_a;
mod sub_b;

pub fn run() {
    println!("vars module");
    sub_b::func_b();
}
```
### vars/sub_a.rs
```rust
pub fn func_a() {
    println!("func a")
}
```
