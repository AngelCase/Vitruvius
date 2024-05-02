Vue.jsのコンポーネントで親から子に値を渡す方法。
HTMLの属性のように値を設定できる：
```
<my-component title="テスト"></my-component>
...
props: {
	title: {
		type: String
	}
}
```
JSの仕様上、[[JSにおける値渡しと参照渡し|オブジェクトと配列は参照渡しされる]]ため、子でpropsの値を変えたら親にも影響するため注意。
これを避けるために、default設定をするなら関数形式で記述する：
```
props: {
	item: {
		type: Object
		default () => ({ count: 0 })
	}
}
```