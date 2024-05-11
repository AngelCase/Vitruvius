[[useEffect]]の
```jsx
useEffect(() => {
  scramble(); // 一度だけ実行したい処理
  // eslint-disable-next-line react-hooks/exhaustive-deps
}, []);
```

## 参考
[How to run useEffect only once (christopherklint.com)](https://www.christopherklint.com/blog/run-useeffect-only-once)
