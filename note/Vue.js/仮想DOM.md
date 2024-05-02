## DOM
Document Object Modelの略
HTMLをプログラムから操作できる仕組み

以下のようなツリー構造をとる（DOMツリー）

![[DOMツリー.png]]

### メリット
-  直接DOM操作すると管理がカオスになるが、仮想DOMだとメンテが早い
	-  どの箇所にどのDOMが当たっているのかがわかりやすい
-  処理が早い
![[仮想DOMの更新.PNG]]
- 見た目（DOM）とデータ（JSONなど）を分離できる

### 例
直接DOM操作をする場合：
```
<a id="google_link">googleへのリンク</a>
```
JSがつながっているのかがぱっと見で分からない。

仮想DOMの場合：
```
<a :href="google">googleへのリンク</a>
```
"google"というものがくっついているのがわかる。