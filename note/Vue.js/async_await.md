[[Promise]]をわかりやすく書き換えたもの。

## async
関数にasyncを付けると、その関数はPromiseを返すようになる。
そのため、非同期実行される関数となる。
```js
async function name() {}
```

## await
awaitを付けると、Promiseが完了するのを待ってから次の処理に進む。
また、awaitはasyncの中でのみ使える。（そのうちトップレベルでも使えるようになる）
awaitは実質Promiseに対してthen()をしているため、Promiseのreturnする値が取得できる。
```js
const promise = new Promise(resolve => {
    setTimeout(resolve, 1000, 'foo');; // 中で resolve('foo') が呼ばれる
});

const value = await promise; // 1 秒待機

console.log(value);
```

## 参考
[Promise, async, await がやっていること (Promise と async は書き換え可能？) - Qiita](https://qiita.com/kerupani129/items/2619316d6ba0ccd7be6a#31-async-%E9%96%A2%E6%95%B0)