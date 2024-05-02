![[methods_computed_watch.PNG]]
監視プロパティ。
computedと似ているが、dataが変わった時にコールバック関数を実行するので、より複雑な処理に向いている。

watchはキーとしてdataをとる。
```
data: {
	a: 1
},
watch: {
	a: {
		handler: function (val, oldVal) { /* ... */ },  
		deep: true
	}
}
```
deep: trueとつけることでオブジェクトのネストされた奥まで監視する。
そのほか、開始が始まった瞬間にコールバックを実行するimmediate: trueなどがある。