## 背景
JavaのAPIを読んでいると「自然順序付け」という単語が頻繁に出てくるので
自然順序付けについて調べたことをメモ。

## 自然順序付けとは
オブジェクトをソートする際、
オブジェクト同士の大小を比較し順序を決める必要があるが、
自作クラスだとコンパイラはそれをどう順序付けしてよいか判断できない。

例えば、以下のような`Chara`クラスをソートしたいとする。
```java
// 自然順序付けされていないキャラ
class Chara {
	final public int hp;
	final public String name;

	public Chara(int hp, String name) {
		this.hp = hp;
		this.name = name;
	}
}
```
`Chara`は体力`hp`と名前`name`を持つ。
これを「体力`hp`の小さい順に並び替えたい」とプログラマが考えていたとして、
どこにも書いてないのでコンパイラは順序付けのやり方を知りようがない。

そのような場合に、
例えば`Comparable`インタフェースを実装することで
そのクラスに全体順序付けを強制できる。
そしてこの順序付けを「自然順序付け」という。

## 例
ソートする際のやり方として、例えば以下の2つがある。
- `Comparator`でクラスの外から順序付けを決める
- `Comparable`によりクラス内で順序付けのやり方を決めておく（自然順序付け）
これらの例を見ていく。
### `Comparator`によるソート
`Comparator`は、`Arrays.sort`などのソートメソッドに渡すことでソート順を制御するのに使える。
まずは以下のように、`compare`をオーバーライドして順序を指定する。
```java
// HPを元に昇順にする
class HpOrder implements Comparator<Chara> {
	@Override
	public int compare(Chara c1, Chara c2) {
		return c1.hp - c2.hp;
	}
}
```
`compare`は以下のように値を返せばよい：
- 最初の引数が2番目の引数より小さい場合、負の整数
- 両方が等しい場合、0
- 最初の引数が2番目の引数より大きい場合、正の整数

その他にも細かい制約があるがドキュメントを参照のこと。

実際にソートする際は、`sort`の引数に`Comparator`を渡してやればよい。
```java
public static void main(String[] args) {
	// 初期化
	var charaList = new ArrayList<Chara>();
	charaList.add(new Chara(100, "hero"));
	charaList.add(new Chara(200, "knight"));
	charaList.add(new Chara(50, "monk"));
	charaList.forEach(c -> System.out.println(c.name + " : " + c.hp));
	// hero : 100
	// knight : 200
	// monk : 50

	// ソート
	charaList.sort(new HpOrder());
	charaList.forEach(c -> System.out.println(c.name + " : " + c.hp));
	// monk : 50
	// hero : 100
	// knight : 200
}
```

### `Comparable`によるクラスの自然順序付けでのソート
`Comparable`インタフェースを実装することで、
クラスに全体順序付けを強制できる。

今回は以下のように`Chara`をフィールドに持つ`HpOrderedChara`というクラスを作成する。
```java
// 自然順序が「体力が小さい順」のキャラ
class HpOrderedChara implements Comparable<HpOrderedChara> {
	final private Chara chara;

	public HpOrderedChara(Chara chara) {
		this.chara = chara;
	}

	@Override
	public int compareTo(HpOrderedChara hpOrderedChara) {
		return this.getHp() - hpOrderedChara.getHp();
	}

	public String getName() {
		return chara.name;
	}

	public int getHp() {
		return chara.hp;
	}
}
```
`HpOrderedChara`は`Comparable`を実装しており、
`compareTo`をオーバーライドする必要がある。

`compareTo`の値の返し方は`compare`とほとんど同じ：
- このオブジェクトが指定されたオブジェクトより小さい場合、負の整数
- 等しい場合、0
- このオブジェクトが指定されたオブジェクトより小さい場合、正の整数

その他細かい制約はドキュメントを参照のこと。

実際にソートする際は、`sort`の引数に`Comparator.naturalOrder`を渡してやればよい。
`Comparator.naturalOrder`は自然順序で`Comparable`オブジェクトを比較するコンパレータを返す。
```java
public static void main(String[] args) {
	var charaList = new ArrayList<Chara>();
	charaList.add(new Chara(100, "hero"));
	charaList.add(new Chara(200, "knight"));
	charaList.add(new Chara(50, "monk"));

	// charaListを元にhpOrderedCharaListを初期化
	var hpOrderedCharaList = new ArrayList<HpOrderedChara>();
	charaList.forEach(chara -> hpOrderedCharaList.add(new HpOrderedChara(chara)));
	hpOrderedCharaList.forEach(c -> System.out.println(c.getName() + " : " + c.getHp()));
	// hero : 100
	// knight : 200
	// monk : 50

	// ソート
	hpOrderedCharaList.sort(Comparator.naturalOrder());
	hpOrderedCharaList.forEach(c -> System.out.println(c.getName() + " : " + c.getHp()));
	// monk : 50
	// hero : 100
	// knight : 200
}
```
なお、今回は自然順序付けしているので
`sort`に`null`を渡すこともできる。
```java
hpOrderedCharaList.sort(null);
```
ドキュメントには以下のように書いてある。
>指定されたコンパレータが`null`の場合は、リストの全要素で[`Comparable`](https://docs.oracle.com/javase/jp/8/docs/api/java/lang/Comparable.html "java.lang内のインタフェース")インタフェースを実装し、要素の[自然順序](https://docs.oracle.com/javase/jp/8/docs/api/java/lang/Comparable.html "java.lang内のインタフェース")を使用する必要があります。

## 参考
[Comparable (Java Platform SE 8 ) (oracle.com)](https://docs.oracle.com/javase/jp/8/docs/api/java/lang/Comparable.html)
[Comparator (Java Platform SE 8 ) (oracle.com)](https://docs.oracle.com/javase/jp/8/docs/api/java/util/Comparator.html)
[ArrayList (Java Platform SE 8 ) (oracle.com)](https://docs.oracle.com/javase/jp/8/docs/api/java/util/ArrayList.html)
[java - what does arrayListName.sort(null) do? - Stack Overflow](https://stackoverflow.com/questions/49436291/what-does-arraylistname-sortnull-do)
