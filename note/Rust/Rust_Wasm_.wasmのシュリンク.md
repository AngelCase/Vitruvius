ビルド設定の微調整でサイズは小さくできる。

## Link Time Optimizations (LTO)
LLVMがインライン化や関数のプルーニングをする機会をさらに増やす。
`.wasm`の軽量化だけでなく実行速度も速くなるが、
コンパイルが長くなる。

設定するにはは`Cargo.toml`で以下を追加すればよい。
```toml
[profile.release]
lto = true
```

## 速度を犠牲にしてサイズを最適化する
設定するには`Cargo.toml`で以下を追加すればよい。
```toml
[profile.release]
opt-level = 's'
```
さらにサイズを最適化するには以下のようにする（速度はさらに遅くなる）。
```toml
[profile.release]
opt-level = 'z'
```
注意点として、
`opt-level = 's'`の方が`opt-level = 'z'`より軽量なバイナリになることがある。
そのため必ず計測すること。

LLVMにより生成された`.wasm`バイナリを後処理することで
コードサイズを15から20%節約し、
さらに多くの場合実行時間も高速化される。
```sh
# Optimize for size.
wasm-opt -Os -o output.wasm input.wasm

# Optimize aggressively for size.
wasm-opt -Oz -o output.wasm input.wasm

# Optimize for speed.
wasm-opt -O -o output.wasm input.wasm

# Optimize aggressively for speed.
wasm-opt -O3 -o output.wasm input.wasm
```
