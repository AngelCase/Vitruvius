セルのクリックで生死を切り替えたり、
ゲームの一時停止をできるようにする。

## ポーズと再開
`www/index.js`で次のような変更を加える：
- requestAnimationFrameを呼んだ時のidentifierを持っておき、cancelAnimationFrameをそのidentifierと呼ぶことでアニメを停止させられるようにする
・再生/停止ボタンがクリックされたらキューされたアニメフレームのidentifierがあるかを確認する。持っているならばゲームは進行中→アニメを停止させたい→`renderLoop`は二度と呼ばれない。持っていなければポーズ中→`requestAnimationFrame`をよぶことでゲームを再開させる。

変数`animationId`を`requestAnimationFrame`から返すidentifierとする。
キューされたアニメフレームが無ければ`null`をセットしておく。
```js
let animationId = null;

const renderLoop = () => {
  drawGrid();
  drawCells();

  universe.tick();

  // 戻り値を代入するように変更
  animationId = requestAnimationFrame(renderLoop);
};
```
これにより、以下のように`animationId`の値からゲームが停止されているかを判定できる。
```js
const isPaused = () => {
  return animationId === null;
};
```
以下のコードでボタン押下によるゲームの停止・再開を行う。
```js
const playPauseButton = document.getElementById("play-pause");

const play = () => {
  playPauseButton.textContent = "⏸";
  renderLoop();
};

const pause = () => {
  playPauseButton.textContent = "▶";
  cancelAnimationFrame(animationId);
  animationId = null;
};

playPauseButton.addEventListener("click", event => {
  if (isPaused()) {
    play();
  } else {
    pause();
  }
});
```
最後に、これまで`requestAnimationFrame(renderLoop)`を呼んでゲームを始めていた部分を`play()`に置き換えることで、ボタンのアイコンを適切に初期化する。

## `"click"`イベントによるセルの状態の切り替え
セルの生死の切り替えをできるようにする。

`src/lib.rs`の`Cell`に`toggle`メソッドを実装する。
```rust
impl Cell {
    fn toggle(&mut self) {
        *self = match *self {
            Cell::Dead => Cell::Alive,
            Cell::Alive => Cell::Dead,
        };
    }
}
```
次に、与えられた行と列のセルの状態を切り替えるために、
- 行と列をセルのベクトルのインデックスに変換し、
- セルの状態を切り替える`toggle`メソッドを呼ぶ。
```rust
#[wasm_bindgen]
impl Universe {
    // ...

    pub fn toggle_cell(&mut self, row: u32, column: u32) {
        let idx = self.get_index(row, column);
        self.cells[idx].toggle();
    }
}
```
そして`www/index.js`にて、
- `<camvas>`要素のクリックイベントをlisten、
- クリックイベントのページ由来の座標系をcamvas由来の座標系に変換、
- 行番号と列番号に変換し、
- `toggle_method`で状態を切り替え
- シーンを再描画する。
```rust
canvas.addEventListener("click", event => {
  const boundingRect = canvas.getBoundingClientRect();

  const scaleX = canvas.width / boundingRect.width;
  const scaleY = canvas.height / boundingRect.height;

  const canvasLeft = (event.clientX - boundingRect.left) * scaleX;
  const canvasTop = (event.clientY - boundingRect.top) * scaleY;

  const row = Math.min(Math.floor(canvasTop / (CELL_SIZE + 1)), height - 1);
  const col = Math.min(Math.floor(canvasLeft / (CELL_SIZE + 1)), width - 1);

  universe.toggle_cell(row, col);

  drawGrid();
  drawCells();
});
```
