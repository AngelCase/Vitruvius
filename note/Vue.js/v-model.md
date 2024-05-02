双方向データバインディングを実現する。
```
<input type="text" v-model="yourName">
```

## 様々な修飾子
### .lazy
入力フォームから離れた時初めてデータを更新する：
```
<input v-model.lazy="msg">
```

### .number
ユーザの入力を自動的に数字に型変換する：
```
<input v-model.number="age" type="number">
```

### .trim
ユーザ入力から空白を自動的に取り除く：
```
<input v-model.trim="email">
```