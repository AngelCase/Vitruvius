## ポインタ先の値の変更不可
```c++
const int* a;
a = 1; // OK
*a = 1; // NG
```
## アドレスの変更不可
```c++
int* const a;
a = 1;	// NG
*a = 1;	// OK
```
## アドレスとポインタ先両方の値の変更不可
```c++
const int* const a;
a = 1;	// NG
*a = 1;	// NG
```
