[[Vue3_watch|watch]]よりシンプルに書ける。
```
watchEffect(() => {
	cocnsole.log('watch, ${watch.value}')
})
```
watchと違って、画面が読み込まれた時点で実行される。
また、古い値を取ってくることはできない。
