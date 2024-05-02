## 基本
Haskellのリストは同じ型のいくつかの要素を格納できる。
GHCIで名前を適宜する際、`let`キーワードを使える。
```haskell
ghci> let lostNumbers = [2,8,1,99,10]
ghci> lostNumbers 
[2,8,1,99,10]
```

## 文字列は文字のリスト
文字列は文字のリストである。
つまり`"hello"`は`['h','e','l','l','o']`のシンタックスシュガーである。

## リストの結合
`++`でリストをくっつけることができる。
```haskell
ghci> [1,2,3] ++ [4,5,6]
[1,2,3,4,5,6]
ghci> "hello" ++ "world"
"helloworld"
```
注意点として、Haskellは`++`は左側のリストをすべて調べる。
500万要素くらいの大きさのリストを`++`でくっつけると時間がかかる。

## リストの先頭への要素の追加
なお、先頭に要素を追加する`:`であれば即時に完了する。
```haskell
ghci> 'A':" SMALL CAT"
"A SMALL CAT"
ghci> 5:[1,2,3,4,5]
[5,1,2,3,4,5]
```

ちなみに`[1,2,3]`は`1:2:3:[]`のシンタックスシュガーである（`[]`はからのリスト）。

## リストの要素の取り出し
リストの要素をインデックスで指定して取り出す場合、`!!`を使用する。
インデックスは0から始まる。
```haskell
ghci> "Steve Buscemi" !! 6
'B'
ghci> [9.4,33.2,96.2,11.2,23.25] !! 1
33.2
```

## リストの中にリスト
リストの中にリストを入れられる。
長さは違ってもいいが、型が違ってはならない。
```haskell
ghci> [[1,2,3],[4]]
[[1,2,3],[4]]
ghci> [[1,2,3],[]] 
[[1,2,3],[]]
ghci> [[1,2,3],['a']]
<interactive>:11:3: error:
...
```

## リストの比較
詳細：[[Haskell_リストの比較]]
個人的にめちゃくちゃ引っかかったポイント。
Haskellのリストの比較は、辞書式順序で比較する。
先頭要素から見ていき、もしそれが同じだったら2番目の要素が比較される。
```haskell
ghci> [1,2,0,100] > [1,2,100,0]
False
```
参考：[Comparing lists in Haskell, or more specifically what is lexicographical order? - Stack Overflow](https://stackoverflow.com/questions/3651144/comparing-lists-in-haskell-or-more-specifically-what-is-lexicographical-order)

## リストを扱う関数
`head`はリストをとり、その先頭を返す（基本最初の要素）。
```haskell
ghci> head [5,4,3,2,1]
5 
```
`tail`はリストを取り、先頭以外を返す。
```haskell
ghci> tail [5,4,3,2,1]
[4,3,2,1] 
```
`last`はリストをとり、最後の要素を返す。
```haskell
ghci> last [5,4,3,2,1]
1 
```
`init`はリストを取り、末尾以外を返す。
```haskell
ghci> init [5,4,3,2,1]
[5,4,3,2]
```

なお、これらの関数は空のリストに使うと例外が発生するので注意。
```haskell
ghci> head []
*** Exception: Prelude.head: empty list
```

`length`はリストを取り、その長さを返す。
```haskell
ghci> length [5,4,3,2,1]
5
```
`null`はリストが空かをチェックする。
空なら`True`、そうでなければ`False`を返す。
```haskell
ghci> null [1,2,3]
False
ghci> null []
True
```
`reverse`はリストを逆順にする。
```haskell
ghci> reverse [5,4,3,2,1]
[1,2,3,4,5]
```

`take`は数とリストをとり、先頭からその数の要素を取り出す。
リストより多くの要素を要求するとリストそのものを返し、
0個の要素を要求すると空のリストを返す。
```haskell
ghci> take 3 [5,4,3,2,1]
[5,4,3]
ghci> take 1 [3,9,3]
[3]
ghci> take 5 [1,2]
[1,2]
ghci> take 0 [6,6,6]
[]
```
`drop`も似たように動く。
リストの先頭から指定した数の要素をドロップする。
```haskell
ghci> drop 3 [8,4,2,1,5,6]
[1,5,6]
ghci> drop 0 [1,2,3,4]
[1,2,3,4]
ghci> drop 100 [1,2,3,4]
[]
```
`maximum`は順序のあるリストをとり、その最大の要素を返す。
`minimum`は最小の要素を返す。
```haskell
ghci> minimum [8,4,2,1,5,6]
1
ghci> maximum [1,9,2,3,4]
9
```
`sum`はリストの数の合計を返す。
`product`はリストの積を返す。
```haskell
ghci> sum [5,2,1,6,3,2,5,7]
31
ghci> product [6,2,1,2]
24
ghci> product [1,2,5,6,7,9,2,0]
0 
```
`elem`は何かの値とリストを取り、
その値がリストにあるかを返す。
大抵infix functionとして呼ばれる。その方が読みやすいから。
```haskell
ghci> 4 `elem` [3,4,5,6]
True
ghci> 10 `elem` [3,4,5,6]
False
```
