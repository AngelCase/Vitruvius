## 前提条件
![[java_fruit.png]]
## `<? extends E>`
`<? extends E>`で`E`もしくは`E`を継承している型（サブクラス）であることを要求する。
![[java_extends.png]]
```java
List<? extends Fruit> basket = new ArrayList<Fruit>(); // 1
List<? extends Fruit> basket = new ArrayList<Melon>(); // 2
List<? extends Fruit> basket = new ArrayList<Lemon>(); // 3
```

## `<? super E>`
`<? extends E>`で`E`もしくは`E`のスーパークラスであることを要求する。
`extends`の逆。
![[java_super.png]]
```java
List<? super Melon> baskets = new ArrayList<Melon>(); // 1
List<? super Melon> baskets = new ArrayList<Fruit>(); // 2
List<? super Melon> baskets = new ArrayList<Food>(); // 3
List<? super Melon> baskets = new ArrayList<Object>(); // 4
```

## 参考
[[Java] ジェネリクス 境界ワイルドカード型のご紹介 - Qiita](https://qiita.com/ysn/items/66e225004bf656f012c7)
