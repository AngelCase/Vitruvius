Vue.js 3.2からの機能。
[[SingleFileComponent|SFC]]内でCompositionAPIをシンプルに書ける。
setupを使う際に\<script setup\>と書くだけで、export defaultやreturnを書かなくてよくなる。

## メリット
- ランタイムパフォーマンス向上
- IDEパフォーマンス向上
- これまでのscriptとも併用可能
- propsとemit定義時に純粋なTS構文が使える

## 書き方
今までは上からtemplate→script→styleだったが、script setupでは一番上をscriptにすることが推奨されている。

### props：親から子へ
[[setup()]]を使わなくなったので、引数が取れない。
そこで、definePropsを使う。
```
<script setup>
	const props = defineProps({
		title: String
	})
...
```

## emit：子から親へ
こちらも同様にdefineEmitsを使う。
ただし、配列で指定する必要があるので注意。
```
const emitTest = defineEmits(['custom-event'])
...
<button @click="emitTest('custom-event', '子からの値')">
	Emitテスト
</button>
```
