idは#、classは.をつける。
:hoverで「マウスを乗せた時」というルールを作れる。
```
.content:hover {
...
}
```

CSSファイルを読み込むにはlinkタグを使う：
```
<link rel="stylesheet" href="css/app.css">
```

## BEM
セレクタの役割をBlock、Element、Modifierの3つの概念に分けて考える設計思想。
```
block {}
block__element {}
block_modifier {}
block__element_modifier {}
```
実際の例：
```
.navi {}
.navi__button {}
.navi__button_red {}
```
modifierはアンダーバー1つではなくハイフン2つでつなぐmindBEMdingという書き方もある。

## Sass/Scss
従来のCSSだと変数やインポートが使えなかったため複雑になってしまっていた。
Sass/Scssでは変数、インポート、ネスト（&）などを使える。

BEMなどとの相性がいい。
BEMだと、modifierだけが違うセレクタを書こうとすると、何度も同じBEを書く必要がある：
```
.navi__button_red {}
.navi__button_blue {}
.navi__button_black {}
```
&を使うと楽になる：
```
.block {
	&_element1 {
	}
	&_element2 {
		&_modifier1 {
		}
		&_modifier2 {
		}
	}
}
```

それぞれの違いは、Sassはプログラミングのように書け、Scssは従来のCSSのように書ける。