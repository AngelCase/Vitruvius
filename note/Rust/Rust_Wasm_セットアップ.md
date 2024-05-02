## `rustup`、`rustc`、`cargo`
Rustのツールチェインとして`rustup`、`rustc`、`cargo`をインストール。
[Install Rust - Rust Programming Language (rust-lang.org)](https://www.rust-lang.org/tools/install)
```sh
$ rustup --version
rustup 1.26.0 (5af9b9484 2023-04-05)
info: This is the version for the rustup toolchain manager, not the rustc compiler.
info: The currently active `rustc` version is `rustc 1.71.0 (8ede3aae2 2023-07-12)`

$ rustc --version
rustc 1.71.0 (8ede3aae2 2023-07-12)

$ cargo --version
cargo 1.71.0 (cfd3bbd8f 2023-06-08)
```

## `wasm-pack`
次に`wasm-pack`をインストールする。
`wasm-pack`はRustで生成されたWebAssemblyをビルド、テスト、公開するためのツール。
[wasm-pack (rustwasm.github.io)](https://rustwasm.github.io/wasm-pack/installer/) からダウンロードしインストール。
```sh
$ wasm-pack --version
wasm-pack 0.12.1
```

## `cargo-generate`
次に`cargo-generate`をインストール。
`cargo-generate`は既存のGitリポジトリをテンプレとしてRustプロジェクトを作るためのツール。
以下のコマンドでインストール。
```sh
$ cargo install cargo-generate
```
インストールされていることを確認。
```sh
$ cargo-generate --version
cargo generate 0.19.0
```

## `npm`
最後に`npm`。
`npm`はJavaScriptのためのパッケージマネージャ。
今回はJavaScriptのバンドラーと開発用サーバを動かすためにインストールする。
[Getting started | npm Docs (npmjs.com)](https://docs.npmjs.com/getting-started)
```sh
$ npm -v
8.5.5
```
