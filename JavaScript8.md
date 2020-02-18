# JavaScript まとめ8

## DOMとイベントその2

### form関係

formの部品で使われる要素を操作してみます


index.html
```
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        input {
            width: 300px;
        }

    </style>
</head>

<body>
    <input type="text" id="text1">

    <button id="btn1">クリック</button>

    <div id="div1"></div>

    <script src="./index.js"></script>
</body>

</html>
```
index.js
```
const text1 = document.getElementById("text1")
const btn1 = document.getElementById("btn1")
const div1 = document.getElementById("div1")

function showText() {
    div1.innerHTML = text1.value
}

btn1.addEventListener("click", showText)
```

#### クリックイベントばかり使用してきましたが、他にもイベントはたくさんあります(全部は書ききれません)

キーが離されたときに発生するkeyup イベントを使用  
index.html
```
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        input {
            width: 300px;
        }

    </style>
</head>

<body>
    <input type="text" id="text1">

    <div id="div1"></div>

    <script src="./index.js"></script>
</body>

</html>
```
index.js
```
const text1 = document.getElementById("text1")
const div1 = document.getElementById("div1")

function showText() {
    div1.innerHTML = text1.value
}

text1.addEventListener("keyup", showText)
```
要素がフォーカスを失ったときに発生するblur イベントを使用  
index.html
```
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        input {
            width: 300px;
        }

    </style>
</head>

<body>
    <input type="text" id="text1">

    <div id="div1"></div>

    <script src="./index.js"></script>
</body>

</html>
```
index.js
```
const text1 = document.getElementById("text1")
const div1 = document.getElementById("div1")

function showText() {
    div1.innerHTML = text1.value
}

text1.addEventListener("blur", showText)
```
要素がフォーカスを受け取ったときに発生するfocus イベントを使用  
index.html
```
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        input {
            width: 300px;
        }

    </style>
</head>

<body>
    <input type="text" id="text1">

    <script src="./index.js"></script>
</body>

</html>
```
index.js
```
const text1 = document.getElementById("text1")

function showText() {
    alert("focus")
}

text1.addEventListener("focus", showText)
```
ダブルクリックされたときに発生するdblclick イベントを使用  
index.html
```
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        input {
            width: 300px;
        }

    </style>
</head>

<body>

    <input type="text" id="text1">

    <p>下の画像をダブルクリック</p>
    <div id="div1">
        <img src="https://developer.mozilla.org/static/img/favicon144.e7e21ca263ca.png">
    </div>

    <script src="./index.js"></script>
</body>

</html>
```
index.js
```
const text1 = document.getElementById("text1")
const div1 = document.getElementById("div1")
const img1 = document.querySelector("#div1 img")

function showText() {
    text1.value = "ダブルクリックしました！！！！"
    img1.src = "https://developer.mozilla.org/static/img/dino-confuse.545ce52bb569.svg"
}

div1.addEventListener("dblclick", showText)

text1.value = ""
img1.src = "https://developer.mozilla.org/static/img/favicon144.e7e21ca263ca.png"
```
submit(フォームを送信したら)イベントとchange(input,select,textarea要素において、ユーザーによる要素の値の変更が確定したときに発生)イベントを使っています

index.html
```
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
</head>

<body>

    <div class="container my-5">

        <h1>アンケートフォーム</h1>

        <form id="form1">
            <div class="form-group">
                <label for="email1">Eメールアドレス</label>
                <input type="email" class="form-control" name="name_email" id="email1" placeholder="Eメールアドレス">
            </div>
            <div class="form-group">
                <label for="password1">パスワード</label>
                <input type="password" class="form-control" name="name_pw" id="password1" placeholder="パスワード">
            </div>

            <div class="form-group">
                <label for="sex">性別</label>
                <select class="form-control" name="name_sex" id="sex">
                    <option value=""></option>
                    <option value="男">男</option>
                    <option value="女">女</option>
                </select>
            </div>

            <div class="form-group">
                <label for="textarea1">ご意見ご要望</label>
                <textarea class="form-control" name="name_text" id="textarea1" rows="3"></textarea>
            </div>

            <p>好きなペット</p>
            <div class="form-check">
                <label class="form-check-label mr-2"><input type="checkbox" name="name_pet" value="犬">犬</label>
                <label class="form-check-label mr-2"><input type="checkbox" name="name_pet" value="猫">猫</label>
                <label class="form-check-label mr-2"><input type="checkbox" name="name_pet" value="鳥">鳥</label>
                <label class="form-check-label mr-2"><input type="checkbox" name="name_pet" value="爬虫類">爬虫類</label>
                <label class="form-check-label"><input type="checkbox" name="name_pet" value="ハムスター">ハムスター</label>
            </div>

            <p class="my-3">年代</p>
            <div class="form-check">
                <label class="form-check-label mr-2"><input type="radio" name="name_age" value="10" checked>10代</label>
                <label class="form-check-label mr-2"><input type="radio" name="name_age" value="20">20代</label>
                <label class="form-check-label mr-2"><input type="radio" name="name_age" value="30">それ以上</label>
            </div>

            <p class="my-3">スライドさせて年収を教えて下さい <span id="nensyu"></span>(万円)</p>
            <div class="form-group">
                <input type="range" class="form-control-range" id="range" min="0" max="10000" step="10" value="0">
            </div>

            <button type="submit" class="btn btn-primary">送信する</button>
        </form>

    </div>
    <script src="./index.js"></script>
</body>

</html>
```
index.js
```
const form1 = document.getElementById("form1")
const nensyu = document.getElementById("nensyu")
const range = document.getElementById("range")

function showText(e) {

    // ページ移動を止める、これは頻出　今後何回も使います
    e.preventDefault()

    // 全部にidを振って、それにアクセスする方法もありますが、ここでは主にname属性を使います
    // form1 はformの要素を取得
    // form1.name属性でアクセスできる
    const email = form1.name_email.value
    const pw = form1.name_pw.value
    const sex = form1.name_sex.value
    const text = form1.name_text.value

    // getElementsByName でname属性を取得
    const p = document.getElementsByName("name_pet")
    // このpは配列
    let pet = ""
    for (const v of p) {
        // チェックされたものだけ
        if (v.checked) {
            pet += v.value + " "
        }
    }

    const age = form1.name_age.value
    const nen = range.value

    alert(`
Eメールアドレス：${email}
パスワード：${pw}
性別：${sex}
ご意見ご要望：
${text}
好きなペット：${pet}
年代：${age}
年収：${nen}万円
`)
}

function nensyuShow() {
    nensyu.innerHTML = range.value
}

form1.addEventListener("submit", showText)

range.addEventListener("change", nensyuShow)
```

続く
