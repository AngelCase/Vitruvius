## requestAnimationFrameとは
`requestAnimationFrame()`を使うことで、ウェブサイトでのアニメーション表現を行える。

`setInterval()`と似ている。
ただし、`setInterval()`はモニタのリフレッシュレートを考慮しないのでガタついて見える可能性があるのに対し、
`requestAnimationFrame()`にはリフレッシュレートに合わせて実行されるという特徴があり、
より滑らかに見える。
![[requestAnimationFrame_setInterval.png]]

## 構文
コールバックを引数にとり、
それを再描画の前に実行する。
```js
requestAnimationFrame(callback)
```
このコールバックにてHTMLの属性値を連続的に変更するなどして
アニメーションが実現できる。

## 使用時の注意
`requestAnimationFrame()`を一度呼んだだけだと、コールバックは一度しか実行されない。
連続してコールバックを実行したい場合、
コールバックの中でさらに`requestAnimationFrame()`を呼び出す必要がある。

また、モニタのリフレッシュレートが高いと、実行速度が速くなる点にも注意。
「2秒間要素を動かす」みたいなことをやりたい場合、
コールバックの第一引数に[DOMHighResTimeStamp](https://developer.mozilla.org/ja/docs/Web/API/DOMHighResTimeStamp)が渡されるので
それを利用すること。

## 例
以下の例では、ある要素を2秒間右に移動させている。
```js
const element = document.getElementById("some-element-you-want-to-animate");
let start, previousTimeStamp;
let done = false;

function step(timestamp) {
  if (start === undefined) {
    start = timestamp;
  }
  const elapsed = timestamp - start;

  if (previousTimeStamp !== timestamp) {
    // ここで Math.min() を使用して、要素がちょうど 200px で止まるようにします。
    const count = Math.min(0.1 * elapsed, 200);
    element.style.transform = `translateX(${count}px)`;
    if (count === 200) done = true;
  }

  if (elapsed < 2000) {
    // Stop the animation after 2 seconds
    previousTimeStamp = timestamp;
    if (!done) {
      window.requestAnimationFrame(step);
    }
  }
}

window.requestAnimationFrame(step);
```

## アニメーションを停止させたいとき
`cancelAnimationFrame()`を使う。
`requestAnimationFrame()`の戻り値を引数にとり、
スケジュールされたアニメーションフレームリクエストをキャンセルする。
```js
const requestAnimationFrame =
  window.requestAnimationFrame ||
  window.mozRequestAnimationFrame ||
  window.webkitRequestAnimationFrame ||
  window.msRequestAnimationFrame;

const cancelAnimationFrame =
  window.cancelAnimationFrame || window.mozCancelAnimationFrame;

const start = Date.now();

let myReq;

function step(timestamp) {
  const progress = timestamp - start;
  d.style.left = `${Math.min(progress / 10, 200)}px`;
  if (progress < 2000) {
    // requestAnimationFrame を呼び出すたびに requestId を更新することが重要です
    myReq = requestAnimationFrame(step);
  }
}
myReq = requestAnimationFrame(step);
// キャンセル処理は、最後の requestId を使用します
cancelAnimationFrame(myReq);
```
コールバックのリクエストIDは最新のものを使う必要がある点に注意。

## 参考
[Window.requestAnimationFrame() - Web API | MDN (mozilla.org)](https://developer.mozilla.org/ja/docs/Web/API/window/requestAnimationFrame)
[window.cancelAnimationFrame() - Web API | MDN (mozilla.org)](https://developer.mozilla.org/ja/docs/Web/API/Window/cancelAnimationFrame)
[requestAnimationFrame の使い方（アニメーション） (webdesignleaves.com)](https://www.webdesignleaves.com/pr/jquery/requestAnimationFrame.html)
[Canvasだけじゃない！requestAnimationFrameを使ったアニメーション表現 - ICS MEDIA](https://ics.media/entry/210414/)
