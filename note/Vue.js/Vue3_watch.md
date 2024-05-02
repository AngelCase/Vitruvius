```
const search = ref('')
watch(search, () => {
	console.log('watch: ${search.value}')
})
```
引数として現在の値、変更前の値をとることもできる。
```
watch(search, (newValue, prevValue) => {
	...
})
```
画面を開いた時点では実行されず、値が変更されたら実行される。