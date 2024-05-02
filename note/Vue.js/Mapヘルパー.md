アクションするだけのメソッドをアクションと同じmethodsとして展開する。
これによって繰り返し書く必要がなくなる。

Vuexのそれぞれの機能に対してヘルパーが存在する：
![[Mapヘルパー.PNG]]

スプレッド構文で書く。
```
methods: {
	// この書き方で
	...mapActions(['incrementAction'])
	// 以下と等価
	incrementAction() {
		this.$store.dispatch('incrementAction')
	}
}
```
引数を与えたい場合、別のmethodsから呼び出してやる必要がある：
```
addCount() {
	this.addCountAction({
		value: 10
	})
}
```
