引数の役割は2つあり、それぞれプロデューサーとコンシューマー。

## Producer
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

## Consumer
コンシューマーはAPIからオブジェクトを受け取る引数。`super`を使うべき。
```java
// このように実装しておくと
public <E> void popAll(Collection<? super E> dst) { ... }

// こう使える
Stack<Number> numberStack = new Stack<>();
Collection<Object> objectsHolder = ... ;

numberStack.popAll(objectsHolder);
```

## PECS
PECS、つまりプロデューサーextends、コンシューマーsuperで覚える。

ただし戻り値型に`extends`と`super`は使わないこと。
利用者側に余計な制約を強いてしまうため。

関連：[[Java_extends_super]]

## 参考
[Effective Java 第３版 「ほぼ全章」を「読みやすい日本語」で説明してみました。 - Qiita](https://qiita.com/nyandora/items/3e5ec76ca3881bc17924)
