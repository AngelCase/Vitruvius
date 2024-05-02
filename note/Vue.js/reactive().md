オブジェクトをリアクティブにする。
```
const book = reactive({
	title: 'タイトル',
	author: ['大谷', '伊藤']
})
```
ただし、スプレッド構文を使うとリアクティブでなくなってしまう。
```
return {
	...bookRefs
}
```
その場合はtoRefs()を使えばよい。
```
return {
	...toRefs(bookRefs)
}
```