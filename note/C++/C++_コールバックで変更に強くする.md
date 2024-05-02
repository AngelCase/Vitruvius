[コールバック](https://qiita.com/tetsu0121/items/87bb7dd98a1cbb0a75aa)

## 概要
コールバックを利用することで、既存のコードを変更せずに処理を追加できる場合がある。
例えばHeroクラスは体力をメンバ変数として保持している：
```c++
class Hero
{
    uint32_t m_life = 100; // 耐久値（ダメージを受けると減り、回復すると増える）
};
```
これを増減させる際、以下のようにコールバックをとる関数を定義しておく：
```c++
//-------------------------------------------------------------------------
class Hero
{
public:
    void     Take_Attack( const std::function<void( uint32_t& )>& );
    void     Take_Heal  ( const std::function<void( uint32_t& )>& );

private:
    uint32_t m_life = 100;
};

//-------------------------------------------------------------------------
void Hero::Take_Attack( const std::function<void( uint32_t& )>& callback )
{
    callback( m_life );
    printf( "ダメージを受けた！現在の耐久力は %d です.\n", m_life );
}
```
利用する際は以下のようにコールバックを渡す：
```c++
void Attack_fromZako( uint32_t& hero_life )
{
    hero_life -= 10;
    printf( "ザコの攻撃！\n" );
}

int main()
{
    Hero hero;

    hero.Take_Attack( Attack_fromZako );

    return 0;
}
```
こうすればHeroにむやみにsetterを増やす必要がなくなる。

その他、ラムダ式を使えば`this`のキャプチャなどを行えるため、さらに強力。

## 感想
内容は理解できた。使いこなせると強そうな雰囲気は感じる。
だがどう活用していいか思いつかない。
