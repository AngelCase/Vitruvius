配列を操作するJSのメソッド。
配列の要素を追加・置換・削除できる。

2の位置から1つ取り除いてtrumpetを挿入：
```
const myFish = ['angel', 'clown', 'drum', 'sturgeon'];
const removed = myFish.splice(2, 1, 'trumpet');

// myFish は ["angel", "clown", "trumpet", "sturgeon"]
// removed は ["drum"]

```