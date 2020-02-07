# JavaScript まとめ6

## 非同期処理

JavaScriptの特徴は非同期処理にあります。

他のプログラム言語をやっていた人にとって、JavaScriptの非同期処理は難解な部分です。

以下のPHPを実行してください  
※ sleepは与えられた秒数分プログラムの実行を遅延させます
```
<?php

sleep(1);

echo "はじめまして\n";

echo "プログラム終端\n";
```

以下のJavaScriptを実行してください  
※ setTimeoutは一定時間後に特定の処理を行う（繰り返さずに一度だけ）メソッド
```
setTimeout(() => {
    console.log("はじめまして");// 1
}, 1000);//ミリ秒指定なので1000が1秒

console.log("プログラム終端");// 2
```
上のPHPは同期処理です。  
下のJavaScriptは非同期処理をしています

1秒も待たない0にしたらどうなるでしょうか
```
setTimeout(() => {
    console.log("はじめまして");// 1
}, 0);

console.log("プログラム終端");// 2
```
これでも2が最初に表示されます

### 非同期は実行順序が変わる

同期処理は上から順番に実行。非同期処理は実行順番が変わると覚えましょう。  
もしも2を最後に表示したければ以下のように書きます。

```
setTimeout(() => {
    console.log("はじめまして");
    console.log("プログラム終端");
}, 1000)
```

### コールバック地獄

以下のPHPを実行してください
```
<?php

sleep(1);

echo "はじめまして\n";

sleep(1);

echo "私は\n";

sleep(1);

echo "山田太郎です\n";

echo "プログラム終端\n";
```

これをJavaScriptで書くと
```
setTimeout(() => {

    console.log("はじめまして");

    setTimeout(() => {

        console.log("私は");

        setTimeout(() => {

            console.log("山田太郎です");
            console.log("プログラム終端");

        }, 1000)

    }, 1000)

}, 1000)
```
入れ子が深くなります。  
入れ子が深くなってわかりにくくなることを、JavaScriptでは俗に「コールバック地獄 (Callback Hell)」と呼びます。

## setTimeout と setInterval

setTimeout は一定時間後に特定の処理を行う（繰り返さずに一度だけ）メソッド  
setInterval は一定時間間隔で特定の処理を繰り返し行うメソッド  

setTimeout
```
function hello() {
    console.log(`こんにちは`);
}

// 3秒後にhello関数を実行
// helloのような関数は、他の関数(メソッド)の引数として渡す関数なのでコールバック関数と呼ばれる
setTimeout(hello, 3000);
```

setInterval
```
let count = 0;
function hello() {
    count++;
    console.log(`こんにちは ${count}`);
}

// 3秒間隔でhello関数を実行
// helloのような関数は、他の関数(メソッド)の引数として渡す関数なのでコールバック関数と呼ばれる
setInterval(hello, 3000);
```

setInterval を止めたくなったらどうするか？
```
let id;
let count = 0;
function hello() {
    count++;
    console.log(`こんにちは ${count}`);
    if (count === 3) {
        // タイマーを止める
        clearInterval(id);
        console.log("止まりました");
    }
}

id = setInterval(hello, 3000);
```
setTimeout setInterval 両方とも、数字の戻り値(intervalID)があり、
それを変数に保存しておいてから、止めたい時に clearInterval(その変数) という使い方で止まります。
setTimeout の場合、１回しか実行しないので、実行前に止めることになります。

### なぜ非同期処理が必要か

同期処理の方がプログラマ的には書きやすいし、読みやすいですが、なぜJavaScriptは非同期処理なのでしょうか？  
同期処理だと時間のかかる重い処理をした時に、その処理が終わるまでブラウザがフリーズ(htmlが途中までしか表示しないなど)してしまいます。  
非同期処理だと時間のかかる重い処理を後回しにして実行するのでブラウザがフリーズしない。  
JavaScript は基本ブラウザで動く言語です。ブラウザがフリーズすると利用する人は大変不便です。  
ですからJavaScriptには「非同期処理」が備わっています。

PythonやC#、その他大勢のプログラム言語は、ブラウザで動かす言語ではないです。  
しかしGUI ([マウスや指などでポチポチ操作できる画面のこと](https://wa3.i-3-i.info/word1371.html)) は作ることができます。  
GUIでも時間のかかる重い処理をした時は、画面がフリーズします。  
PythonやC#、その他大勢の言語は、フリーズ回避策として、並行処理(マルチスレッド・マルチタスク)をします。  
JavaScript の場合シングルスレッドの処理系なので、並行処理ができません。  
JavaScript は並行処理ができないかわりに「非同期処理」なのです。

下の例のsetTimeoutは「処理が重い非同期処理」という意味で使っています。
実際はこのような使い方はしません。
```
const books = [
    {
        title: "FACTFULNESS(ファクトフルネス) 10の思い込みを乗り越え、データを基に世界を正しく見る習慣",
        author: "ハンス・ロスリング",
        price: "1980"
    }, {
        title: "自分の頭で考えたい人のための15分間哲学教室",
        author: "アン・ルーニー",
        price: "1880"
    }
];

function getBook() {
    setTimeout(() => {
        let output = "";
        for (const v of books) {
            output += v.title + "\n";
        }
        console.log(output);
    }, 1000);
}

function addBook(book) {
    setTimeout(() => {
        books.push(book);
    }, 2000);
}

addBook({
    title: "失敗図鑑 すごい人ほどダメだった!",
    author: "大野 正人",
    price: "1320"
});

getBook();
```
addBookをしても、addBookの方が時間がかかるので追加できてないです。

コールバック関数で対応
```
const books = [
    {
        title: "FACTFULNESS(ファクトフルネス) 10の思い込みを乗り越え、データを基に世界を正しく見る習慣",
        author: "ハンス・ロスリング",
        price: "1980"
    }, {
        title: "自分の頭で考えたい人のための15分間哲学教室",
        author: "アン・ルーニー",
        price: "1880"
    }
];

function getBook() {
    setTimeout(() => {
        let output = "";
        for (const v of books) {
            output += v.title + "\n";
        }
        console.log(output);
    }, 1000);
}

function addBook(book, callback) {
    setTimeout(() => {
        books.push(book);
        callback();
    }, 2000);
}

addBook({
    title: "失敗図鑑 すごい人ほどダメだった!",
    author: "大野 正人",
    price: "1320"
}, getBook);
```
addBookにgetBook関数を渡して、addBookしてからgetBookするような工夫

## Promise

Promiseとは何か？  
ここからは難しいので「コールバック地獄」を解消する仕組みが作られたという程度の理解でいいですが、将来的には覚えた方がよい内容です。

http://www.tohoho-web.com/ex/promise.html の例
```
// 引数を2倍にする非同期関数
function aFunc1(data, callback) {
    setTimeout(() => {
        callback(data * 2);
    }, Math.random() * 1000);
}

function sample_callback() {
    // 非同期関数を用いて100の2倍を求める
    aFunc1(100, (value) => {
        console.log(value);
    });
}

sample_callback();
```
1回だけ呼び出すのであれば、上記で問題ありませんが、1回目で得られた値を用いて、aFunc1() を2度、3度呼び出そうとすると、下記の様な実装になります。
```
// 引数を2倍にする非同期関数
function aFunc1(data, callback) {
    setTimeout(() => {
        callback(data * 2);
    }, Math.random() * 1000);
}

function sample_callback_hell() {
    aFunc1(100, (data) => {
        console.log(data);
        aFunc1(data, (data) => {
            console.log(data);
            aFunc1(data, (data) => {
                console.log(data);
            });
        });
    });
}

sample_callback_hell();
```
以下のように書くとうまくいきそうですが、実行結果がランダムになります
```
// 引数を2倍にする非同期関数
function aFunc1(data, callback) {
    setTimeout(() => {
        callback(data * 2);
    }, Math.random() * 1000);
}

function sample_timing_problem() {
    aFunc1(100, function (data) {
        console.log(data);
    });
    aFunc1(200, function (data) {
        console.log(data);
    });
    aFunc1(400, function (data) {
        console.log(data);
    });
}

sample_timing_problem();
```
Promiseを使うとこのように書けます
```
// Promise オブジェクトを返却する関数
function aFunc2(data) {
    return new Promise(function (callback) {
        setTimeout(function () {
            callback(data * 2);
        }, Math.random() * 1000);
    });
}

function sample_promise3() {
    aFunc2(100).then((data) => {
        console.log(data);
        return aFunc2(data);
    })
        .then((data) => {
            console.log(data);
            return aFunc2(data);
        })
        .then((data) => {
            console.log(data);
        });
}

sample_promise3();
```
非同期処理の成功時(resolve)、失敗時(reject)を返します。
```
const books = [
    {
        title: "FACTFULNESS(ファクトフルネス) 10の思い込みを乗り越え、データを基に世界を正しく見る習慣",
        author: "ハンス・ロスリング",
        price: "1980"
    }, {
        title: "自分の頭で考えたい人のための15分間哲学教室",
        author: "アン・ルーニー",
        price: "1880"
    }
];

function getBook() {
    setTimeout(() => {
        let output = "";
        for (const v of books) {
            output += v.title + "\n";
        }
        console.log(output);
    }, 1000);
}

function addBook(book) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            books.push(book);

            const error = false;

            if (!error) {
                resolve();
            } else {
                reject("エラーです");
            }

        }, 2000);
    });
}

addBook({
    title: "失敗図鑑 すごい人ほどダメだった!",
    author: "大野 正人",
    price: "1320"
}).then(getBook);
```
resolveメソッドを呼びだす事で非同期処理の成功を示し、thenメソッドの引数の処理を実行しています
```
const books = [
    {
        title: "FACTFULNESS(ファクトフルネス) 10の思い込みを乗り越え、データを基に世界を正しく見る習慣",
        author: "ハンス・ロスリング",
        price: "1980"
    }, {
        title: "自分の頭で考えたい人のための15分間哲学教室",
        author: "アン・ルーニー",
        price: "1880"
    }
];

function getBook() {
    setTimeout(() => {
        let output = "";
        for (const v of books) {
            output += v.title + "\n";
        }
        console.log(output);
    }, 1000);
}

function addBook(book) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            books.push(book);

            const error = true;

            if (!error) {
                resolve();
            } else {
                reject("エラーです");
            }

        }, 2000);
    });
}

addBook({
    title: "失敗図鑑 すごい人ほどダメだった!",
    author: "大野 正人",
    price: "1320"
})
    .then(getBook)
    .catch((error) => {
        console.log(error);
    });
```
エラーの場合はcatchで受け取れます。

https://qiita.com/rawHam/items/838eefc80bc35a90e49a の例
```
function f1() {
    return new Promise((resolve, reject) => {
        console.log("[START]");
        console.log("#1: f1");
        resolve("f1 ==> f2");
    })
}

function f2(passVal) {
    return new Promise((resolve, reject) => {
        //setTimeoutは動きをそれっぽくするために入れているだけなので削除可
        setTimeout(() => {
            //f1のresolve内の "f1 ==> f2" がpassValに代入される
            console.log(passVal);
            console.log("#2: f2");
            resolve("f2 ==> f3");
        }, Math.random() * 2000)
    })
}

function f3(passVal) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log(passVal);
            console.log("#3: f3");
            resolve("f3 ==> f4");
        }, Math.random() * 3000)
    })
}

function f4(passVal) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log(passVal);
            console.log("#4: f4");
            resolve("f4");
        }, Math.random() * 4000)
    })
}

//f1->f2->f3->f4の順に実行
f1()
    .then(f2)
    .then(f3)
    .then(f4)
    .then((response) => {
        console.log("Final function: " + response);
        console.log("[END]");
    })
```

## async/await

async/await を使うと簡潔に非同期処理が書け更に見やすい。

今の非同期処理はこの書き方がオススメ。  
しかし、古いブラウザは未対応なので、トランスパイル（JavaScript のコードを古いブラウザでも見れるように、新しい書き方から古い書き方へと変換）した方がいい

```
function f1() {
    return new Promise((resolve, reject) => {
        console.log("[START]");
        console.log("#1: f1");
        resolve("f1 ==> f2");
    })
}

function f2(passVal) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log(passVal);
            console.log("#2: f2");
            resolve("f2 ==> f3");
        }, Math.random() * 2000)
    })
}

function f3(passVal) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log(passVal);
            console.log("#3: f3");
            resolve("f3 ==> f4");
        }, Math.random() * 3000)
    })
}

function f4(passVal) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log(passVal);
            console.log("#4: f4");
            resolve("f4");
        }, Math.random() * 4000)
    })
}

//f1->f2->f3->f4の順に実行
async function init() {
    const res1 = await f1();
    console.log(res1);

    const res2 = await f2(res1);
    console.log(res2);

    const res3 = await f3(res2);
    console.log(res3);

    const res4 = await f4(res3);
    console.log("Final function: " + res4);
}

init();
```
async/await のエラー処理
```
function f1() {
    return new Promise((resolve, reject) => {
        console.log("[START]");
        console.log("#1: f1");
        resolve("f1 ==> f2");
    })
}
function f2(passVal) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log(passVal);
            console.log("#2: f2");
            resolve("f2 ==> f3");
        }, Math.random() * 2000)
    })
}

function f3(passVal) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log(passVal);
            console.log("#3: f3");
            reject("エラー");
        }, Math.random() * 3000)
    })
}

function f4(passVal) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log(passVal);
            console.log("#4: f4");
            resolve("f4");
        }, Math.random() * 4000)
    })
}

//f1->f2->f3->f4の順に実行
async function init() {
    try {
        const res1 = await f1();
        console.log(res1);

        const res2 = await f2(res1);
        console.log(res2);

        const res3 = await f3(res2);
        console.log(res3);

        const res4 = await f4(res3);
        console.log("Final function: " + res4);
    } catch (err) {
        console.log(err);
    }
}

init();
```

以上、難しい内容でしたが、時間をかけて覚えまよう。

以下のページは詳しいです。

https://sbfl.net/blog/2019/11/04/promise-cookbook/

https://rightcode.co.jp/blog/information-technology/javascript-async-await
