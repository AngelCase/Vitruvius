優先度付きキュー。
「優先度付きキュー」自体の説明はこちらを参照のこと：[[priority_queue]]
C++のstdとの違いとして、
Javaはデフォルトで昇順（正確には[[Java_自然順序付け|自然順序付け]]に従う）である点に注意。

## 主要なメソッド
- `offer(E e)`：要素の追加。`Queue`インタフェースによるもの。
- `add(E e)`：同じく要素の追加。`Collection`インタフェースによるもの。
- `poll()`：キューの先頭を取得し、削除する。
- `peek()`：キューの先頭を取得する。削除はしない。

## 使用例
```java
// リスト
var list = new ArrayList<Integer>(Arrays.asList(2, 5, 4, 3, 1));

System.out.println("natural list");
list.sort(Comparator.naturalOrder());
print(list);

System.out.println("reverse list");
list.sort(Comparator.reverseOrder());
print(list);

System.out.println();

// 優先度付きキュー
System.out.println("natural priority queue");
var que = new PriorityQueue<Integer>();
for (int num : list) {
	que.add(num);
}
print(que);

System.out.println("reverse priority queue");
var revQue = new PriorityQueue<Integer>(Comparator.reverseOrder());
for (int num : list) {
	revQue.add(num);
}
print(revQue);
```
出力：
```
natural list
12345
reverse list
54321

natural priority queue
12345
reverse priority queue
54321
```
なお、`print`は以下のような画面出力用の自作メソッドである。
```java
private void print(ArrayList<Integer> list) {
	for (int num : list) {
		System.out.print(num);
	}
	System.out.println();
}

private void print(PriorityQueue<Integer> que) {
	PriorityQueue<Integer> qCopy = que;
	while (!qCopy.isEmpty()) {
		System.out.print(qCopy.poll());
	}
	System.out.println();
}
```

## 参考
[PriorityQueue (Java Platform SE 8 ) (oracle.com)](https://docs.oracle.com/javase/jp/8/docs/api/java/util/PriorityQueue.html)
