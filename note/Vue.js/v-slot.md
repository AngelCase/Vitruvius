スロットに名前を付けて、1つのコンポーネント内で複数のスロットを扱えるようにする。
```
親：
<template v-slot:header>
	スロットコンテンツ
</template>
子：
<slot name="header">
	ここが置き換わる
</slot>
```

名前を指定しなかった場合"default"を指定したのと同じことになり、これはname属性を省いたslotタグを指す。

省略記法は#。

## スコープ付きスロット
slotタグにバインドされた値はスロットプロパティと呼ばれる。
親でv-slotの値として名前を指定することでスロットプロパティを受け取れる。
```
子：
<slot v-bind:user="user">
	{{ user.lastName }}
</slot>
親：
<template #default="slotProps">
	{{ slotProps.user.lastName }}
</template>
```
{{ slotProps.user.lastName }}の部分で、親が子のdataを使えている。
slotPropsの部分は自由に設定可能。