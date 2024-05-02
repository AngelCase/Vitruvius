## panic時のログ出力
`wasm-pack-template`には、
デフォルトで`console_error_panic_hook`クレートの依存関係が入っている。
その設定は`src/utils.rs`でされている。

この機能を使うには、
以下のように初期化関数などの中で初期化関数を呼び出し、
フックのインストールを行う必要がある。
```rust
pub fn new() -> Universe {
    utils::set_panic_hook();

    // ...
}
```

## ログ出力
`console.log`関数へのアクセスは`web-sys`クレートから行える。
まず、`Cargo.toml`にて`web-sys`を依存関係として追加し、
`"console"`機能を有効にする。
```toml
[dependencies]

# ...

[dependencies.web-sys]
version = "0.3"
features = [
  "console",
]
```
今回は、簡単のために`println!`スタイルのマクロとして`console.log`関数をラップする。
```rust
extern crate web_sys;

// A macro to provide `println!(..)`-style syntax for `console.log` logging.
macro_rules! log {
    ( $( $t:tt )* ) => {
        web_sys::console::log_1(&format!( $( $t )* ).into());
    }
}
```

```rust
log!(
	"cell[{}, {}] is initially {:?} and has {} live neighbors",
	row,
	col,
	cell,
	live_neighbors
);
```

## デバッガーでtickごとに停止させる
JavaScriptの`debugger;`文を使うことで、ブラウザ上でステップ実行できる。
```rust
const renderLoop = () => {
  debugger;
  universe.tick();

  drawGrid();
  drawCells();

  requestAnimationFrame(renderLoop);
};
```
