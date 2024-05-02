プリミティブ型をリアクティブにする。
```
setup() {
	let name = '大谷'
	const nameRef = ref('錦織')
}
```
この例の場合、nameは非リアクティブ、nameRefはリアクティブとなっている。
nameRefはリアクティブにするためにラップされているので、値を取り出したい場合は.valueを付ける：
```
console.log(nameRef.value)
```