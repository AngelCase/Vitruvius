[[Promise]]
[[async_await]]

## コードの比較
### Promiseの例
```js
// 非同期でAPIにリクエストを投げて値を取得する処理
function request1(): Promise<number> {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(1);
    }, 1000);
  });
}
 
// 受け取った値を別のAPIにリクエストを投げて値を取得する処理
function request2(result1: number): Promise<number> {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(result1 + 1);
    }, 1000);
  });
}
 
// 受け取った値を別のAPIにリクエストを投げて値を取得する処理
function request3(result2: number): Promise<number> {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(result2 + 2);
    }, 1000);
  });
}
 
request1()
  .then((result1) => {
    return request2(result1);
  })
  .then((result2) => {
    return request3(result2);
  })
  .then((result3) => {
    console.log(result3);
    // @log: 4
  });
```

### async_awaitの例
```js
// 非同期でAPIにリクエストを投げて値を取得する処理
function request1(): Promise<number> {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(1);
    }, 1000);
  });
}
 
// 受け取った値を別のAPIにリクエストを投げて値を取得する処理
function request2(result1: number): Promise<number> {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(result1 + 1);
    }, 1000);
  });
}
 
// 受け取った値を別のAPIにリクエストを投げて値を取得する処理
function request3(result2: number): Promise<number> {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(result2 + 2);
    }, 1000);
  });
}
 
async function main() {
  const result1 = await request1();
  const result2 = await request2(result1);
  const result3 = await request3(result2);
  console.log(result3);
  // @log: 4
}
 
main();
```

## 参考
[Promise / async / await | TypeScript入門『サバイバルTypeScript』 (typescriptbook.jp)](https://typescriptbook.jp/reference/promise-async-await#promise%E3%81%A8%E3%82%B8%E3%82%A7%E3%83%8D%E3%83%AA%E3%82%AF%E3%82%B9)