# LYRASOFT Javascript 編碼規範

自 2018 年開始，LYRASOFT 以 [AirBnb Javascript Style](https://github.com/airbnb/javascript) 為主要開發規範。並逐步採用 ES2015 + ES7 stage-2 開發環境。

> 中文請參考 https://github.com/jigsawye/javascript

本文件主要針對 AirBnb 標準作額外風格建議

## 程式縮排

可允許 HTML 內的 inline JS 使用 4 spaces， LYRASOFT 的 PHP 規範以 4 spaces 為縮排，當 JS 不得已必須寫在 PHP 檔案內時則不在此限，不過應避免混用的情形發生。

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

    var MyClass = function(params) {
        // Code...
    }

    MyClass.prototype.flower = function () {
        // Code...
    };

    // Push to global
    window.MyClass = window.MyClass || MyClass;

})(jQuery);

// New instance
var newObject = new MyClass(params);
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
