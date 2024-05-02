## fpsタイマーの実装
JavaScriptで`performance.now`を使って計測するだけなので割愛。

## `console.time`による`universe::tick`の計測
`console.time`に対応する関数をRust側で呼び、
`universe:tick`にかかる時間を計測する。
参考：[[console.timeで経過時間を画面出力する]]

## さらにライフゲームを成長させる
ブラウザの開発者ツールをもとにボトルネックを探し、
パフォーマンスを向上させる。

## もっと早くする
nightlyコンパイラとcargo benchcmpが必要となるのでインストールする必要がある。
```sh
rustup install nightly
cargo install cargo-benchcmp
```

`benches/bench.rs`を作成し、以下のように記述することで
```rust
#![feature(test)]

extern crate test;
extern crate wasm_game_of_life;

#[bench]
fn universe_ticks(b: &mut test::Bencher) {
    let mut universe = wasm_game_of_life::Universe::new();

    b.iter(|| {
        universe.tick();
    });
}
```

`src/lib.rs`からは`#[wasm_bindgen]`や`js_sys`由来のコードをコメントアウトし、
`Cargo.toml`からは`cdylib`をコメントアウトする。

夜間ビルドを利用するために以下のように`cargo bench`を実行する：
```sh
cargo +nightly bench | tee before.txt
```
`tee`コマンドで出力を`before.txt`にも出力している。

Rustコードを改善した後、
出力先を`after.txt`に変更したうえでベンチマークをとり、
```sh
cargo +nightly bench | tee after.txt
```
改善前と後をbenchcmpで比較する。
```sh
$ cargo benchcmp before.txt after.txt 
 name            before.txt ns/iter  after.txt ns/iter  diff ns/iter   diff %  speedup
 universe_ticks  930,020             71,378                 -858,642  -92.33%  x 13.03
```
13.03xのスピードアップができたことがわかる。

WebAssemblyは一般的なハードウェアアーキテクチャに厳密に対応しているが、
実際にブラウザから確認しても高速化されていることは確認した方がよい。
