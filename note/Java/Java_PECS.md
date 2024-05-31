引数の役割は2つあり、それぞれプロデューサーとコンシューマー。

## Producer extends
プロデューサーはAPIにオブジェクトを提供する引数。`extends`を使うべき。
例えば、`Number`のリストを消費できる関数なら、
`Integer`や`Double`のリストも消費できるべき。

以下の例ではリストの中身を画面出力している。
```java
public static void main(String[] args) {
    Collection<Integer> integers = Arrays.asList(1, 2, 3);
    Collection<Double> doubles = Arrays.asList(1.1, 2.2, 3.3);

    printNumbers(integers);
    printNumbers(doubles);
}

public static void printNumbers(Collection<? extends Number> collection) {
    for (Number number : collection) {
        System.out.println(number);
    }
}
```

## Consumer super
コンシューマーはAPIからオブジェクトを受け取る引数。`super`を使うべき。
```java
public static void main(String[] args) {
    List<Number> numbers = new ArrayList<>();
    List<Object> objects = new ArrayList<>();

    addNumbers(numbers);
    addNumbers(objects);

    System.out.println(numbers); // Outputs: [1, 2]
    System.out.println(objects); // Outputs: [1, 2]
}

public static void addNumbers(Collection<? super Integer> collection) {
    collection.add(1);
    collection.add(2);
}
```

逆に、`extends Object`だと要素を追加するところでエラーになる。
```java
collection.add(1); // エラー
```
エラーになる理由は具体的な型がわからないから。
`List<Number>`に`String`

## PECS
PECS、つまりプロデューサーextends、コンシューマーsuperで覚える。

ただし戻り値型に`extends`と`super`は使わないこと。
利用者側に余計な制約を強いてしまうため。

関連：[[Java_extends_super]]

## 参考
[Effective Java 第３版 「ほぼ全章」を「読みやすい日本語」で説明してみました。 - Qiita](https://qiita.com/nyandora/items/3e5ec76ca3881bc17924)
