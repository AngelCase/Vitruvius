## コマンド
メソッド呼び出しをオブジェクトにするパターン。今まで直接メソッドを呼び出していた部分にオブジェクトを挟む。
例えばAボタンを押したらジャンプする場合、「ジャンプする」というコマンドをオブジェクトとして作る。他のアクションも同様。そしてこれらのオブジェクトは同じ基底クラスを継承する。
コマンドパターンを使わない単純な実装だと以下のようになる：
```c++
void InputHandler::handleInput()
{
	if(isPressed(BUTTON_X)) jump(); // ジャンプ
	else if (isPressed(BUTTON_Y)) fireGun(); // 発砲
	else if (isPressed(BUTTON_A)) swapWeapon(); // 武器の交換
	else if (isPressed(BUTTON_B)) lurchIneffectively(); // 意味もなくふらつく
}
```
コマンドパターンを使う場合、まず指示（コマンド）を表す基底クラスを次のように定義する：
```c++
class Command
{
public:
	virtual ~Command() {}
	virtual void execute() = 0;
};
}
```
ゲームでの各動作に対応するサブクラスを作成する：
```c++
class JumpCommand : public Command
{
public:
	virtual void execute() { jump(); }
};

// 以下、同様...
```
入力ハンドラの中では、各ボタンに対するコマンドオブジェクトへのポインタを格納する：
```c++
class InputHandler
{
public:
	void handleInput();

	// 以下、コマンドをボタンに割り当てるメソッドなど

private:
	Command* buttonX_;
	Command* buttonY_;
	Command* buttonA_;
	Command* buttonB_;
}
```
これで入力ハンドラの処理は、これらのオブジェクトに依頼するだけとなる：
```c++
void InputHandler::handleInput()
{
	if (isPressed(BUTTON_X)) buttonX_->execute();
	else if (isPressed(BUTTON_Y)) buttonY_->execute();
	else if (isPressed(BUTTON_A)) buttonA_->execute();
	else if (isPressed(BUTTON_B)) buttonB_->execute();
}
```
この方法のメリットは次の通り。
- キーコンフィグがやりやすくなる。オブジェクトを付け替えるだけなので。
- AI操作、デモ操作がやりやすくなる。
- 取り消し機能が作りやすくなる。コマンドクラスの中に実行機能とは逆の取り消し機能を入れるだけで良い。後は実行したコマンドをリストとして保持して、必要に応じて、そこから取り出せば良い。
注意すべき点として、ジャンプコマンドのように、純粋に動作を行うだけで、状態を持たないものの場合、すべてのインスタンスは等価なので2つ以上生成する事はメモリの無駄。その場合フライウェイトを使う。

## フライウェイト
同じパラメータを持つインスタンスがたくさんあるのはメモリの無駄である。パラメーターが同じ部分を1つのクラスでまとめて、それぞれのインスタンスは、そのオブジェクトへの参照を持つようにする。
利用例としてマップチップがある。マップチップを列挙型などで定義して2次元配列に格納するやり方がよくあるが、そのマップチップが「通れるのか」「水なのか」をswitch文で判定するのは不格好である。マップチップ自身に判断させる方が良い。とは言え、すべてのマップチップがインスタンスを持つ必要は無い。どうせ各マップチップは状態を持たないので、単一のインスタンスを参照すれば良い。

## オブザーバ
実績システムを何も考えずに実装すると、コードの至るところに実績システムが入り込んでしまう。そうではなく、サブジェクト(監視対象)にオブザーバを渡しておき、何かイベントが起こるたびに、それをオブザーバに通知してもらう。オブザーバは通知内容によってやりたい処理をする。
オブザーバは次のようなインタフェースで定義する：
```c++
class Observer
{
public:
	virtual ~Observer() {}
	virtual void onNotify(const Entity& entity, Event event) = -1;
};
```
このインタフェースを実装した具象クラスはすべてオブザーバになる。今回の例だと実績システム：
```c++
class Achievements : public Observer {
public:
	virtual void onNotify(const Entity& entity, Event event)
	{
		switch (event)
		{
		case EVENT_ENTITY_FELL:
			if (entity.isHero() && heroIsOnBridge_)
			{
				unlock(ACHIVEMENT_FELL_OFF_BRIDGE);
			}
			break;

			// 他のイベントの処理...
			// heroIsOnBridge_の更新...
		}
	}

private:
	void unlock(Achievement achivement)
	{
		// まだ与えられていない場合は達成バッジを与える...
	}

	bool heroIsOnBridge_;
}
```
監視対象はサブジェクトと呼ばれる。サブジェクトの仕事は2つあり、1つはオブザーバのリストを持っておくこと：
```c++
class Subject
{
private:
	// 多言語使用車への説明のために配列にしているが
	// 動的なコレクションでよい
	Observer* observers_[MAX_OBSERVERS]; 
	int numObservers_;
}
```
サブジェクトはそのリストを変更するためのAPIを公開する：
```c++
class Subject
{
public:
	void addObserver(Observer* observer)
	{
		// 配列に追加する...
	}

	void removeObserver(Observer* observer)
	{
		// 配列から削除する...
	}

	// その他の処理...
}
```
サブジェクトのもう1つの仕事は通知を送信すること：
```c++
class Subject
{
protected:
	void notify(const Entity& entity, Event event)
	{
		for (int i = 0; i < numObservers_; i++)
		{
			observers_[i]->onNotify(entity, event);
		}
	}

	// 以下、その他の処理...
}
```
一つの問題点として、サブジェクトとオブザーバが破棄された時に、登録解除を絶対にしなければいけないという点がある。より安全な登録解除は、オブザーバの基底クラスで登録解除処理を書いておく方法。そうすれば、オブザーバを作るたびに処理を書き忘れないか心配する必要がなくなる。
もう一つの問題点として、オブザーブとサブジェクトの処理のつながりが見えにくくなること。そのため、オブザーバーパターンを使うのは、ほとんど関係のない2つのことを結びつけるときにのみであり、通信の両側について考える必要がしばしばある場合使ってはいけない。
かつてのオブザーバパターンは、オブザーバをクラスとして表現していたが、今時はメンバ関数へのポインタをオブザーバとして登録できるようにしたほうが使いやすい。
(感想)オブザーバーが複数いるなら強いけど、一つだけだったらコードが冗長な感じがする。ただ、目的が抽象化なのであれば問題ないのかも。

## プロトタイプ
インスタンスをたくさん作る必要がある時、毎回新しく作るのではなく、1つプロトタイプを作っておき、それをコピーすることによって返す方法。例えば、モンスタースポナーを実装するとき、一体のモンスターだけ作っておき後はそれをクローンする。
ソースコードではなく、データに対してプロトタイプの考え方を用いることができる。
例えば、様々なゴブリンの敵キャラを作りたいとする。その時の問題点として、多くのパラメータが各ゴブリンにおいて重複してしまう。これを防ぐためにパラメーターとしてプロトタイプというものを設定しておく。もし存在しないパラメータがあった場合、プロトタイプを見に行くようにすれば、データの無駄を減らせる。

## シングルトン
シングルトンの機能は、インスタンスが1つしか存在しないと保証することと、グローバルなアクセスポイントを提供すること。
以下ではシングルトンの代替手段について述べる。
他のオブジェクトのお世話をするマネージャにシングルトンは使われがちだが、そもそもマネージャは必要ない。マネージャの機能はオブジェクトのヘルパーであることが多いので、それらの機能はオブジェクトに移動してしまえばいい。
インスタンスが1つしか存在しないことを保証したければ、コンストラクタ内で「インスタンス化済みフラグ」を trueにしてやれば良い。
グローバルなアクセスポイントを提供する代わりに、引数渡し、基底クラスからの取得、他のグローバルなデータからの取得、サービスロケータなどを検討する。
(感想)結局グローバルのアクセスポイントについての問題は解決してない気がする。結局グローバルになってるし、また別の問題を生みそうな気もする。

## サービスロケータ
グローバルにアクセスしたいサービスへの参照を持つ。
サービスロケータにどうやってサービスを登録するか、登録されてないサービスにアクセスが来た場合どうするかを考える必要がある。
シングルトンと似ており、問題点も据え置きなので乱用しすぎないこと。

## ステート
様々な状態に応じて異なる振る舞いをする場合、if文を増やすのではなく、状態ことにクラスを分けて、そのインスタンスを切り替えることで振る舞いを変える。
同時に複数の状態が存在する場合、例えば「地面にいる/空中にいる」と「銃を持っている/持っていない」のような状態が独立で存在する場合は、「地面にいて銃を持っている」のようにしてしまうと組み合わせ爆発が起きるので、それぞれ別のステートとして持つ。ただしそれぞれの状態が影響しあう場合はif文で他方の状態をチェックするようなことになってしまう。
また、それぞれの状態においてコードの重複がある場合、状態に階層構造を持たせれば良い。例えばジャンプは「立っている時」「歩いている時」「走っている時」など、様々な状態において可能である。そういった場合は、これらの状態に「地上にいる」という状態を継承させ、そこにジャンプのためのコードを書けば良い。

## ダブルバッファ
データの処理が完了し切るまでデータを参照されたくない場合、バッファを2つ用意しておき、処理完了までは古いものを参照させれば良い。例えばグラフィック表示にて、描画が終わる前にビデオドライバにバッファを参照されてしまうと中途半端な画面が見えてしまう、といった場合に使える。

## バイトコード
ソースコードの外で命令セットを用意し、外部から振る舞いを設定可能にする。MODの仕組みを提供する場合、直接ソースコードを触らせるより安全。

## サブクラスサンドボックス
基底クラスで、仮想メソッド一つとその仮想メソッドが使う用のユーティリティメソッドをいくつか提供する。例えば超能力をいろいろ作る場合、基底クラスを用意し、ユーティリティメソッドの中にに外部の物理システムやサウンドとのつながりを作る。そうすることで派生クラスは外部のシステムとつながらずに済む。
（感想）ただの継承な気がする。基底クラスに機能を置いているだけ。

## 型オブジェクト
継承によって型を決めるのではなく、オブジェクトを参照することによって型を決める。例えば、たくさんの種類のモンスターを作りたい場合、モンスタークラスを継承してたくさんのモンスターを作るのではなく、モンスタークラスから系統オブジェクトへの参照を持たせることで、そのモンスターがどの系統なのかを設定する。この系統オブジェクトによって、そのインスタンスがどの型なのかを示せるので「型オブジェクト」と呼ばれる。
![[型オブジェクトを使わない場合.JPEG]]
![[型オブジェクトを使う場合.JPEG]]
```c++
class Breed
{
public:
	Breed(int health, const char* attack)
	: health_(health),
	  attack_(attack)
	{}

	int getHealth() { return health_; }
	const char* getAttack() { return attack_; }

private:
	int health_; // 生命力の初期値
	const char* attack_;
};

class Monster
{
public:
	Monster(Breed& breed)
	: health(breed.getHealth()),
	  breed_(breed)
	{}

	const char* getAttack()
	{
		return breed_.getAttack();
	}

private:
	int health_; // 現在の生命力
	Breed& breed_;
};
```
このパターンのメリットは以下。
- 継承を使わなくて良いため柔軟な構造を持てる
- ソースコードの変更なしに型を追加・修正できる

## コンポーネント
単一のゲーム要素が複数のドメインを利用している時、各ドメインをそれぞれのコンポーネントクラスに置き、ゲーム要素は、コンポーネントを入れる単なる入れ物(コンテナ)になる。
コンポーネントパターン適用前のモノリシック（一枚岩）な例が以下：
```c++
class Bjorn
{
public:
	Bjorn() : velocity_(0), x_(0), y_(0) {}

	void update(World& world, Graphics& graphics);

private:
	static const int WALK_ACCELERATION = 1;

	int velocity_;
	int x_, y_;

	Volume volume_;

	Sprite spriteStand_;	
	Sprite spriteWalkLeft_;	
	Sprite spriteWalkRight_;	
};
```
`update()`は以下のようになっている：
```c++
void Bjorn::update(World& world, Graphics& graphics)
{
	// ユーザー入力を主人公の速度に反映させる。
	switch (Controller::getJoystickDirection())
	{
		case DIR_LEFT:
			velocity_ -= WALK_ACCELERATION;
			break;
		case DIR_RIGHT:
			velocity_ += WALK_ACCELERATION;
			break;
	}

	// 速度に従って位置を変更する
	x_ += velocity_;
	world.resolveCollision(volume_, x_, y_, velocity_);

	// 適切なスプライトを描画する
	Sprite* sprite = &spriteStand_;
	if (velocity_ < 0) sprite = &spriteWalkLeft_;
	else if (velocity_ > 0) sprite = &spriteWalkRight_;
	graphics.draw(*sprite, x_, y_);
}
```
ここからコンポーネントパターンを適用していく。
`Bjorn`から別のコンポーネントクラスに切り出す：
```c++
class PhysicsComponent
{
public:
	void update(Bjorn& bjorn, World& world)
	{
		bjorn.x += bjorn.velocity;
		world.resolveCollision(volume_,
			bjorn.x, bjorn.y, bjorn.velocity);
	}
private:
	Volume volume_;
};

class Bjorn
{
public:
	int velocity;
	int x, y;

	void update(World& world, Graphics& graphics)
	{
		input_.update(*this);
		physics_.update(*this, world);
		graphics_.update(*this, graphics);
	}

private:
	InputComponent input_;
	PhysicsComponent physics_;
	GraphicsComponent graphics_;
};
```
これを続けると`Bjorn`固有のコードはなくなるので、`Bjorn`を各コンポーネントを使用する一般的なクラスに作り替えることができる。
コンポーネント間の通信には以下の3つの方法を状況に応じて使い分ける。
- コンポーネントを持つコンテナオブジェクトの状態変数を変更する。これはすべてのオブジェクトが持っていて、当然の基本的な属性に使う。例えば位置など。
- コンポーネント同士が互いの参照を持つ。明らかに別物なのに、緊密に関係しているものに使う。
- メッセージを送る。「送りっぱなし」になるので、重要度が低い通信に使う。例えば物理シミュレーションが衝突を検知したら、サウンドコンポーネントに音を出すようにメッセージを送ると言った場合。
メッセージを送る実装について、まずすべてのコンポーネントが実装する基底インタフェースの`Component`を定義する：
```c++
class Component{
public:
	virtual ~Component() {}
	virtual void receive(int message) = 0;
}
```
次にメッセージを送るためのメソッドをコンテナオブジェクトに追加する：
```c++
class ContainerObject
{
public:
	void send(int message)
	{
		for (int i = 0; i < MAX_COMPONENTS; i++)
		{
			if (components_[i] != NULL)
			{
				components_[i]->receive(message);
			}
		}
	}

private:
	static const int MAX_COMPONENTS = 10;
	Component* components_[MAX_COMPONENTS];
}
```

## イベントキュー
誰かのメソッド呼び出し(リクエスト)をキューとして保存しておき、そのリクエストを処理するプログラムがキューから取り出して処理する。そうすることによって、単にソースコードを分離するだけでなく、時間的にも分離できる。もちろん、リクエストがちゃんと処理されるかは引き出し側次第のため、送り渡し側が応答を必要とする場合には向いていない。
オブザーバパターンの非同期版といえる。

## サービスロケータ
グローバルにアクセスしたいサービスへの参照を持つ。
サービスロケータにどうやってサービスを登録するか、登録されてないサービスにアクセスが来た場合どうするかを考える必要がある。
シングルトンと似ており、問題点も据え置きなので乱用しすぎないこと。

## データ局所化
CPUはRAMからデータをとってくる時、それに隣接するデータもまとめてキャッシュにとってくる。このことを考慮せずにプログラムを書くと、キャッシュミスが多くなり実行速度が低下する。
データを局所化することで、キャッシュの効果を高められる。効率が良いのは、実際の値が格納された配列。ポインタを使うとメモリ上を飛びまわるため、キャッシュミスが起こりやすくなるのでなるべく避ける。
その他、キャッシュ効率を上げるためのテクニックとして、ホット/コールド分離というものがある。頻繁に使われるデータはホット、めったに使わないデータはコールドとして扱い、コールドなデータはポインタにしておくことでキャッシュを圧迫せずに済む。

## ダーティフラグ
データが更新されていないことを示すダーティフラグを用意しておき、実際のデータが必要になったとき、ダーティフラグが偽なら過去の値をそのまま使い、真ならデータを計算し直す。

## オブジェクトプール
再利用可能なオブジェクトの集合を管理するプールクラスを定義し、それが使用中かどうかを返す。新しいオブジェクトが欲しければプールに依頼する。
何百というオブジェクト生成・破棄しても、メモリのフラグメンテーションが起きない。