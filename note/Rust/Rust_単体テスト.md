Rustではプロダクションコードとテストコードを同じソースコードの中に書ける。
以下に例を示す：
```rust
// プロダクションコード
struct Rectangle {
    width: u32,
    height: u32,
}
impl Rectangle {
    fn compare_area(&self, other: &Rectangle) -> bool {
        self.width * self.height > other.width * other.height
    }
}

fn double_value(a: i32) -> i32 {
    a * 2
}

// テストコード
#[cfg(test)]
// サブモジュールtestsを作る
mod tests {
    // superで親の全ての内容を指定し、
    // useでその内容を取り込む
    use super::*;

    #[test]
    fn test_a_is_larger() {
        let a = Rectangle {
            width: 5,
            height: 5,
        };
        let b = Rectangle {
            width: 3,
            height: 3,
        };

        assert!(a.compare_area(&b));
    }

    #[test]
    fn test_double() {
        assert_eq!(6, double_value(3))
    }
}
```
`#[cfg(test)]`の中は`cargo test`実行時のみコンパイルされる。
`mod`でサブモジュールを作っているため、プロダクションコードがあるのとは違う階層になっている。
そのため、`use super::*`で親の内容をすべて取り込んできている。

## `assert_eq!`と`assert_ne!`マクロ
`assert_eq!`と`assert_ne!`マクロの比較対象の値は`PartialEq`と`Debug`トレイトを実装している必要がある。
これは単純に、構造体やenum定義に`#[derive(PartialEq, Debug)]`という注釈をつけるだけで済む。

## カスタムの失敗メッセージ
`{}`プレースホルダーを含むフォーマット文字列を渡せば、
失敗メッセージとともにカスタムメッセージを表示できる。
```rust
#[test]
fn greeting_contains_name() {
	let result = greeting("Carol");
	assert!(
		result.contains("Carol"),
		//挨拶(greeting)は名前を含んでいません。その値は`{}`でした
		"Greeting did not contain name, value was `{}`",
		result
	);
}
```

## `should_panic`でパニックを確認する
`#[should_panic]`でパニックが起こることをテストできる。
```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    #[should_panic]
    fn greater_than_100() {
        Guess::new(200);
    }
}
```
ただし、このテストは不正確になりやすい。
なぜなら、意図しない箇所でのパニックでもテストが通ってしまうため。
そこで、以下のように`expected`引数を追加すれば、
失敗メッセージに指定のテキストが含まれていることを確かめられる。
```rust
impl Guess {
    pub fn new(value: i32) -> Guess {
        if value < 1 {
            panic!(
                //予想値は1以上でなければなりませんが、{}でした。
                "Guess value must be greater than or equal to 1, got {}.",
                value
            );
        } else if value > 100 {
            panic!(
                //予想値は100以下でなければなりませんが、{}でした。
                "Guess value must be less than or equal to 100, got {}.",
                value
            );
        }

        Guess { value }
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    //予想値は100以下でなければなりません
    #[should_panic(expected = "Guess value must be less than or equal to 100")]
    fn greater_than_100() {
        Guess::new(200);
    }
}
```
