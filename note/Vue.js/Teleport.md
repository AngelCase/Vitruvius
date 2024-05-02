親子関係を飛び越えて表示できる機能。
![[Vue3_teleport.PNG]]
特にモーダルウィンドウの実装などで役立つ。

## 書き方
teleportコンポーネントでtargetを指定すると、targetで指定された要素が置き換えられる：
```
<teleport target="セレクタ">
	<MyModal />
</teleport>
```