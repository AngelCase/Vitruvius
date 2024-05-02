## インストール
以下のサイトを参考にインストールを進める。
[Install Rust - Rust Programming Language (rust-lang.org)](https://www.rust-lang.org/tools/install)
64bitを選んで実行。
デフォルトインストールかカスタムインストールかを聞かれるが、デフォルトを選ぶ。
インストールが終わったら以下のコマンドで`rust-lesson`という名前のパッケージを作る。
```bash
cargo new rust-lesson
```

## VSCode設定
以下の拡張機能を導入する。
- rust-analyzer
  - RustのためのLanguage Server Protocolの実装
  - RUSTという拡張機能が推奨されていたがdeprecatedとなっていたので、replace先のrust-analyzerを導入
- CodeLLDB
  - C++やRustのためのデバッガ
以下をsettings.jsonに記述することで、ファイル保存時の自動フォーマットが行われる：  ``
```json
  "[rust]": {
    "editor.defaultFormatter": "rust-lang.rust-analyzer",
    "editor.formatOnSave": true
  }
``` 

## Hello, world!
初期化時点では以下のようなコードがある。
```rust
fn main() {
    println!("Hello, world!");
}
```
`!`はマクロ呼び出し。
以下のコマンドでビルドと実行を行う。
```bash
cargo run
```

## その他のコマンド
- `cargo build`
  - `target`ディレクトリ内にビルド結果が生成される
- `cargo check`
  - ビルドせず、コンパイルによるコードのチェックを行う
