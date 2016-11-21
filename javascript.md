# LYRASOFT Javascript 編碼規範

本文件依照 Joomla 編碼規範中的 Javascript 指導原則修改而來。

## 程式縮排

LYRASOFT 以 4 格空格作為 JS 檔案的主要縮排間距，請勿在 `.js` 在檔案中使用 Tab 做排版。

可允許 inline JS 使用 Tab，因 LYRASOFT 的 PHP 規範以 Tab 為縮排，當 JS 不得已必須寫在 PHP 檔案內時則不在此限，
不過縮排以外的程式碼對齊排版請依舊使用空格，避免混用的情形發生。

## 變數

### 宣告

請永遠使用 `var` 預先宣告變數，不要使用全域變數。

連續宣告時，可使用逗號來排版，若有需要也可對齊等號（採用 Tab 排版時，第一行請不要對齊）：

``` javascript
var foo    = 'foo',
    sakura = 'samuari',
    olive  = 'peace',
```

### 命名

使用駝峰式命名，但開頭小寫，例如： `fooBar`。

請讓變數命名語意化，易於理解。

``` javascript
var element = document.getElementById('elementId');
```

迴圈中的索引則不在此限，允許使用 `i`, `j`, `k` 作為索引變數。

### 類型檢查

以下提供一些常見的方法來判斷型別。

- 字串: `typeof object === 'string'`
- 數字: `typeof object === 'number'`
- 布林: `typeof object === 'boolean'`
- 物件: `typeof object === 'object'`
- 空白物件: `jQuery.isPlainObject( object )`
- 函式: `jQuery.isFunction( object )`
- 陣列: `jQuery.isArray( object )`
- HTML元素: `object.nodeType`
- null: `object === null`
- null or undefined: `object == null`

**判斷 Undefined:**

- 全域變數: `typeof variable === 'undefined'`
- 區域變數: `variable === undefined`
- 屬性: `object.prop === undefined`

## 字串

- 一般狀況下，總是使用單引號(`'`)而非雙引號(`"`)來定義字串。
- 過長字串應以串接形式宣告 (使用 `+`)，字串串接時，，串接符前後必須空一格。
- 有需要隔斷單字的空白，需要放在每一行字串的尾部，而非下一行字串的開頭。
- 串接運算子應放在每個字串片段的結尾，此時串接符後方不需空格。

``` javascript
var longString = 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. ' +
    'Sed placerat, tellus eget egestas tincidunt, lectus dui ' +
    'sagittis massa, id mollis est tortor a enim. In hac ' +
    'habitasse platea dictumst. Duis erat justo, tincidunt ac ' +
    'enim iaculis, malesuada condimentum mauris. Vestibulum vel ' +
    'cursus mauris.';
```

## 數字

為了閱讀性考量，應使用 `parseInt()` 或 `parseFloat()` 將變數轉為數字，不要使用一元運算子 `+` 來轉換數字。

**錯誤:**

``` javascript
var count = +document.getElementById('inputId').value;
```

**正確:**

``` javascript
var count = parseInt(document.getElementById('inputId').value);
```

## 陣列

使用中括號建立物件，而非 `Array` 建構子

**錯誤:**

``` javascript
var myArr = new Array;
```

**正確:**

``` javascript
var myArr = [];
```

陣列長度未知的狀況下使用 `push()`，來增加新的元素

``` javascript
var myArr = [];
myArr.push('foo');
```

宣告陣列時最後一個元素不需後綴逗號，這會造成陣列長度多了 1 個空值。

**錯誤:**

``` javascript
array = ['foo', 'bar',];
```

**正確:**

``` javascript
array = ['foo', 'bar'];
```

## 物件

一般空物件請使用大括號建立物件，而非以 `Object` 建構子建立

**錯誤:**

``` javascript
var myObj = new Object;
```

**正確:**

``` javascript
var myObj = {};
```

宣告物件並對屬性附值時，其冒號`:`之前不應空格，之後則需要空一格：

``` javascript
var obj = {foo: bar, baz: yoo};
```

使用 `new` 關鍵字建立特定物件時，若沒有參數需求，我們省略括號

**錯誤:**

``` javascript
var myObj = new Object();
```

**正確:**

``` javascript
var myObj = new Object;
```

## 類別

### 靜態宣告

一般來說，沒有複用的需求的物件，我們宣告靜態物件來使用，但宣告時需要避免與全域物件名稱衝突，且必須用匿名函式包裹起來製造 Sandbox 環境。
匿名函式與內容結構之間前後皆空一行。

物件內的方法之間需用逗號分隔，方法與方法之間空一行以區隔開來，逗號緊隨上一個方法的結尾。第一個方法與開頭大括號之間可以空一行。

若要使用 jQuery，在物件外統一使用 `jQuery` ，傳進 Sandbox 之後方可改名為 `$`

一律使用 `use strict` 模式，strict 宣告的後方空一行。

``` javascript
(function ($) {
    "use strict";

    window.MyObject = window.MyObject || {

        foo: function()
        {},

        bar: function()
        {}
    };

})(jQuery);
```

### 類別式宣告

可重複創建實體的物件，我們改用 Prototype 模式來宣告

``` javascript
(function ($) {
    "use strict";

    var MyClass = function(params)
    {
        // Code...
    }

    MyClass.prototype.flower = function ()
    {
        // Code...
    };

    // Push to global
    window.MyClass = window.MyClass || MyClass;

})(jQuery);

// New instance
var newObject = new MyClass(params);
```

## 控制結構

控制結構中，條件式的中括號與關鍵字之間必須間隔一個空格，每個判斷條件之間的邏輯運算子（如 `&&`）前後需要空一格。最先與最後一個判斷條件跟括號之間不需要空格。
而判斷式內如果用到比較運算子（如 `<`, `==`），則運算子前後也需要空格。

控制結構所包裹的區塊空間，其開頭大括號不換行，結束括號需自己在新的一行。

範例：

``` javascript
if (foo == 'bar' && baz < 5) {
    // Code...
}
```

過長的條件式可以換行，換行後內縮一次，邏輯運算子請放在換行後的開頭

``` javascript
if (a == b && c == d && e == f
    && g == h && j == k) {
    // Code...
}
```

若有多層控制結構，每一層的關鍵字與上一層結束括號需換行，每一層開頭括號不需獨立換行：

``` javascript
if (a == b) {
    // Code...
}
else if (c == d) {
    // Code...
} 
else {
    // Code...
}
```

請勿省略括號

``` javascript
// 錯誤範例
if (a == b) alert(1);
```

### for 範例

``` javascript
for (i = 0; i < 5; i++) {
    // Code...
}

for (i in foo) {
    // Code...
}
```

### Try catch 範例

``` javasript
try {
    // Code...
}
catch (err) {
    console.log(err);
}
```

### While 範例

``` javascript
while (flower) {
    // Code...
}

// Do While
do {
    // Code...
} while (flower)
```

### Switch 範例

Case 與判斷內容間空一格，判斷式與後方冒號之間不空格。

Case 內容程式碼與 break 皆內縮一排。

``` javascript
switch (foo) {
    case 'bar':
        // Code...
        break;

    case 'baz':
        // Code...
        break;

    default:
        // Code...
}
```

## 三元運算子

在下列情況使用三元運算子:

- 只有一個條件
- 程式區塊內只包含一個操作時

``` javascript
lyraRocks ? 'This is true' : 'else it is false';
```

否則就使用標準語法

``` javascript
if (condition) {
	// statements
}
else {
	// statements
}
```

## 函式

### 函式宣告

函式宣告時，函式名稱或 `function` 關鍵字與參數區塊開頭括號之間不空格。參數區塊內與開頭結尾參數之間無空格。多個參數時，參數之間用逗號隔開，
逗號前方不空格，後方空一格。

包裹函式邏輯區塊的大括號，開頭結尾皆需獨立換行。

採用變數宣告時，結尾必須後綴分號，一般宣告時則不需要分號。

範例：

``` javascript
// 一般宣告
function foo(bar, baz) {
    // Code...
}

// 變數宣告
var flower = function(sakura, olive) {
    // Code...
};
```

參數過多時，可換行並內縮一排，逗號在前一排結尾

``` javascript
var flower = function(sakura, olive, sunflower,
    rose, orange) {
    // Code...
};
```

### 函式呼叫

直接呼叫時，括號與函式名稱之間不空格

``` javascript
flower('sakura');
```

鏈狀呼叫時，若過長可考慮換行：

``` javascript
$('.someElement')
	.hide()
	.delay(1000)
	.fadeIn();
```

## 註解

**單行**

- 註解放置在程式碼上方
- 雙斜線與註解之間應空一格

```
// I am a single line comment.
```

**多行註解**

- 多行註解應放置在說明的目標程式碼上方
- 註解起始字元 `/*` 應放在第一行註解上方，註解結束字元 `*/` 應放在最後一行註解下方，
- 每一行註解應加上 1 個星號後綴一個空白

```
/*
 * I am a multiline comment.
 * Line two
 * Line three
 */
```

## 區塊註解 (Docblock)

- 區塊註解應用於描述函式、物件與變數時。
- 註解起始字元應有兩個星號 `/**` 並放在第一行註解上方，註解結束字元 `*/` 應放在最後一行註解下方。
- 每一行註解應加上 1 個星號，星號前後各加綴一個空白
- 註解最少應包含敘述、參數與回傳值三種內容並依照這個順序排列。若該函式無參數，則可以省略 `@params`。
- 敘述與其他內容之間應該要有一空行

範例：

``` javascript
/**
 * This is my function.
 *
 * @params  {HtmlElement}  html  A html element object.
 *
 * @return  {Object}
 */
```

詳細 JS Docblock 內容目前以 PHPStorm 自動產生的樣式為主，具體格式可約略參考 [YUI Doc](http://yui.github.io/yuidoc/syntax/index.html)，但我們的格式會要求排版與對齊。
