![[methods_computed_watch.PNG]]

算出プロパティ。
computedはdataが更新されたタイミングで再実行される。
逆に、値が更新されない限りは何度呼出しても再実行されずキャッシュが帰ってくる。
必ずreturnをつけて値を返す必要がある。
```
{{ totalPrice }}
...
data() {
	return {
		price: 1,
		number: 1
	}
},
computed: {
	totalPrice() {
		return this.number * this.price
	}
}
```

アロー関数でも書ける：
```
totalPrice: app => app.number * app.price
```
thisを使わない点に注意（参考：[[JSのthis]]）。
