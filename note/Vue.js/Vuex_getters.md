[[computed]]相当。
computed内で呼ばれることが多い。
```
computed: {
	visibleUsers() {
		return this.$store.getters.visibleUsers
	}
}
```
store内：
```
getters: {
	visibleUsers: state => state.users.filter( user => user.isVisible )
}
```