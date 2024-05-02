thisは書く場所によって役割が変わる。
```
console.log(this);
// windowオブジェクト

const obj = {
	test: function() {
		console.log(this);
	}
}
// オブジェクトの中にある時はそのオブジェクト

const objArrow = {
	test: () => {
	console.log(this)
	}
}
// windowオブジェクト
```
こうなる原因はJSのオブジェクトが階層構造になっていることによる。
![[JSのオブジェクトの階層構造.PNG]]