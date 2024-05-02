本章では、ある時点の宇宙の次の状態が期待通りであることのテストをするが、
詳細は割愛し
Rustにより生成されたWebAssemblyの関数のテストを行う上で重要な点のみを記す。


今回、テスト用に借用された参照を返す関数を作るが、
`#[wasm_bindgen]`属性は付けない。
```rust
impl Universe {
    pub fn get_cells(&self) -> &[Cell] {
		// 省略
    }
}
```
なぜなら、Rustで生成されたWebAssembly関数は
借用された参照を返すことができないから。

テストコードは、 `#[wasm_bindgen_test]`を付ける以外は普通のテストコードと同じ。
```rust
#[wasm_bindgen_test]
pub fn test_tick() {
	// 省略
}
```
テストは`tests/web.rs`に書く。

以下のコマンドでテスト実行。
```sh
$ wasm-pack test --chrome --headless
```

## テスト実行時のエラー
[[Rust_Wasm_ライフゲームのテスト_エラー]]
