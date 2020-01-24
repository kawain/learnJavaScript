# JavaScript まとめ2

## 配列

JavaScriptの配列は全て可変長です。配列の要素にはどんなデータ型も入ります。  
配列に数値や文字列だけでなく、配列(多次元配列)や、後ほど説明する「オブジェクト」を入れると、より高度で複雑なプログラムが作成可能です。

### 作成

空の配列は、色々なデータを追加していく際の準備としてよく使います
```
const a = [];
const b = [1, 2, 3, 4, 5];

console.log(a);//Array[] 空の配列
console.log(b);//Array(5)[1, 2, 3, 4, 5]
```
newで作成する方法は2通りあります
```
const array1 = new Array(5);    // サイズが5の配列ができる
const array2 = new Array(1, 2); // [1, 2]ができる

console.log(array1);
console.log(array2);
```

### 要素を取得

インデックス番号でアクセス  
範囲外は　undefined
```
const c = ["東京", "中山", "阪神", "京都", "中京", "新潟", "小倉", "札幌", "福島", "函館"];

console.log(c[0]);
console.log(c[2]);
console.log(c[3]);
console.log(c[10]);//undefined
console.log(c[11]);//undefined
console.log(c[100]);//undefined
```

### 要素の数

length プロパティ
```
const c = ["東京", "中山", "阪神", "京都", "中京", "新潟", "小倉", "札幌", "福島", "函館"];

console.log(c.length);
// 最後の要素
const last = c[c.length - 1];
console.log(last);//函館
```

### 配列の末尾に要素を追加

push
```
const c = ["東京", "中山", "阪神", "京都", "中京", "新潟", "小倉", "札幌", "福島", "函館"];

const newLength = c.push("みかん");

console.log(newLength);//11
console.log(c);//
```

### 配列の末尾の要素を削除

pop
```
const c = ["東京", "中山", "阪神", "京都", "中京", "新潟", "小倉", "札幌", "福島", "函館"];

const last = c.pop();

console.log(last);//函館
console.log(c);//
```

### 配列の先頭の要素を削除

shift
```
const c = ["東京", "中山", "阪神", "京都", "中京", "新潟", "小倉", "札幌", "福島", "函館"];

const first = c.shift();

console.log(first);//東京
console.log(c);//
```

### 配列の先頭に要素を追加

```
const c = ["東京", "中山", "阪神", "京都", "中京", "新潟", "小倉", "札幌", "福島", "函館"];

const newLength = c.unshift("いちご");

console.log(newLength);//11
console.log(c);//
```

### 要素のインデックスを取得

indexOf  
なければ -1 が返る
```
const items = ['バナナ', 'イチゴ', 'リンゴ', 'メロン'];

const result = items.indexOf('リンゴ');
console.log(result);//2

const result2 = items.indexOf('リンゴ2');
console.log(result2);//-1
```

### インデックス位置を指定して要素を削除

```
const items = ['バナナ', 'イチゴ', 'リンゴ', 'メロン'];

// インデックス1から2要素削除
const removedItem = items.splice(1, 2);
console.log(removedItem);//Array["イチゴ", "リンゴ"]

console.log(items);//Array["バナナ", "メロン"]

// 範囲外でもエラーにはならない
const removedItem2 = items.splice(1, 100);
console.log(removedItem2);//Array["メロン"]

console.log(items);//Array["バナナ"]
```

### 分割代入

```
const foo = ['one', 'two', 'three'];

const [one, two, three] = foo;

console.log(one); // "one"
console.log(two); // "two"
console.log(three); // "three"

let a, b;

[a, b] = [10, 20];

console.log(a);// 10
```
既定値の設定  
配列から取り出した値が undefined だった場合に使用される既定値を指定できます。
```
let a, b;

[a = 5, b = 7] = [1];
console.log(a); // 1
console.log(b); // 7
```
変数の入れ替え
```
let a = 1;
let b = 3;

[a, b] = [b, a];
console.log(a); // 3
console.log(b); // 1
```
関数から返された配列の解析
```
function f() {
    return [1, 2];
}

let a, b;
[a, b] = f();
console.log(a); // 1
console.log(b); // 2
```
返り値の無視
```
function f() {
    return [1, 2, 3];
}

var [a, , b] = f();
console.log(a); // 1
console.log(b); // 3
```
このようにすべての返り値を無視することもできます
```
[,,] = f();
```

## 配列は参照（値渡しと参照渡し）

参照とは、本当のデータがある場所ではなくて、本当のデータがある場所のアドレスのこと。

標準で値渡しになるデータ型は、プリミティブ型(数値やブール型、文字列など)と呼ばれる小さいデータのものです。
配列や後述する自作のオブジェクトなどはデータ容量が大きくなりがちです。
大きなデータをプログラム内でコピーしまくると、メモリ使用量が増えます。
そういうことを避けるために標準で参照になっていると思います。

値渡し
```
function func1(a) {
    // この a はコピーされた a なので、これを変えても影響が出ない
    a = "こんばんは";
}

const a = "こんにちは";

func1(a);//この a はコピーして渡される

console.log(a);//こんにちは
```
参照渡し
```
function func1(a) {
    // a の住所をもらって、a を書き換えたので影響が出る
    a[0] = "こんばんは";
}

const a = ["こんにちは"];

func1(a);//この a の参照が渡される（アドレス・住所が渡される）

console.log(a);//Array [ "こんばんは" ]
```
参照渡し
```
function func1(a) {
    a[4] = 50;
}

const a = [1, 2, 3, 4, 5];

func1(a);

console.log(a);//Array [  1, 2, 3, 4, 50  ]
```

### 配列をコピーする

場合によってはコピーしたくなる時もあります

slice
```
function func1(a) {
    // この a はコピーされた a なので、これを変えても影響が出ない
    a[4] = 50;
}

const a = [1, 2, 3, 4, 5];

func1(a.slice());

console.log(a);//Array [  1, 2, 3, 4, 5  ]
```

concat

Array.concat([item0, item1, ...])は引数に渡した配列を連結した新しい配列を生成する方法。
したがって、以下のように引数を渡さなければ何も連結していない新しいは配列が生成される。

```
func1(a.concat());
```

コピーには新しい書き方のスプレッド構文がおすすめ
```
function func1(a) {
    a[4] = 50;
}

const a = [1, 2, 3, 4, 5];

func1([...a]);//[...a]

console.log(a);//Array [  1, 2, 3, 4, 5  ]
```

## スプレッド構文

配列に配列を好きな場所に追加
```
const parts = ['shoulders', 'knees'];
const lyrics = ['head', ...parts, 'and', 'toes'];

console.log(lyrics);//Array(5) [ "head", "shoulders", "knees", "and", "toes" ]
```
配列に配列をpush
```
const data1 = [1, 2, 3];
const data2 = ['d', 'e', 'f'];

data1.push(...data2);
console.log(data1);//[ 1, 2, 3, "d", "e", "f" ]
```
配列連結
```
const arr1 = [0, 1, 2];
const arr2 = [3, 4, 5];
const arr3 = [...arr1, ...arr2];

console.log(arr3);
```
配列を分割する
```
const [a, b, ...other] = [1, 2, 3, 4, 5];

console.log(a);        // 1
console.log(b);        // 2
console.log(other);    // [3, 4, 5]
```
文字列を配列にする
```
const word = "おはよう";
const converted = [...word];

console.log(converted);//[ "お", "は", "よ", "う" ]
```
配列から重複を取り除く  
Set オブジェクトは重複しない値の集合を管理するためのオブジェクトです
```
const data = ['a', 'b', 'c', 'a', 'b', 'd'];
const dist = [...(new Set(data))];

console.log(dist);    // ["a","b","c","d"]
```

## 2 次元配列

```
const board = [
    ['R', 'N', 'B', 'Q', 'K', 'B', 'N', 'R'],
    ['P', 'P', 'P', 'P', 'P', 'P', 'P', 'P'],
    [' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '],
    [' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '],
    [' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '],
    [' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '],
    ['p', 'p', 'p', 'p', 'p', 'p', 'p', 'p'],
    ['r', 'n', 'b', 'q', 'k', 'b', 'n', 'r']
];

console.log(board.join('\n') + '\n\n');

// キングの前のポーンを 2 つ前へ移動
board[4][4] = board[6][4];
board[6][4] = ' ';
console.log(board.join('\n'));
```
どうして203になるのか考えてみましょう
```
const arr = [];
for (let i = 0; i < 3; i++) {
    arr[i] = [];
    for (let j = 0; j < 4; j++) {
        arr[i][j] = i * 100 + j;
    }
}
console.log(arr[2][3]);//203
```

### 配列を特定文字で連結して文字列にする

join
```
const items = [2018, 1, 10];

const result = items.join('/');

console.log(result);
```
join()の引数を何も指定しない場合は「カンマ区切り」の文字列を取得できます
```
const items = ['バナナ', 'イチゴ', 'リンゴ', 'メロン'];

const result = items.join();

console.log(result);//バナナ,イチゴ,リンゴ,メロン
```

## 埋める

fill(value, start = 0, end = this.length)

```
let items = [0, 0, 0, 0, 0];
items.fill(1);
console.log(items);//[ 1, 1, 1, 1, 1 ]

items = [0, 0, 0, 0, 0];
items.fill(1, 3);
console.log(items);//[ 0, 0, 0, 1, 1 ]

items = [0, 0, 0, 0, 0];
items.fill(1, 2, 4);
console.log(items);//[ 0, 0, 1, 1, 0 ]
```

```
const array = new Array(100); // サイズ100の配列を作成

console.log(array);

array.fill(0); 

console.log(array);
```

## 配列をソート

Javascriptのソートはデフォルトでは、配列内の各要素を文字列に変換してソートする
```
const array = [8, 1, 9, 12, 10];
array.sort();
console.log(array);//[ 1, 10, 12, 8, 9 ]
```
これを数値としてソートするには

昇順ソート
```
const array = [8, 1, 9, 12, 10]
array.sort((a, b) => {
    if (a < b) return -1;
    if (a > b) return 1;
    return 0;
});

console.log(array);//[ 1, 8, 9, 10, 12 ]
```
降順ソート
```
const array = [8, 1, 9, 12, 10]
array.sort((a, b) => {
    if (a > b) return -1;
    if (a < b) return 1;
    return 0;
});

console.log(array);//[ 12, 10, 9, 8, 1 ]
```

## map(), filter(), reduce()

map() メソッドは、与えられた関数を配列のすべての要素に対して呼び出し、その結果からなる新しい配列を生成します
```
const a1 = [1, 2, 3, 4, 5];

const a2 = a1.map(e => {
    return e * e;
});

console.log(a1);//[ 1, 2, 3, 4, 5 ]
console.log(a2);//[ 1, 4, 9, 16, 25 ]
```
filter() メソッドは、引数として与えられたテスト関数を各配列要素に対して実行し、それに合格したすべての配列要素からなる新しい配列を生成します
```
const a1 = [12, 5, 8, 130, 44];

const a2 = a1.filter(e => {
    return e >= 10;
});

console.log(a1);//[ 12, 5, 8, 130, 44 ]
console.log(a2);//[ 12, 130, 44 ]
```
reduce() は配列の各要素に対して（引数で与えられた）reducer 関数を実行して、単一の値にします。
```
const array1 = [1, 2, 3, 4];

const reducer = (accumulator, currentValue) => accumulator + currentValue;

// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));// 10

// 5 + 1 + 2 + 3 + 4
console.log(array1.reduce(reducer, 5));// 15
```

```
const arr = [1, 2, 3];

function calc(prev, current) {
    return prev += current;
}

const sum = arr.reduce(calc, 0);// 関数と初期値

console.log(sum); // 6
```
calc(prev, current) の箇所は  
1回目 calc(0, 1)  
2回目 calc(1, 2)  
3回目 calc(3, 3)  
となっていき 6 が返る


