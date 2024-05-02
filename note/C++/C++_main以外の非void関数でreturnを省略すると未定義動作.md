## 概要
C++ではnon-void関数でreturnを省略できてしまう。ただし未定義動作。
```cpp
int func()
{
    int a=10;
    //do something with 'a'
    //oops no return statement
}


int main()
{
     int p=func();
     //using p is dangerous now
     //return statement is optional here 
}
```

## 参考
[関数内でreturnを省略することで発生しうる未定義バグ - Qiita](https://qiita.com/pshiko/items/835907b89c08a185e5f6)
[g++ - Omitting return statement in C++ - Stack Overflow](https://stackoverflow.com/questions/3402178/omitting-return-statement-in-c)