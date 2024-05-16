## 現象
`fs`を使おうとすると以下のようなエラーが出る。
```
Module not found: Can't resolve 'fs'
```

## 原因
`fs`はNode.jsのモジュールであり、
サーバーサイドでしか使えないため。
クライアントサイドJSで使おうとするとこのようなエラーになる。

## 参考
[Next.js でのエラー Module not found: Can't resolve 'fs' の対処方法 (gotohayato.com)](https://gotohayato.com/content/553/)
[Module not found: Can't resolve 'fs' · Issue #7755 · vercel/next.js · GitHub](https://github.com/vercel/next.js/issues/7755)
