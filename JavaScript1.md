# JavaScript まとめ1

表示確認は Google Chrome が圧倒的に使われています。
デプロイ（本番配置）前には、様々なブラウザで表示確認した方が良いです(昔はInternet Explorerという困ったブラウザがありまして、JavaScriptやCSSでさえもうまく表示できずに大変困りました)。
最近は、webpack や Parcel を使ってファイルを束ねたりするので、その過程で Babel が動いていい感じにしてくれます(この辺りは後半でやっていきます)。

- webpack や Parcel は JavaScript のファイルを一つに束ねるツール
- Babel は JavaScript のコードを古いブラウザでも見れるように、新しい書き方から古い書き方へと変換するツール

## 行末のセミコロン

```
const person = "John Doe";← これ
```
あってもなくてもよいですが、場合によってないとダメなので、セミコロンありで記述していきます。

## JavaScriptの書く場所

HTMLファイルに直接書く場合
```
<body>
    <script>
        // ここにJavaScriptコードを書く
    </script>
</body>
```
HTMLファイルから読み込んで使う場合
```
<body>
    <script src="./js/index.js"></script>
</body>
```
この場合 js/index.js のファイルにJavaScriptコードを書く  
scriptタグはbody終了タグのすぐ上に書くのが多いが、headタグ内に書く方法もある。  
scriptタグに defer属性をつけた例  
HTML パース完了後 JS ファイルを実行
```
<head>
    <script src="./js/index.js" defer></script>
</head>
```
Node.jsで実行するには、コマンドプロンプトや端末で  
```
$ node index.js
```
で実行できます。  
index.js の .js は省略可能。  
この場合も index.js のファイルにJavaScriptコードを書く

## 変数宣言

基本的に const を使用します
```
var a = "varは古い書き方";

const b = "新しい書き方";
// invalid assignment to const `b'
// b = "エラーになったら let";
let c = "新しい書き方";
c = "let は再代入可能";
```

### 1つのステートメントで多くの変数を宣言できます

```
const person = "John Doe", carName = "Volvo", price = 200;
```

### 値なしで宣言された変数の値は未定義

後で使うために宣言しておく方法
```
let name;

console.log(name);//undefined

if (true) {
    name = "山田太郎";
}

console.log(name);//山田太郎
```

## 色々な出力方法

```
window.alert("Hello, 世界");

console.log("Hello, 世界");

// id="hoge"の箇所に書く
document.getElementById("hoge").innerHTML = "Hello, 世界";

document.write("Hello, 世界");
```

```
// 入力ダイアログ
const result1 = window.prompt("お名前は？");
console.log(`こんにちは、${result1}さん`);// バッククォート(`)で囲めば ${} で変数が展開できる

// 確認ダイアログ
const result2 = window.confirm("よいですか？");
if (result2) {
    console.log("よいです");
} else {
    console.log("よくないです");
}
```

## 変数名のつけ方

JavaScriptは小文字で始まるキャメルケースを使用する

```
// スネークケース
first_name, last_name, master_card, inter_city
↓
// キャメルケース
firstName, lastName, masterCard, interCity
```

## コメント

```
// 一行コメント

/*
複数行
コメント
*/
```

## JavaScriptは動的型付け言語

同じ変数に違うデータ型を代入できる
```
let x;
x = 5;
console.log(x);//5
x = "John";
console.log(x);//John
```
これは初心者にとっては気軽で便利なようですが、バグの原因にもなります。  
チームで開発する場合、バグを減らすためにデータ型に厳しい TypeScript が使われたりします。  
TypeScript は、コンパイルすると JavaScript になる言語です。マイクロソフト製です。

## データ型

|型|例|説明|
|---|---|---|
|未定義(undefined)|undefined|未定義値|
|ヌル(null)|null|ヌル値|
|真偽値(boolean)|true, false|真偽値|
|文字列(string)|"abc..."|文字列|
|数値(number)|123, 123.45, 1.23e45, NaN|数値|
|オブジェクト(object)|const o = new Object()|オブジェクト|
|関数(function)|function() { ... }|関数|
|シンボル(symbol)|Symbol("foo")|ES6(ES2015) で追加された型で、他の値と重複しないユニークなシンボル値を生成します|

### データ型は typeof でわかる

```
const a = 1;
const b = "1";
const c = "こんにちは";
const d = false;

console.log(typeof a);//number
console.log(typeof b);//string
console.log(typeof c);//string
console.log(typeof d);//boolean
```

## 構文

if, else if, else
```
function myFunction() {
    let greeting;
    const time = new Date().getHours();//Dateは日付や時刻を扱うことが可能なJavaScriptに最初から入っているオブジェクト
    
    console.log(time);

    if (time < 10) {
        greeting = "Good morning";
    } else if (time < 20) {
        greeting = "Good day";
    } else {
        greeting = "Good evening";
    }
    console.log(greeting);
}
myFunction();
```
switch
```
switch(expression) {
  case x:
    // code block
    break;
  case y:
    // code block
    break;
  default:
    // code block
}
```

```
let day;
switch (new Date().getDay()) {
    case 0:
        day = "Sunday";
        break;
    case 1:
        day = "Monday";
        break;
    case 2:
        day = "Tuesday";
        break;
    case 3:
        day = "Wednesday";
        break;
    case 4:
        day = "Thursday";
        break;
    case 5:
        day = "Friday";
        break;
    case 6:
        day = "Saturday";
}

console.log(day);
```

```
let text;
switch (new Date().getDay()) {
    case 4:
    case 5:
        text = "まもなく週末";
        break;
    case 0:
    case 6:
        text = "週末です";
        break;
    default:
        text = "週末を楽しみにして";
}

console.log(text);
```
while
```
let i = 0;
while (i < 10) {
    console.log(i)
    i++;
}
```
do while
```
let i = 0;

do {
    console.log(i);
    i++;
} while (i < 10);
```
while, do while 違いは、必ず１回は実行するのが do while
```
let i = 100;

while (i < 10) {
    console.log("while", i)
    i++;
}

let j = 100;

do {
    console.log("do while", j);
    j++;
} while (j < 10);
```
for 一番基本的なfor文
```
for (let i = 0; i < 5; i++) {
    console.log(i);
}
```

## 演算子

```
+, -, *, **, /, %, ++, --
```

```
const x = 2 ** 3;
console.log(x);//2の3乗で8
```

```
const x = 9 % 2;
console.log(x);//9 / 2 = 4 余り 1
```

```
x += y は同じ意味 x = x + y
x -= y は同じ意味 x = x - y
x *= y は同じ意味 x = x * y
x /= y は同じ意味 x = x / y
x %= y は同じ意味 x = x % y
x **= y は同じ意味 x = x ** y
```

```
let name = "山田 ";
name += "太郎";
console.log(name);//山田 太郎
```

## 比較演算子

if などで使用
```
==, ===, !=, !==, >, <, >=, <=, ? :
```

=== 3つあるのは、(※)データタイプ(データ型、型)も比較する

```
const a = 1;
const b = "1";

if (a == b) {
    console.log("同じです");//ここが表示
} else {
    console.log("違います");
}

// === にすると
if (a === b) {
    console.log("同じです");
} else {
    console.log("違います");//ここが表示
}
```

? : は3項演算子で出てきます

```
const age = 26;
const beverage = (age >= 21) ? "ビール" : "ジュース";
console.log(beverage);// "ビール"
```

## 論理演算子

```
&&, ||, !
```

## ブール値 boolean

ブール値には、trueまたはfalseの2つの値のみを指定できます。

```
const a = true;

console.log(a);//true
console.log(!a);//false

const b = false;

console.log(a && b);//false
console.log(a || b);//true

const x = 5;
const y = 5;
const z = 6;

console.log(x == y);//true
console.log(x == z);//false
```

## 文字列

JavaScriptでは文字列型とStringオブジェクトがあり混乱を招きますが、すべてただの文字列と覚えておいて問題ないです。
文字列で使うメソッドとプロパティは、Stringオブジェクトのものを使っているということです。

文字列はダブルクォーテーション（"）またはシングルクォーテーション（'）で囲んで表現
```
'foo'
"bar"
```
配列でもよく使う length プロパティ
```
const hello = 'Hello, 世界';
const helloLength = hello.length;
console.log(helloLength);//9
console.log(hello[7]);//世
// 文字列の一番最後の文字を取得したいときは、先ほどの length プロパティと組み合わせて以下のようにします
console.log(hello[helloLength - 1]);//界
```
部分文字列を探して抽出する
```
const a = "長い文字列の中に、ある文字列が存在するか調べたい";

const f = a.indexOf("ある");
console.log(f);//9

const g = a.indexOf("長");
console.log(g);//0

const h = a.indexOf("わ");
console.log(h);//-1 無いときは-1
```
-1 が返ることを利用して部分文字列が文字列全体に含まれていないかどうか調べることができます。
```
const a = "長い文字列の中に、ある文字列が存在するか調べたい";

if (a.indexOf("調べたい！") !== -1) {
    console.log("あります");
} else {
    console.log("ありません");
}
```
slice メソッドで抽出
```
const a = "slice() メソッドを使用";
console.log(a.slice(8, 12));//メソッド
```
最初の引数は抽出を始める最初の位置で、2番目の引数が抽出する最後の文字の直後の位置

ある文字より後の文字を抽出したい場合には、2 つ目の引数を省略することができます
```
const a = "slice() メソッドを使用";
console.log(a.slice(8));//メソッドを使用
```
toLowerCase()とtoUpperCase()という 2 つのメソッドは大文字・小文字を切り替えます
```
const radData = 'My NaMe Is MuD';
console.log(radData.toLowerCase());//my name is mud
console.log(radData.toUpperCase());//MY NAME IS MUD
```
文字列の一部分を書き換える
```
const a = "酸桃も桃も桃の類";

console.log(a.replace("桃", "もも"));
```
これは先頭のものしか置換されません

全置換をする場合、正規表現
```
const a = "酸桃も桃も桃の類";

console.log(a.replace(/桃/g, "もも"));//酸ももももももももの類
```
split と join を使う方法もある
```
const a = "酸桃も桃も桃の類";

console.log(a.split("桃").join("もも"));//酸ももももももももの類
```
split メソッドは、指定した separator 文字列を使って分割する箇所を決定し、
文字列を複数の部分文字列に区切ることにより配列に分割します
```
const str = 'The quick brown fox jumps over the lazy dog.';
const words = str.split(' ');
console.log(words);
//Array(9) [ "The", "quick", "brown", "fox", "jumps", "over", "the", "lazy", "dog." ]

const str2 = "https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/String";
const words2 = str2.split("/");
console.log(words2[6]);//JavaScript
```
trim メソッドは、文字列の両端の空白を削除します
```
const greeting = '   Hello world!   ';

console.log(greeting);//   Hello world!   
console.log(greeting.trim());//Hello world!
```
startsWith メソッドは文字列が引数で指定された文字列で始まるかを判定して true か false を返します  
endsWith メソッドは終わりの文字を判定して true か false を返します  
```
const str = 'To be, or not to be, that is the question.';

console.log(str.startsWith('To be'));//true
console.log(str.endsWith('question.'));//true
```
### テンプレート文字列

最近のJavaScriptでは、バッククォート（`～`）で囲むと変数や式が展開できます  
バッククォートの出し方は、Windowsなら「Shift」＋「@」
```
const name = "山田太郎";
const age = 20;

const a = `私の名前は${name}です。年齢は${age * 3}です。`;

console.log(a);//私の名前は山田太郎です。年齢は60です。

console.log(`

\`バッククォートの中でバッククォートを使う場合\`
改行も
書きやすい
です。by ${name}

`);
```
includes メソッドは、1 つの文字列を別の文字列の中に見出すことができるかどうかを判断し、必要に応じて true か false を返します
```
const sentence = 'The quick brown fox jumps over the lazy dog.';

const word = 'fox';

// 三項演算子になっている
console.log(`The word "${word}" ${sentence.includes(word) ? 'is' : 'is not'} in the sentence`);
// The word "fox" is in the sentence
```
まだまだ沢山文字列メソッドがありますがこのあたりにしておきます  
https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/String










