引数の役割は2つあり、それぞれプロデューサーとコンシューマー。

## Producer
プロデューサーはAPIにオブジェクトを提供する引数。`extends`を使うべき。
```java
// このように実装しておくと
public <E> void pushAll(Iterable<? extends E> src) { ... }

// こう使える
Stack<Number> numberStack = new Stack<>();
Iterable<Integer> integers = ... ;

numberStack.pushAll(integers);
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
