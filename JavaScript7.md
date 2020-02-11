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

続く
