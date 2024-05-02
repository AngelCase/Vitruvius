Vue.jsのコンポーネントにおいて、子から親にデータを渡す方法。
親のほうでカスタムイベントを用意しておいて、それを子が実行する、といった形式。
親：
```
<emit-test @custom-event="parentMethod"></emit-test>
...
methods: {
	parentMethod() {
	}
}
```
子：
```
let emitTest = {
...
	methods: {
		childeMethod() {
			this.$emit('custom-event', '値')
		}
	}
}
```