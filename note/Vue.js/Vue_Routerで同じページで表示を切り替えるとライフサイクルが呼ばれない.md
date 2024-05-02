例えば、以下のルーティングの例で、/book/1と/book/2を切り替えた際にはライフサイクルのcreated()などが呼ばれない。
```
path: '/book/:id'
component: BookDetail,
props: route => ([
	id: Number(route.params.id)
])
```

watchを使って解決できる。
```
watch: {
	$route(to, from) {
		// ページ切り替え時に行いたい処理
	}
}
