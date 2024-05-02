## ライブラリを作る
ライブラリを作る際は以下のコマンドでライブラリ用のパッケージを作れる：
```sh
cargo new --lib library_name
```
これを利用したい場合、GitHubにpushして、そのURLをCargo.tomlの`[dependencies]`にコピーする：
```toml
lib_demo = {git = "https://github.com/name/library_name"}
```
そのあと、ビルドすることでダウンロードできる：
```sh
cargo build
```
使用する際は以下のように記述する：
```rust
extern crate lib_demo;
...
lib_demo::run();
```

## 外部ライブラリを取り込む
以下のコマンドを実行する：
```sh
cargo add rand
```
または手動でCargo.tomlの`[dependencies]`にコピーする：
```toml
rand = "0.8.5"
```
そのあと、ビルドすることでダウンロードできる：
```sh
cargo build
```
使用する際は`use`でライブラリの内容を取り込むことができる：
```rust
use rand::Rng;
pub fn gen_ran() -> u8 {
	let mut rng = rand::threadrng();
	let n: u8 =rng.gen();
	n
}
```