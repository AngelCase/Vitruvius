プログラミングをしている時、
コードを書いている時間よりも画面をスクロールしている時間の方が長い。
つまりコードを読みやすくすると言う事は、書きやすくすることを意味する。

意味のない単語を変数名につけないこと。
`info`も`data`も似たような意味だから、変数名についていても中身を区別できず、意味がない。
また、`customer`と`customerObject`も違いが分からないので意味がない。

メンバープレフィックスをやめる。`m_`のような。もはやエディタの強調機能で判別できる。
(感想)これどうなんだろう？まだ色んなとこでメンバープレフィックスは使われている気がする。

コンストラクターがオーバーロードされている場合は、
staticなファクトリーメソッドを用意するほうが好ましい。
つまり、一般には以下が望ましい。
```java
Complex fulcrumPoint = Complex.FromRealNumber(20.0);
```
以下は好ましくない。
```java
Complex fulcrumPoint = new Complex(23.0);
```

1つのコンセプトには、1つの単語を使う事。
fetch、get、retrieveを同じ意味で使っていると混乱を招く。
整合性を持った単語集を用意しておくと便利。

関数の引数はできるだけ少ないほうがいい。4つ以上は多すぎる。
ただし、`Point p = new Point (0, 0)`のように座標としてxとyを取るのは自然。この場合、2つの引数は1つの値を構成するコンポーネントだから。

関数に booleanを渡すのは良くない。そうなると、その数は2つのことをしていることになる。
真のときに1つ、偽のときに1つ。関数は1つのことだけをやるべきである。

関数名に引数の名前を変更する方法がある。
例えば、`assertEqualsはassertExpectedEqualsActual(expected, actual)`とすれば
引数の順番を覚えておかなくて済む。

ソースコードを書式化するために、
ある関数が別の関数を呼び出しているのであれば、
それは垂直方向に近い位置に置くべきである。
また可能な限り呼び出し側を呼び出される側の上に置くべき。
そうすることで、自然な流れで読める。
(感想) これ実践したい。確かに読みやすくなりそう。

複数の例外を出すメソッドを使う際にはcatchが大量に並ぶが、
それを止めたい場合ラッピングしてしまうのが良い。
これは特にサードパーティーのライブラリを使う際、ライブラリを変更しやすくなるので有用である。

1行ごとにnullチェックをするようなコードを書くのではなく、
メソッドをラップして代わりに例外を創出するか、
代わりとなるオブジェクトで常にオブジェクトを返すようにする。
例えば、以下のようなnullチェックをしているコードを考える：
```java
List<Employee> employees = getEmployees();
if (employees != null) {
	for (Employee e : employees) {
		totalPay += e.getPay();
	}
}
```
そうではなく、`getEmployees()`が空のオブジェクトを返すようにすれば、コードは以下のようにきれいになる：
```java
List<Employee> employees = getEmployees();
for (Employee e : employees) {
	totalPay += e.getPay();
}
```
`getEmployees()`の実装は以下のようになる：
```java
public List<Employee> getEmployees() {
	if(もしも従業員オブジェクトが存在しないなら)
		return Collections.emptyList();
}
```

テストコードで大事なのは読みやすさ。多くのことを可能な限り少ない表現で言い表す必要がある。

(発見)for文をもつ関数はparseArgumentCharactersと複数形で、
for文内で呼ばれる関数はparseArgumentCharacterと単数系になっている。
なるほど、こういう命名をするのかと参考になった。
```java
private void parseArgumentCharacters(String argChars) {
	for (int i = 0; i < argChars.length(); i++)
		parseArgumentCharacter(argChars.charAt(i));
}
```

関数では1つのことを行うべき。
例えば、以下のコードでは3つのことをおこなっている。
1. すべての社員に対しループを行う
2. それぞれの社員に支払いをすべきか確認する
3. 支払いを行う
```java
public void pay() {
	for (Employee e : employees) {
		if (e.isPayday()) {
			Money pay = e.calculatePay();
			e.deliverPay(pay);
		}
	}
}
```
以下のように、1つのことだけを行う関数に分割できる。
```java
public void pay() {
	for (Employee e : employees)
		payIfNecessary(e);
}

private void payIfNecessary(Employee e)) {
	if (e.isPayday())
		calculateAndDeliverPay(e);
}

private void calculateAndDeliverPay(Employee e) {
	Money pay = e.calculatePay();
	e.deliverPay(pay);
}
```
