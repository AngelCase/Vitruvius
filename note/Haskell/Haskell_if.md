Haskellの`if`は、`else`が必須である。
```haskell
doubleSmallNumber x = if x > 100
                        then x
                        else x*2
```
その理由は、Haskellではすべての式と関数は何かを返す必要があり、
`if`は式であるため。

例えば、`doubleSmallNumber`の結果に1を足したければ、以下のように書ける：
```haskell
doubleSmallNumber' x = (if x > 100 then x else x*2) + 1
```
（なお、`'`はHaskellの文法で何かあるわけではなく、ただ関数名に使えるだけである）
