```js
if(!!obj){

}
```
否定の論理演算子を2つ重ねたもの。
「だったらtrueはtrueになるだけだから意味なくない?」と感じるが、実は意味がある。

objがundefinedの場合、普通はfalseとして扱われるが、undefinedキーワードに未対応のブラウザが存在している。
その場合だと、undefinedがboolean型に変換されず、if文がエラーになってしまう可能性がある。
否定演算子は対象のオブジェクトの否定をboolean型で返すため、それを利用してif文で正しく判定できるようにしている。

## 参考
[【JavaScript】!!（ビックリマーク2つ）って何？ | 衣食住よりプログラミング (senews.jp)](https://senews.jp/bikkuri-2/)