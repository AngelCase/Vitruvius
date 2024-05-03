rangeを使えばリストの並んでいる要素をまとめて記述できる。
```haskell
ghci> [1..20]
[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20]
```
アルファベットにも使える。
```haskell
ghci> ['a'..'z']
"abcdefghijklmnopqrstuvwxyz"
ghci> ['K'..'Z']
"KLMNOPQRSTUVWXYZ"
```
rangeはステップも指定できるのがステキ。
```haskell
ghci> [2,4..20]
[2,4,6,8,10,12,14,16,18,20]
ghci> [3,6..20]
[3,6,9,12,15,18]
```
ただし、そこまで賢くはない。
例えば2のべき乗は作れない。あくまで最初のステップだけ指定できる。

数字が下がる場合はステップを明示的に示す必要がある
（そうしないと空のリストが返ってくる）。
```haskell
ghci> [5,3..1]
[5,3,1]
ghci> [5..2]  
[]
```

浮動点少数はうまく扱えないので使わない方がいい。
```haskell
ghci> [0.1, 0.3 .. 1]
[0.1,0.3,0.5,0.7,0.8999999999999999,1.0999999999999999]
```

上限を指定しなければ無限のリストも作れる。
例えば、13の倍数が24個欲しければ`[13,26..24*13]`とすることもできるが、
`[13,26]`とすればよい。
なぜなら、Haskellは怠惰なので実際にそれを要求するまで評価しないから。

`cycle`はリストを受け取り、それを無限にサイクルさせたリストを返す。
画面出力しようとすると無限に続くので`take`でスライスしている。
（`take`はリストの先頭から指定した数だけ取り出す）
```haskell
ghci> take 10 (cycle [1,2,3])
[1,2,3,1,2,3,1,2,3,1]
ghci> take 12 (cycle "LOL ")
"LOL LOL LOL "
```

`repeat`は要素を受け取り、その要素が無限に続くリストを返す。
```haskell
ghci> take 10 (repeat 5)
[5,5,5,5,5,5,5,5,5,5]
```

`replicate`を使えばよりシンプルに同じ要素が続くリストを作れる。
```haskell
ghci> replicate 3 10
[10,10,10]
```