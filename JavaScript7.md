# JavaScript まとめ7

## DOMとイベント

DOMとは「Document Object Model」

JavaScriptでHTMLやCSSを操作する仕組み

これによって、ページ移動せずに、画面の一部または全部を変更できる

HTML、PHPなどでは

例えば、

https://example.com/

ページ移動　↓

https://example.com/contact.php

ページ移動　↓

https://example.com/about.php

このようにページ移動しないと画面は変わりません。

しかし、JavaScriptは違います。

ページを開いておいた状態で、ページ移動せずに、画面の一部または全部を変更できます。

## 変更の引き金がイベント

### 例1

index.html
```
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>

<body>
    <h1>JavaScript</h1>
    <div id="hoge"></div>
    <script src="./index.js"></script>
</body>

</html>
```
index.js
```
const e = document.getElementById("hoge")
e.innerHTML = "<h2>おはよう</h2>"
```
document.getElementByIdでHTMLのidプロパティのある要素を取得します。

HTML文書内では、idプロパティは重複しない（オンリーワン、ユニーク、一意）約束ですので、idを振ってあると特定できます。

※ CSS でも id プロパティの要素に装飾できますが、CSS では class は使用しても id は使用しないのが流行です。
id は JavaScript で使うものと覚えましょう。

変数eの型はHTMLElementですが、ここでは大雑把に要素と呼びます。この`<div id="hoge"></div>`のdiv要素を取得しました。

このeにはinnerHTMLプロパティというものがありまして、それにHTML文字列を代入することができます。

似たようなものにinnerTextというのもありますので、innerTextでも試してください。

例1の場合、ページを開くと同時に文字を代入表示しているので、「引き金」は「ページを開いた」ということです。

これでは、HTMLやPHPとあまり変わらないので、違う例を示します。

### 例2

index.html
```
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>

<body>
    <h1>JavaScript</h1>
    <div id="hoge">あいうえお</div>
    <button id="btn">クリック</button>
    <script src="./index.js"></script>
</body>

</html>
```
index.js
```
const e = document.getElementById("hoge")
const b = document.getElementById("btn")

b.addEventListener("click", () => {
    e.innerHTML = "かきくけこ"
})
```

ボタンをクリックしたことを引き金として、画面の一部を変更しています。  
この時ページ移動はしていません。変わったのはページの一部だけです。

addEventListenerはイベントを登録して、それが発生したら実行する関数を記録します。
イベント（引き金）が発生するまでは何もしないでずっと待っているわけです。  
このような特徴から、JavaScriptはイベント駆動型プログラミングと呼ばれます。

## 要素を取得するメソッド

- getElementById()  
id プロパティが指定された文字列に一致する要素
- querySelector()  
CSSセレクターに一致するの最初の要素
- querySelectorAll()  
CSS セレクターに一致するの要素のリスト(複数)
- getElementsByTagName()  
htmlタグ名に一致するの要素のリスト(複数)
- getElementsByClassName()  
クラス名に一致するの要素のリスト(複数)

index.html
```
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>

<body>
    <h1>JavaScript</h1>

    <div id="hoge">あいうえお</div>

    <h2>サンプル</h2>

    <div class="box">
        <ul>
            <li class="lst">一番</li>
            <li class="lst">ニ番</li>
            <li class="lst">三番</li>
            <li class="lst">四番</li>
            <li class="lst">五番</li>
        </ul>
    </div>

    <script src="./index.js"></script>
</body>

</html>
```
index.js
```
const a = document.getElementById("hoge")
console.log("innerHTML", a.innerHTML)
console.log("innerText", a.innerText)
console.log("textContent", a.textContent)
// innerText、innerHTML、textContent の違い
// innerHTML はhtml表示になる
// innerText、textContent はhtmlにならない
// 昔はinnerTextはFireFoxで、textContentはInternet Explorerでは使用できませんでした

const b = document.querySelector("h1")
console.log(b.textContent)

const c = document.querySelector(".box .lst")
console.log(c.textContent)

const d = document.querySelectorAll(".box .lst")
console.log(d)//これは配列なので回せます
for (const v of d) {
    console.log(v.textContent)
}

const e = document.getElementsByTagName("li")
console.log(e)//これは配列なので回せます
for (const v of e) {
    console.log(v.textContent)
}

const f = document.getElementsByClassName("lst")
console.log(f)//これは配列なので回せます
for (const v of f) {
    console.log(v.textContent)
}
```

## 何も無いところから要素を作り追加する

- createElement()  
htmlタグで指定したHTML要素を生成する
- createTextNode()  
新しいTextを生成します
- appendChild()  
特定のhtmlタグの末尾に追加

index.html
```
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>

<body>
    <script src="./index.js"></script>
</body>

</html>
```
index.js
```
const newDiv = document.createElement("div")
const newP1 = document.createElement("p")
const newP2 = document.createElement("p")

const text1 = document.createTextNode("こんにちは")
newP1.appendChild(text1);

const text2 = document.createTextNode("はじめまして")
newP2.appendChild(text2)

newDiv.appendChild(newP1)
newDiv.appendChild(newP2)

document.body.appendChild(newDiv)
```

#### 何も無いところから要素を作るのは記述が面倒と思う場合

innerHTML を使えばよい

先程の例では、以下のように書くと短い
```
document.body.innerHTML = "<div><p>こんにちは</p><p>はじめまして</p></div>"
```

## 要素を削除する

※要素を削除する場合は、親のノードから見て、子要素を削除する方法
- parentNode  
該当要素の親の階層（ノード）の要素を取得
- removeChild  
親のノードから子要素を指定して削除

※ Internet Explorerでは使用できませんが、remove()メソッドが簡単

index.html
```
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>

<body>
    <h1>JavaScript</h1>

    <div id="box">
        <div id="hoge">あいうえお</div>
        <div id="fuga">かきくけこ</div>
        <div id="hage">さしすせそ</div>
    </div>

    <button id="del">削除</button>

    <button id="rm">remove()メソッドで削除</button>

    <script src="./index.js"></script>
</body>

</html>
```
index.js
```
// あいうえおはいきなり削除
const e = document.getElementById("hoge")
const oya = e.parentNode
oya.removeChild(e)

const f = document.getElementById("fuga")
const g = document.getElementById("hage")

const btn = document.getElementById("del")

btn.addEventListener("click", () => {
    // かきくけこはボタンを押したら削除
    f.parentNode.removeChild(f)
})

const rm = document.getElementById("rm")

rm.addEventListener("click", () => {
    // さしすせそはボタンを押したら削除
    g.remove()
})
```

## インラインCSS

HTMLElement.style.プロパティ

https://www.w3schools.com/jsref/dom_obj_style.asp

index.html
```
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>

<body>
    <h1>JavaScript</h1>

    <button id="btn">変更</button>

    <script src="./index.js"></script>
</body>

</html>
```
index.js
```
const h = document.querySelector("h1")
const btn = document.getElementById("btn")

btn.addEventListener("click", () => {
    if (h.style.color == "blue") {
        h.style.color = "red"
    } else if (h.style.color == "red") {
        h.style.color = "blue"
    } else {
        h.style.color = "blue"
    }
})
```

## CSSクラス

Element.classListに対して  
- add  
クラスを追加する
- remove  
クラスを削除する
- contains  
クラスが含まれているか確認する
- toggle  
クラスが含まれていれば削除、含まれていなければ追加する  
などのメソッドで、CSSクラスを操作する

index.html
```
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        .b {
            font-weight: bold;
        }

        .c {
            color: coral;
        }

        .s {
            font-size: 50px;
        }

    </style>
</head>

<body>
    <h1>JavaScript</h1>

    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Magni quia beatae asperiores error, ipsa reprehenderit possimus quibusdam iure impedit explicabo quidem, neque dicta reiciendis in, nemo ea. Vitae, temporibus veritatis?</p>
    <p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Error nostrum tempora accusamus aperiam, fugit ex voluptatibus suscipit quibusdam minus iusto odio cumque in hic earum? Atque numquam officia odio omnis.</p>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Facilis, iste. Delectus eius harum ut at ullam hic minus esse, error totam autem quaerat quis, doloremque, praesentium in maiores recusandae tempore!</p>

    <button id="btn1">変更1</button>
    <button id="btn2">変更2</button>
    <button id="btn3">変更3</button>

    <script src="./index.js"></script>
</body>

</html>
```
index.js
```
const h1 = document.querySelector("h1")
const ps = document.querySelectorAll("p")
const btn1 = document.getElementById("btn1")
const btn2 = document.getElementById("btn2")
const btn3 = document.getElementById("btn3")

btn1.addEventListener("click", () => {
    h1.classList.toggle("s");
})

btn2.addEventListener("click", () => {
    ps[0].classList.add("c")
    ps[1].classList.remove("s")
    ps[2].classList.remove("b")
})

btn3.addEventListener("click", () => {
    ps[0].classList.remove("c")
    ps[1].classList.add("s")
    ps[2].classList.add("b")
})
```

続く

