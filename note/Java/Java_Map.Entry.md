## Map.Entryとは
マップのエントリ（キーと値のペア）。

`Map.entrySet`メソッドを使うことで`Map.Entry`のコレクション・ビューが得られる。
このコレクション・ビューのイテレータからの取得以外に
マップ・エントリへの参照を手に入れる方法はない。

### 例
配列`nums`があるとき、出現頻度が高い値を`k`個返す関数。
```java
public int[] topKFrequent(int[] nums, int k) {
	// 出現回数を調べる
	var map = new HashMap<Integer, Integer>();
	for (final int n : nums) {
		Integer curFreq = map.getOrDefault(n, 0);
		map.put(n, curFreq + 1);
	}

	// 出現回数順に並べる
	Comparator<Map.Entry<Integer, Integer>> comp =
			(lhs, rhs) -> rhs.getValue() - lhs.getValue();
	var que = new PriorityQueue<Map.Entry<Integer, Integer>>(comp);
	for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
		que.add(entry);
	}

	// 多い順にk個取り出す
	int[] retArr = new int[k];
	for (int i = 0; i < k; i++) {
		retArr[i] = que.poll().getKey();
	}

	return retArr;
}
```

## Pairとの違い
### ライブラリ
`Pair`はjavafxライブラリにあり、
`Map.Entry`はjava.utilライブラリにある。
### クラスとインタフェース
`Pair`はクラスだが、`Map.Entry`はインタフェース。

## 参考
[What's the difference between Map.entry and Pair in Java? - Stack Overflow](https://stackoverflow.com/questions/71551963/whats-the-difference-between-map-entry-and-pair-in-java)
