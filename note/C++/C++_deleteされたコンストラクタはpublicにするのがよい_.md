言語仕様とかではなく、あくまで個人の意見ベース。
以下のようなケースの違いについて。
```cpp
// publicにする場合
class A
{
public:
    A() = default;
    A(const A&) = delete;
};

// privateにする場合
class A
{
public:
    A() = default;

private:
    A(const A&) = delete;
};
```
これらのケースのうち、publicにするケースが推奨される。
その理由は、エラー文章がより明快になるため。
例えば、以下のようなprivateなメンバにアクセスしようとしたケースを考える。
```cpp
class A
{
public:
    A() = default;
private:
    A(const A&) = delete;
};

int main()
{
    A a;
    A a2=a;
}
```
この場合のGCC 4.8のエラー文は以下のようになり冗長である。
```cpp
main.cpp: In function 'int main()':
main.cpp:6:5: error: 'A::A(const A&)' is private
     A(const A&) = delete;
     ^
main.cpp:12:10: error: within this context
     A a2=a;
          ^
```

## 参考
[c++ - Must a deleted constructor be private? - Stack Overflow](https://stackoverflow.com/questions/18931133/must-a-deleted-constructor-be-private)
