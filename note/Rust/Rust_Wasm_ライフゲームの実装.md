## ライフゲーム
詳細は割愛。
[Implementing Life - Rust and WebAssembly (rustwasm.github.io)](https://rustwasm.github.io/docs/book/game-of-life/implementing.html)
[Conway's Game of Life - Wikipedia](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life)
## 設計
### 無限の宇宙
ライフゲームは無限の宇宙で動くが、無限のメモリや計算資源などない。
以下の3を選択する。
1. 宇宙のサブセットを追跡し、必要に応じて領域を広げるやり方。これだと拡張するほどにどんどん遅くなってメモリが枯渇する。
2. 固定サイズの宇宙にする。これだとグライダーなどが途中で消えてしまう。
3. 固定サイズの宇宙で、端と端をくっつける。これならグライダーが無限に飛び続けられる。

### RustとJavaScriptのインターフェース
この部分がこのチュートリアルで最も重要。

JavaScriptのガベージコレクションのヒープ（`Object`や`Array`、DOMノードが割り当てられる場所）はRustの値が存在するWebAssemblyの線形メモリ空間とはまったく異なっている。
WebAssemblyは執筆時点jだとガベージコレクションのヒープに直接アクセスできない。
一方JavaScriptはWebAssemblyの線形メモリ空間を読み書きできるが、スカラー値の`ArrayBuffer`（`u8`、`i32`、`f64`など）として扱うことになる。
WebAssemblyの関数は同様にスカラー値を取り、返す。
これらはすべてWebAssemblyとJavaScript通信を構成する構成要素である。

`wasm_bindgen`は境界を越えて複合構造を扱う方法を定義する。
例えば、Rustの構造をボックス化したり、
JavaScriptのクラス内のポインタをラップしたり、
RustからJavaScriptのオブジェクトへのテーブルにインデックスを作成したりできる。
`wasm_bindgen`は便利だが、データ表現について何も考えなくてよくなるわけではなく、
何の値や構造がこの境界を行き来するのかを考える必要がある。
インターフェースをデザインするためのツールとして考えたほうがいい。

インターフェースをデザインするにあたり、次の事柄を最適化したい。
1. WebAssemblyの線形メモリからのコピー、線形メモリへのコピーを最小にする。
2. シリアライズとデシリアライズを最小化する。コピーに似ているが、シリアライズ、デシリアライズどちらもオーバーヘッドが発生し、コピーも同様に発生する。もしデータ構造への不透明なハンドルを渡せたら、大きなオーバーヘッドを削減できる（つまり、片方でシリアライズして、WebAsssembly線形メモリの既知の場所にコピーし、もう片方でデシリアライズする）。`wasm_bindgen`はJavaScriptの`Object`、またはボックス化されたRust構造への不透明なハンドルを定義してくれる。

経験則によると、良いJavaScriptとWebAssembly間のインターフェースデザインとは、
巨大で寿命が長いデータ構造がWebAssemblyの線形メモリでRust型として実装されていて、
JavaScriptに不透明なハンドルとして公開されているものである。
JavaScriptは、不透明なハンドルをとる公開されたWebAssembly関数を呼び、
それらのデータを変更し、重い処理し、データをクエリし、小さくてコピー可能な結果を返す。
小さい結果を返すことにより、
JavaScriptのガベージコレクションのヒープとWebAssemblyの線形メモリの間を
コピーやシリアライズして行き来させることを回避できる。

## RustとJavaScriptのライフゲームのインタフェース
セルの状態は配列に保持して、`0`と`1`で死んでいるか生きているかの情報を保持する。
各インデックスの値を取りだす際は以下のようにする。
```
index(row, column, universe) = row * width(universe) + column
```
JavaScriptにセルを渡す際は、
`Universe`に`std::fmt::Display`を実装し、
文字列として渡す設計にする。

章の終わりでは`<canvas>`にレンダリングする。

## 実装
最終的な実装は[このリポジトリ](https://github.com/rustwasm/wasm_game_of_life/blob/master/src/lib.rs)。
Rustを更新したら`wasm-pack build`でビルドし、
`rpm run start`で開発サーバを立ち上げておくのを忘れないように。

### 実装で使われているJavaScriptの関連技術
[[requestAnimationFrameでリフレッシュレートに合わせて処理する]]
