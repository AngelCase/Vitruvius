配列を使った例：
```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {}", element);
    }
}
```

`Range`型を使った例：
```rust
fn main() {
    for number in (1..4).rev() {
        println!("{}!", number);
    }
    println!("LIFTOFF!!!");
}

```
出力：
```sh
3!  
2!  
1!  
LIFTOFF!!!
```
