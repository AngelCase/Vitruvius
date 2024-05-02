Vue.jsではtransitionタグでトランジション表現ができる。
![[Vue_transition.PNG]]
v-enterからv-leaveまでの6つのクラスが付いたり消えたりするので、それぞれにcssでスタイルをつけてやることで実現可能。
CSS：
```
.fade{
	&-enter{
			opacity: 0;
		&-to{
			opacity: 1;
		}
		&-active{
			transition: 1s;
		}
	}
	&-leave{
			opacity: 1;
		&-to{
			opacity: 0;
		}
		&-active{
			transition: 1s;
		}
	}
}
```
HTML：
```
<transition name="fade">
	<div v-show="isShow">表示されています</div>
</transition>
```
transitionタグの中には複数のタグを置けない。
複数タグを使いたい場合は、それらをdivタグで囲ってやればよい。

v-forのような複数要素を扱う場合はtransition-groupタグが使える。
また、transition-groupを使う場合、属性nameで指定した名前に-moveをくっつけたクラスが使えるようになり、これによって要素が動く時の設定ができる。