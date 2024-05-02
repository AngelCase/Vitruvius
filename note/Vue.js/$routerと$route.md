スペルは似ているが全くの別物。
## $router
ページ遷移で使う。
```
router.push('home')
```

### ページ遷移時にパラメータを渡す
pathではなくnameで指定してやる必要がある。
渡す側：
```
router.push({ name: 'user', params: 
{ userId: '123' } })
```
受け取る側：
```
props: {
	userId: Number
}
...
再読み込みしても値を保持したい場合：
data() {
	return {
		bookIndex: this.$route.params.id
	}
}
```
router:
```
path: '/book/:id'
component: BookDetail,
props: route => ([
	id: Number(route.params.id)
])
```

## $route
現在表示されているページの情報。
