# JavaScript まとめ5

## クラス

クラスはオブジェクトの設計図。  
料理でいうとクラスはレシピにあたります。出来上がった料理はオブジェクト。
レシピがあると簡単に料理が作れます。

### 基本的な書き方

```
// class自体はレシピなので、このままでは料理はできません
class クラス名 {
    // この中に色々定義する
}

// 料理を作るには

// 以下、同じ意味です
// インスタンス化するには
// オブジェクトを生成するには

// new をつけて生成します

const obj = new クラス名();

// この obj が料理にあたります
```
コンストラクター は、class によって生成されるオブジェクトの生成・初期化を行う特別なメソッドです  
プロパティはクラスのメソッドの中で定義しなければなりません
```
class Person {
    // constructor → コンストラクター
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
}

// 引数がコンストラクターに渡される
const taro = new Person("山田太郎", 62);

console.log(taro);

console.log(taro.name);
console.log(taro.age);
```
メソッドを定義
```
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    // メソッドを定義
    greet() {
        return `こんにちは！私は${this.name}です`;
    }
}

const taro = new Person("山田太郎", 62);
console.log(taro.greet());
```
同じクラスの他のメソッドも利用
```
class Person {
    constructor(name, age, sex, address, want) {
        this.name = name;
        this.age = age;
        this.sex = sex;
        this.address = address;
        this.want = want
    }

    greet() {
        return `こんにちは！私は${this.name}です`;
    }

    nowWant() {
        if (this.want) {
            return "現在恋人募集中です！";
        }
        return "";
    }

    introduce() {
        // 同じクラスの他のメソッドも利用
        return `${this.greet()} ${this.address}に住んでいる${this.age}歳の${this.sex}です。${this.nowWant()}`;
    }
}

// 同じクラスから色々なオブジェクトを作成

const taro = new Person("山田太郎", 33, "男性", "札幌市", true);
console.log(taro.introduce());

const asuka = new Person("森山あすか", 23, "女性", "広島市", false);
console.log(asuka.introduce());
```

### ゲッター、セッターメソッド

#### ゲッター、セッターが何故あるのか？それはJavaScriptに必要なのか？

クラス型のプログラミング言語で昔から有名なのがJavaです。 
そのJavaで沢山利用されてきたゲッター、セッターですから、JavaScriptにも持ってきたという感じがします。  
クラス型のプログラミングはオブジェクト指向と呼んだります。  
「オブジェクト指向」とは何かを教科書通り説明するには時間がかかりますし、人によって見解も違い一種の宗教論争みたいなところもありますので深入りしませんが、「カプセル化」というキーワードが重要なことは理解してください。  
カプセル化とは、クラスのプロパティを外部から直接操作させないことです。  
そうすることで安全なプログラムができる。  
そのためには、プライベートなプロパティにしなければならないのですが、現時点のJavaScriptはプライベートなプロパティが持てません。  
ということは、「カプセル化」できないわけです。  
将来のJavaScriptは、プライベートなプロパティが持てるように検討されていますが、各ブラウザで実装したりしなければならないのでもう少し時間がかかりそうです。

さて、ゲッター、セッターメソッドですが、これはプロパティを読出したり、更新したりするメソッドですが、`()`をつけない形で読んだり書いたりしますので、普通のプロパティのように見えますが、実はメソッドというものです。
```
class Person {
    constructor(name, age) {
        // アンダースコアをつけてプライベートなプロパティだと信じる
        this._name = name;
        this._age = age;
    }

    // _nameのゲッターを定義
    get name() {
        return this._name;
    }

    // _nameのセッターを定義
    set name(v) {
        this._name = v;
    }

    // _ageのゲッターを定義
    get age() {
        return this._age;
    }

    // _ageのセッターを定義
    set age(v) {
        this._age = v;
    }

}

const taro = new Person("山田太郎", 42);

// _nameにアクセス
console.log(taro.name);
// _nameを書き換え
taro.name = "スズキイチロー"
// 確認
console.log(taro.name);

// _ageにアクセス
console.log(taro.age);
// _ageを書き換え
taro.age = 51
// 確認
console.log(taro.age);

// こうやると直接プロパティに触れるが、紳士協定でやってはいけない
taro._name = "みのもんた"
taro._age = 75
console.log(taro._name);
console.log(taro._age);
```

### 静的メソッド

静的メソッドの特徴は、インスタンスを生成しなくても利用できるところです。
static キーワードを使います。静的メソッドは、クラスのインスタンス化なしで呼ばれ、インスタンス化されていると呼べません。静的メソッドは、アプリケーションのユーテリティ関数を作るのによく使います。

```
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    static speak() {
        return "みなさん、こんにちは";
    }
}

// const taro = new Person("山田太郎", 42);
// ↓　これはエラーになるので、taro を作る必要がない
// console.log(taro.speak());

console.log(Person.speak());
```
ユーテリティ関数とは？「役に立つもの」「有用性」「効用」「公益」などの意味

クラスは基本的にはオブジェクトを作りそれを利用しますが、静的メソッドを持つクラスは、便利な関数の寄せ集めという感じで使うという意味です。
```
class Calc {
    static add(a, b) {
        return a + b;
    }

    static sub(a, b) {
        return a - b;
    }

    static mul(a, b) {
        return a * b;
    }

    static div(a, b) {
        return a / b;
    }
}

console.log(Calc.add(1, 2));
console.log(Calc.mul(1000, 0.8));
```

## 継承

通常のクラスです。特に必要がなければconstructorはいりません。
```
class Greeting {
    say() {
        return "Hello";
    }
}

const obj = new Greeting();
console.log(obj.say());
```
違うクラスを作って、sayメソッドを使いたい場合
```
class Greeting {
    say() {
        return "Hello";
    }
}

const obj = new Greeting();
console.log(obj.say());

class English {
    say() {
        return "Hello";
    }
}

const obj2 = new English();
console.log(obj2.say());
```
上のように書くと長いので、こう書けます
```
class Greeting {
    say() {
        return "Hello";
    }
}

const obj = new Greeting();
console.log(obj.say());

class English extends Greeting {
}

const obj2 = new English();
console.log(obj2.say());
```
extends Greeting としてクラスを継承しますと、Greetingクラスのメソッドが利用できます。

JavaScriptの初めから入っているStringオブジェクトがありますが、そういうクラスも継承できます。
```
class MyClass extends String {
}

const obj = new MyClass("hello world!");
console.log(obj.toUpperCase());//toUpperCaseはStringオブジェクトにあるメソッド
```

## オブジェクトを配列に入れて、ループしてメソッドを実行する

メソッド名は同じですが、オブジェクトにより振る舞い、実行結果が違うことを確認してください。  
ここは重要なので100%理解して欲しいです。
```
// 親クラス
class Mammals {
    constructor(name) {
        this.name = name;
    }

    species() {
        console.log("哺乳類です");
    }

    speak() {
        console.log("話せます");
    }

    walk() {
        console.log("歩けます");
    }

    run() {
        console.log("走れます");
    }

    swim() {
        console.log("泳ぐこともできます");
    }

    introduction() {
        console.log(`私は${this.name}です。`);
    }
}

// 以下Mammalsの小クラス

class Man extends Mammals {
    constructor(name) {
        // 親クラスのメソッド、この場合コンストラクタを呼ぶ
        super(name);
    }

    species() {
        // 親クラスのメソッドを呼ぶ
        super.species();
        console.log("人間です");
    }
}

class Cat extends Mammals {
    constructor(name) {
        super(name);
    }

    species() {
        super.species();
        console.log("猫です");
    }

    // 親クラスのメソッドをオーバーライドしている
    speak() {
        console.log("にゃおと鳴きます");
    }

    run() {
        console.log("走れます");
    }

    swim() {
        console.log("水が嫌いです");
    }
}

class Dog extends Mammals {
    constructor(name) {
        super(name);
    }

    species() {
        super.species();
        console.log("犬です");
    }

    speak() {
        console.log("わんと吠えます");
    }

    run() {
        console.log("走るの大好きです");
    }

    swim() {
        console.log("泳ぐこともできます");
    }
}

class Dolphin extends Mammals {
    constructor(name) {
        super(name);
    }

    species() {
        super.species();
        console.log("イルカです");
    }

    speak() {
        console.log("鳴きます");
    }

    walk() {
        console.log("歩けません");
    }

    run() {
        console.log("水の中を走れます");
    }

    swim() {
        console.log("泳ぎは得意です");
    }
}

const arr = [
    new Man("山田太郎"),
    new Cat("たま"),
    new Dog("ぽち"),
    new Dolphin("フリッパー")
];

for (const v of arr) {
    v.introduction();
    v.species();
    v.speak();
    v.walk();
    v.run();
    v.swim();
    console.log("---------------");
}
```
