# JavaScript まとめ3

### 配列を使ったループ処理

for の場合
```
const place = ["東京", "中山", "阪神", "京都", "中京", "新潟", "小倉", "札幌", "福島", "函館"];

for (let i = 0; i < place.length; i++) {
    console.log(place[i]);
}
```
for in の場合(JavaScriptのオブジェクトを処理するのに使うことが多い)
```
for (let v in place) {
    console.log(v);
}
```
for of の場合
```
for (let v of place) {
    console.log(v);
}
```
forEach の場合
```
place.forEach(v => {
    console.log(v);
});
```

## ループ処理から抜け出す break

break を使うと繰り返し文（for 文、for...in 文、for...of 文、do...while 文、while 文、switch文）のループ処理から抜け出すことができます(forEach内では使えない)
```
let i = 0;

while (true) {
    if (i > 500) {
        break;
    }
    i++;
}

console.log(`${i}回ループされました`);
```
多重ループを一気に抜ける
```
// ダメな例
let i = 0;

while (true) {
    let j = 0;

    while (true) {
        if (j > 5) {
            // ２重ループ内のここで全部抜けたい
            break;
        }
        console.log(`j=${j}`);
        j++;
    }

    if (i > 5) {
        break;
    }
    console.log(`***i=${i}`);
    i++;
}

console.log("終了");
```
ラベルを付ける
```
let i = 0;

loop1:
while (true) {
    let j = 0;

    while (true) {
        if (j > 5) {
            // ２重ループ内のここで抜けたい
            break loop1;
        }
        console.log(`j=${j}`);
        j++;
    }

    if (i > 5) {
        break;
    }
    console.log(`***i=${i}`);
    i++;
}

console.log("終了");
```

## ループ処理をスキップする continue

continue 以降は実行しないで、ループの先頭に戻ります
```
for (let i = 0; i < 100; i++) {
    if (i % 2 === 0) {
        continue;
    }
    console.log(i);
}
```

## 関数

同じ処理を定義し何度も使い回しができる形にしたもの

関数はデータ型のひとつなので、変数に代入したり、関数の引数として渡したり、戻り値として関数を返すことが可能

https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Functions
```
// (1)
// 普通の関数
function myFunction1(p1, p2) {
    return p1 * p2;
}

// 関数リテラル
// function (p1, p2) {
//     return p1 * p2;
// };

// (2)
// 古いテクニックと言われる
// 即時関数
let x = (function (p1, p2) {
    return p1 * p2;
})(2, 3);
console.log(x);

// (3)
// 関数式
// 関数リテラルを変数に代入
const myFunction2 = function (p1, p2) {
    return p1 * p2;
};

// (4)
// 関数式を書くならアロー関数が今どきの書き方です
// 上をアロー関数にしたもの
const myFunction3 = (p1, p2) => {
    return p1 * p2;
};

// (5)
// 関数式
// 本体が一文である場合こう書ける
const myFunction4 = (p1, p2) => p1 * p2;

const a = myFunction1(2, 3);
const b = myFunction2(2, 3);
const c = myFunction3(2, 3);
const d = myFunction3(2, 3);
const e = myFunction4(2, 3);

console.log(a);
console.log(b);
console.log(c);
console.log(d);
console.log(e);
```

### 関数式は、ある関数を別の関数の引数として渡すときに便利

意味のない使い方ですが、引数に渡せる例
```
function myfn(f, n) {
    return f(n)
}

const f = (x) => {
    return x * 10;
}

const a = myfn(f, 10);

console.log(a);//100
```

### 関数はその関数自身を呼び出すこともできます

```
function factorial(n) {
    if (n == 0 || n == 1) {
        return 1;
    } else {
        return (n * factorial(n - 1));
    }
}

console.log(factorial(1));//6
console.log(factorial(2));//2
console.log(factorial(3));//6
console.log(factorial(4));//24
console.log(factorial(5));//120
console.log(factorial(6));//720
```

```
function loop(x) {
    if (x >= 10){
        return;
    }
    console.log(x);
    loop(x + 1); // 再帰呼出し
}
loop(0);
```

### 関数のスコープ

関数の内部で宣言された変数は、関数の外部からアクセスすることができません  
その一方、関数は自身が定義されたスコープ内で定義されているすべての変数や関数にアクセスできます

```
// 以下の変数はグローバルスコープで定義
const num1 = 20, num2 = 3, name = "Chamahk";

// この関数はグローバルスコープで定義
function multiply() {
    return num1 * num2;
}

multiply(); // 60 を返す

// 入れ子になっている関数の例
function getScore() {
    const num1 = 2, num2 = 3;

    function add() {
        return name + " scored " + (num1 + num2);
    }

    return add();
}

getScore(); // "Chamahk scored 5" を返す
```

### デフォルト引数

```
function multiply(a, b = 1) {
    return a * b;
}

console.log(multiply(5));//5
console.log(multiply(2, 5));//10
```

### 残余引数

```
function fn(...args) {
    console.log(args)
}

fn(2);//Array[2]
fn(2, 3, 4, 5, 6, 7, 8, 9);//Array(8)[2, 3, 4, 5, 6, 7, 8, 9]
```

### クロージャ

基本的な考え方
```
let a = 0;

function clos() {
    a = a + 1;
    console.log(a);
}

clos();//1
clos();//2
clos();//3
```
関数を返す関数を作成  
自身が定義された環境を保持している
```
function clos() {
    let a = 0;
    return () => {
        a = a + 1;
        console.log(a);
    }
}

const fn1 = clos();
fn1();//1
fn1();//2
fn1();//3

const fn2 = clos();
fn2();//1
fn2();//2
fn2();//3

fn1();//4
fn2();//4
```

## 便利な定義済み関数

### eval() は文字列として表された JavaScript コードを式として評価する関数

```
const moji = "2+2*5";
eval(moji);//12
```


