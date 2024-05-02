## 発生した問題
クラス指定のCSSが効かない。
```html
<nav class="nav">
...
</nav>
```

```css
<style lang="scss" module>
  .nav {
    background-color: #333;
  }
</style>
```
クラスではなくタグを直接指定するとCSSが効く。

## 原因
HTML側の書き方を間違えていた。
- `class`ではなく`:class`
- クラス名の前に`$style.`が必要
CSS Modulesは書き方が特殊なので注意が必要だ。
```css
<style lang="scss" module>
  .nav {
    background-color: #333;
  }
</style>
```