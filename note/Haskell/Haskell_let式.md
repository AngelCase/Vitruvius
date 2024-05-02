## 概要
`where`は変数を束縛しガードを含む関数全体で見れるようにする構文だった。
`let`はきわめてローカルで、ガードをまたぐことはない。

以下はシリンダーの表面積を求める関数である：
```haskell
cylinder :: (RealFloat a) => a -> a -> a
cylinder r h =
    let sideArea = 2 * pi * r * h
        topArea = pi * r ^2
    in  sideArea + 2 * topArea
```

このように、`let <束縛> in <式>`のような形式をとる。
`let`で定義した名前は`in`の後の式からアクセスできる。

## whereとの違い
`let`はそれ自体が式であるが、
`where`は節である。

例えば、`if`は式なのでどこでも使える。
```haskell
ghci> [if 5 > 3 then "Woo" else "Boo", if 'a' > 'b' then "Foo" else "Bar"]
["Woo", "Bar"]
ghci> 4 * (if 10 > 5 then 10 else 0) + 2
42
```
同じことが`let`でもできる。
```haskell
ghci> 4 * (let a = 9 in a + 1) + 2
42
```
ローカルスコープで関数を作るのにも使える。
```haskell
ghci> [let square x = x * x in (square 5, square 3, square 2)]
[(25,9,4)]
```

また、`let`のスコープは狭く、
ガードをまたぐことができない。

## 複数束縛する
複数の変数の束縛をする際はセミコロンを使う。
最後の束縛の後にはつけてもつけなくてもよい。
```haskell
ghci> (let a = 100; b = 200; c = 300 in a*b*c, let foo="Hey "; bar = "there!" in foo ++ bar)
(6000000,"Hey there!")
```

## パターンマッチを使える
`let`束縛でパターンマッチができる。
```haskell
ghci> (let (a,b,c) = (1,2,3) in a+b+c) * 100
600
```

## リスト内包表記の中でも使える
リスト内包表記の中でも使える。
```haskell
calcBmis :: (RealFloat a) => [(a, a)] -> [a]
calcBmis xs = [bmi | (w, h) <- xs, let bmi = w / h ^ 2]
```

`let`はリストをフィルタせず、名前を束縛するだけである。
そのため、BMIが肥満の人だけを取り出す場合以下のように書ける。
```haskell
calcBmis :: (RealFloat a) => [(a, a)] -> [a]
calcBmis xs = [bmi | (w, h) <- xs, let bmi = w / h ^ 2, bmi >= 25.0]
```
`bmi`を`(w, h) <- xs`の中で使うことはできない（`let`束縛に先立って定義されているため）。

リスト内包表記の場合名前のスコープがすでに定義されているので
今回は`in`を省略しているが、
`in`を使うとそのスコープを`in`の中だけにできる。
```haskell
func1 :: [Int] -> [Int]
func1 xs = [x | x <- xs, let y = x * 2 in y > 3]
```
そのため、以下のように出力関数で`y`を使おうとするとエラーになる。
```haskell
func2 :: [Int] -> [Int]
func2 xs = [y | x <- xs, let y = x * 2 in y > 3]
```

GHCiの中で`in`を省略すると、
その名前のスコープは対話セッションが続く限り続く。
```haskell
ghci> let zoot x y z = x * y + z
ghci> zoot 3 9 2
29
ghci> let boot x y z = x * y + z in boot 3 4 2
14
ghci> boot
<interactive>:1:0: Not in scope: `boot'
```
