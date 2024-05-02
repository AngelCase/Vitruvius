イベント発火時のJSの実行が可能になる。
クリックイベントを扱うならclick：
```
<button v-on:click="btnClicked">クリックしてね</button>
...
methods: {
	btnClicked() {
		console.log('clicked')
	}
}
```

その他にも様々なイベントを扱える。
例えば送信ボタンがクリックされたことはsubmitで検知できる。

省略記法は@。

イベントの取得は以下の通り：
```
methods: {
	btnClicked(e) {
		console.log(e)
	}
}
```
引数の取得は以下の通り：
```
<button v-on:click="btnClicked(1)">クリックしてね</button>
...
methods: {
	btnClicked(i) {
		console.log(i)
	}
}
```
イベントと引数の同時取得は以下の通り：
```
<button v-on:click="btnClicked(1, $event)">クリックしてね</button>
...
btnClicked(i, e) {
	console.log('clicked' + i)
	console.log(e)
}
```

## 修飾子
ajax通信を行う際、画面更新が起こるのを防ぎたい。
JSの場合、イベントに対してpreventDefault()をつける：
```
const prevent = document.getElementById('prevent')
prevent.addEventListener('click', (e) => {
	e.preventDefault();
})
```
Vue.jsの場合、修飾子として.preventをv-onにつける：
```
<button v-on.prevent="doThis"></button>
```
divの中にdivが入っている場合などで、内側のdivのイベントだけを発火させたいのに、外側のdivのイベントも発火してしまう現象がある（バブリング）。
![[バブリング.PNG]]
Vue.jsでは修飾子.stopをつける。
修飾子は同時に使える：
```
<button v-on.stop.prevent="doThis"></button>
```