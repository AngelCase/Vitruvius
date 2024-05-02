## 発生した問題
オブジェクト配列`options`の要素の値を変えてもwatchが動かない。
```ts
watch(options, () => {
	// some process
    })
```

## 原因
ネストされたオブジェクトを監視する際は`deep`オプションを`true`にしてやる必要がある。
```ts
  watch(options, () => {
	// some process
    },
    {
      deep: true
    }
  )
```

## 参考
[$watchでオブジェクトの変更を監視する方法 - Qiita](https://qiita.com/_Keitaro_/items/8e3f8448d1a0fe281648)