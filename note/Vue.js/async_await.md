[[Promise]]をわかりやすく書き換えたもの。

## async
関数に`async`を付けると、その関数は`Promise`を返すようになる。
明示的に何も返してなくても`Promise`オブジェクトを返す。
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

### 注意：正しく実装しないと`await`をつけても非同期になる
以下の例だと、`f1()`の中の`executeAfterWait()`の実行は待ってくれない。
```ts
async function executeAfterWait(x, ms) {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log(x);
      resolve();
    }, ms);
  })
}

async function f1(x, ms) {
    // NOTE: ここにawaitをつけていないのでfuga→hogeの順で表示される
	executeAfterWait(x, ms)
}

async function main() {
  await f1("hoge", 1000);
  console.log("fuga");
}

main();
```
実行結果
```
"fuga"
"hoge"
```
`f1()`は`undefined`を`Promise`で包んで返しており、
`await`はその解決を待ってしまっている。

## 参考
[Promise, async, await がやっていること (Promise と async は書き換え可能？) - Qiita](https://qiita.com/kerupani129/items/2619316d6ba0ccd7be6a#31-async-%E9%96%A2%E6%95%B0)