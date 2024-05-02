## 🤔 発生した問題
imgタグのsrc要素を静的に指定するとWebpackによりモジュールとして読み込まれる。
一方、src属性に`v-bind`を使い動的に指定するとそのままのパスとして扱われる。
この違いにより、何も考えずにsrcに変数を渡すと404エラーが発生してしまう。
```vue
<script setup>
    const fileName = 'img-1';
</script>

<template>
    <!-- 変数を展開せずにファイル名を指定すると表示される -->
    <img :src="~/assets/images/img-1.png" />

    <!-- 変数を展開しても画像ファイルが表示されない -->
    <img :src="`~/assets/images/${fileName}.png`" />
</template>
```

## 💡 解決法1
`import.meta.glob`を使う。
```vue
<script setup lang="ts">
import { filename } from 'pathe/utils';

const glob = import.meta.glob('~/assets/images/*.png', { eager: true });
const images = Object.fromEntries(
  Object.entries(glob).map(([key, value]) => [filename(key), value.default])
);

const dynamic_image_name = 'img-1';
</script>

<template>
  <img :src="images[dynamic_image_name]" alt="Discover Nuxt 3" />
</template>
```
`import.meta.glob`はVite.jsで提供されている関数みたいで、ファイルシステムからのモジュールのインポートをサポートしているらしい。
詳しくは参考リンクを参照のこと（[参考](https://vitejs.dev/guide/features.html#glob-import)）。

## 💡 解決法2
動的インポート`import()`を使う。
```vue
<script setup lang="ts">
  const imageName = 'img-1'

  let image = {}
  try {
    image = await import(`~/assets/img/${imageName}.png`)
  } catch (e) {
    console.log(e)
  }
</script>

<template>
  <img :src="image.default" />
</template>
```
注意として、Vite内で使っているRollupの都合上、`import()`の引数が「変数で始まってはいけない」とか、「拡張子で終わらなければいけない」とか制約が色々ある。
詳しくは参考リンクを参照のこと（[参考](https://github.com/rollup/plugins/tree/master/packages/dynamic-import-vars#limitations)）。

## Nuxt3で使えなかった解決法
`require()`を使う。
```vue
<script setup>
    const fileName = 'img-1';
</script>

<template>
    <!-- require()で囲ってあげる -->
    <img :src="require(`~/assets/images/${fileName}.png`)" />
</template>
```
これにより、モジュールとして読み込んでくれる。
だが、viteだと`require()`が使えないのでこの方法は採れなかった。

## 参考
[Vue.jsで画像のsrcに変数を使ったら画像が表示されない - Qiita](https://qiita.com/yusuke1209kitamura/items/fa92ab869ac4955b47d8)
[【Vue.js】imgタグのsrc要素は指定の仕方によって読み込み方が違う - Qiita](https://qiita.com/coppieee/items/4260bd0af246aebf5557)
[require is not defined · Issue #12797 · nuxt/nuxt · GitHub](https://github.com/nuxt/nuxt/issues/12797)
[Assets with dynamic names are not resolved · Issue #14766 · nuxt/nuxt · GitHub](https://github.com/nuxt/nuxt/issues/14766)
[Nuxt - Starter (forked) - StackBlitz](https://stackblitz.com/edit/github-pq8nym-aoavsh?file=app.vue)
[plugins/packages/dynamic-import-vars at master · rollup/plugins · GitHub](https://github.com/rollup/plugins/tree/master/packages/dynamic-import-vars#limitations)
[Features | Vite (vitejs.dev)](https://vitejs.dev/guide/features.html)