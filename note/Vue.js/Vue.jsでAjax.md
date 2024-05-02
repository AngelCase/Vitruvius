[[fetch_API|fetch]]を使ってGETリクエストを送る。
```
methods: {
	async getUrl() {
		const response = await fetch(url, options)
		.then( response => response.json() )
	}
}
```

頻繁にサーバーと通信をすると負荷がかかってしまうため、1秒から3秒程度間隔を開けて実行するのが良い。
Lodash（JSの便利ライブラリ）を使って設定すると楽。
```
_.debounce(fn, 1000) // 1000ms待ってからfnを実行
_.throttlel(fn,1000) // fn実行後1000msは実行しない
```

dataに何か変化が起きるたびに非同期通信をしたい場合、watchで監視するという方法がある。
```
data() {
	return {
		watchTest: ''
	}
}
watch: {
	watchTest() {
		this.watchGetUrl()
	}
}
created() {
	this.watchGetUrl = _.debounce(this.getUrl, 1000)
}
```
この例では、createdで関数をwatchGetUrlオブジェクトに入れておき、watchTestが変更されるたびにwatchでその関数を実行している。