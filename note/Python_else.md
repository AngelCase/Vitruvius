## 概要
Pythonでは、`for`と`while`にも`else`をくっつけることができる。
`else`節は`for`や`while`の条件を満たさないときに実行される。
```python
nums = [1, 2, 3]
for num in nums:
    print(num)
else: 
    print('forループ終了')
```
出力：
```
1
2
3
forループ終了
```

`continue`だと条件文を通るので実行されるが、
`break`すると条件文を通らずループ終了するため実行されない。
```python
nums = [1, 2, 3]
for num in nums:
    print(num)
	break
else: 
    print('forループ終了')
```
出力：
```
1
```

## 使用例
「配列の中に特定の条件を満たすものがあったか？」を確認するコードが
簡単になる。
### elseを使わないループ
```python
arr = [1, 3, 9, 2, 1]
exist = False
for n in arr:
    if n == 9:
        exist = True
        break
if exist:
    print("Found! 9")
else:
    print("Not found 9...")
```

### elseを使うループ
```python
arr = [1, 3, 9, 2, 1]
for n in arr:
    if n == 9:
        print("Found! 9")
        break
else:
    print("Not found 9...")
```

## 参考
[8. 複合文 (compound statement) — Python 3.12.0 ドキュメント](https://docs.python.org/ja/3/reference/compound_stmts.html)
[while, forループのelse | Python Snippets (civic-apps.com)](https://python.civic-apps.com/else-loop/)
