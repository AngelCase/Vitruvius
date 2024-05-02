```
<transition name="fade" mode="out-in">
	<router-view />
</transiiton>
```
mode="out-in"がないと、遷移先のページと遷移前のページが同時に表示されてしまい、変な見た目になる。