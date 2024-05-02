[オブジェクトの隠ぺい](https://qiita.com/tetsu0121/items/324ffca2235184ac5670)

## 概要
あるクラスのメンバ関数を利用したいが、そのprivateなメンバ変数は隠したい、という内容。
以下の例だとService()は使いたいがm_secretStrは隠したい：
```c++
class Module_A_System
{
public:
    void Service()
    {
        printf( "%s\n", secretStr.c_str() );
    }

private:
    const std::string m_secretStr = "call A_System::Service\n";
};
```
方法としては、このModule_A_Systemをラッパークラスで包んでやる、という方法。
詳細は割愛。

## 感想
いまいち理解できておらずラムダ式を使うのが必須なのかよくわからない。いらないのでは？
クラスの利用者の負担が増えすぎている気がするのであまり心惹かれない。
ラムダ式を使うことを強要するのは中々きついと思う。