親から孫へデータを渡せる機能。

importとexportでつながっていれば、間のコンポーネントは無視してデータを渡せる。
![[Provide_Inject.PNG]]

親：
```
provide() {
	return {
		userName: '親で設定した値'
	}
}
```
孫：
```
{{ userName }}
...
inject: ['userName']
```