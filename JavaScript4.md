# JavaScript まとめ4

## オブジェクト

関連のあるデータ（プロパティ）と機能（メソッド）の集まり。

JavaScriptに初めから入っている組み込みのオブジェクトと、自分で作る自作のオブジェクトがあります。

### 組み込みのオブジェクトの例 Math

Math は、数学的な定数と関数を提供するプロパティとメソッドを持つ、組み込みのオブジェクトです
```
console.log(Math);
```
クリックして展開すると以下のようになっています
```
Math

プロパティ

​E: 2.718281828459045
​LN10: 2.302585092994046
​LN2: 0.6931471805599453
​LOG10E: 0.4342944819032518
​LOG2E: 1.4426950408889634
​PI: 3.141592653589793
​SQRT1_2: 0.7071067811865476
​SQRT2: 1.4142135623730951

​functionとなっているのはメソッド

abs: function abs()
​acos: function acos()
​acosh: function acosh()
​asin: function asin()
​asinh: function asinh()
​atan: function atan()
​atan2: function atan2()
​atanh: function atanh()
​cbrt: function cbrt()
​ceil: function ceil()
​clz32: function clz32()
​cos: function cos()
​cosh: function cosh()
​exp: function exp()
​expm1: function expm1()
​floor: function floor()
​fround: function fround()
​hypot: function hypot()
​imul: function imul()
​log: function log()
​log10: function log10()
​log1p: function log1p()
​log2: function log2()
​max: function max()
​min: function min()
​pow: function pow()
​random: function random()
​round: function round()
​sign: function sign()
​sin: function sin()
​sinh: function sinh()
​sqrt: function sqrt()
​tan: function tan()
​tanh: function tanh()
​toSource: function toSource()
​trunc: function trunc()
​以下省略
```

Mathオブジェクトの詳細はこちら
https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Math

```
// プロパティを取得
console.log(Math.PI);

// メソッドを実行
// 四捨五入
console.log(Math.round(10.4));
console.log(Math.round(10.5));
// 切り上げ 
console.log(Math.ceil(10.5));
// 切り捨て
console.log(Math.floor(10.5));
```
桁指定して四捨五入、切り上げ、切り捨てする場合は？
```
// 四捨五入（切り上げ 切り捨て も同じ理屈）
const a = 123.456;

// 小数点2位で四捨五入
const a1 = Math.round(a * 10) / 10;
console.log(a1);//123.5

// 小数点3位で四捨五入
const a2 = Math.round(a * 100) / 100;
console.log(a2);//123.46
```

### Math.random()

0.0以上1.0未満までのランダムな浮動小数点を取得

```
console.log(Math.random());//0.8548820694207037 などのランダムな浮動小数点
```
任意の範囲の整数の乱数を取得したい場合
```
// 切り捨てのfloorで囲む

// 0 ~ 3をランダムに出したいとき
console.log(Math.floor(Math.random() * 4));

// Math.floor(Math.random() * (最大値 - 最小値 + 1) + 最小値));

// 1 ~ 3をランダムに出したいとき
console.log(Math.floor(Math.random() * (3 - 1 + 1) + 1));

// 5 ~ 10をランダムに出したいとき
console.log(Math.floor(Math.random() * (10 - 5 + 1) + 5));
```

### 組み込みのオブジェクトの例 Date

Dateなどの通常のオブジェクトは new を付けて生成します。  
先程の Math はそのまま使えましたが、Mathは「静的プロパティ」「静的メソッド」のみ提供しているクラスだから特別です。
```
const obj = new Date();

console.log(obj);
console.log(obj.getFullYear());
// 月は+1する
console.log(obj.getMonth() + 1);
console.log(obj.getDate());
// 日曜日が0、月が1、火が2、水が3、木が4、金が5、土が6
console.log(obj.getDay());
// 時間
console.log(obj.getHours());
console.log(obj.getMinutes());
console.log(obj.getSeconds());

// 日本語曜日の配列を作る
const weekJP = ["日", "月", "火", "水", "木", "金", "土"];
// 配列なのでインデックスでアクセスできます
console.log(weekJP[2]);
// これを利用して日時をフォーマット
const nowDataTime = `${obj.getFullYear()}年${obj.getMonth() + 1}月${obj.getDate()}日(${weekJP[obj.getDay()]})${obj.getHours()}:${obj.getMinutes()}:${obj.getSeconds()}`;

console.log(nowDataTime);
```
加算減算の例

JavaScriptはPHPに比べて日付の計算が少々面倒な気がしますができることはできます。

UNIXタイムスタンプに変換してから足し算引き算  
UNIXタイムスタンプとは1970年1月1日午前0時0分0秒からの経過秒数
```
// 2020-1-1
const dt1 = new Date(2020, 1 - 1, 1, 0, 0, 0);
let unix1 = dt1.getTime();
// ミリ秒なので13桁表示
console.log(unix1);//1577804400000
// 秒単位の10桁表記にする
unix1 = Math.floor(unix1 / 1000);
console.log(unix1);//1577804400

// 2020-1-10
const dt2 = new Date(2020, 1 - 1, 10, 0, 0, 0);
let unix2 = dt2.getTime();
unix2 = Math.floor(unix2 / 1000);
console.log(unix2);//1578582000

// 引き算する
const s = unix2 - unix1;
console.log(s);//777600秒

// 1日の秒数
const day1 = 60 * 60 * 24;
// 差は777600秒なので日にすると9日
console.log(s / day1);

// 2020-1-1のunixタイムは1577804400
// これに30日分のunixタイムを足す
const s2 = unix1 + day1 * 30;
console.log(s2);//1580396400秒

// これを日付に変換すると 
const d2 = new Date(s2 * 1000);//ミリ秒にするために1000を掛ける
console.log(d2);
```
set系メソッドを使用した場合
```
function format(dt) {
    // 日本語曜日の配列を作る
    const weekJP = ["日", "月", "火", "水", "木", "金", "土"];
    // これを利用して日時をフォーマット
    const d = `${dt.getFullYear()}年${dt.getMonth() + 1}月${dt.getDate()}日(${weekJP[dt.getDay()]})${dt.getHours()}:${dt.getMinutes()}:${dt.getSeconds()}`;
    // 表示
    console.log(d);
}

// 日付の加算
const dt = new Date(2019, 1 - 1, 1, 0, 0, 0);

format(dt);

// 1年後
dt.setFullYear(dt.getFullYear() + 1);

format(dt);

// 6か月後
dt.setMonth(dt.getMonth() + 6);

format(dt);

// 10日後
dt.setDate(dt.getDate() + 10);

format(dt);

// 20時間後
dt.setHours(dt.getHours() + 20);

format(dt);

// 30分後
dt.setMinutes(dt.getMinutes() + 30);

format(dt);

// 10秒後
dt.setSeconds(dt.getSeconds() + 10);

format(dt);
```
https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Date

