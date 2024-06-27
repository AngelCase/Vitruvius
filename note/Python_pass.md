`pass`は何もしない。
```python
for i in range(3):
    print(i)
    if i == 1:
        pass
        print('PASS')
```
実行結果：
```
0
1
PASS
2
```
何かを記述する必要があるが、何も実行したくない場合に使う。
```python
def empty_func():
    pass
```

## 参考
[7. 単純文 (simple statement) — Python 3.12.4 ドキュメント](https://docs.python.org/ja/3/reference/simple_stmts.html#pass)
[Pythonのpass文の意味と使い方 | note.nkmk.me](https://note.nkmk.me/python-pass-usage/)
