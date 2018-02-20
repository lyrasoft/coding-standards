# LYRASOFT CSS 編碼規範 beta

> 這份 CSS 規範尚在 beta 中，因此有部分規則我們尚未完全導入到專案內，但足以作為討論時的參考

## 主要原則

### Good CSS Architecture

- 可預測 Predictable
- 可重複使用 Reusable
- 考維護性 Maintainable
- 可擴充 Scalable

See [CSS Architecture](http://philipwalton.com/articles/css-architecture/) by Philip Walton

## 語法與格式

### 樣式屬性格式

每一個屬性都應該存在獨立的一行，並縮排一個層級。冒號之前不應該有空白字元，但須後綴一空白字元。所有的樣式屬性須以分號 (;) 作為結尾。

```css
/* 正確 */
.example {
    background: black;
    color: #fff;
}

/* 錯誤 - 冒號後面沒有後綴空白 */
.example {
    background:black;
    color:#fff;
}

/* 錯誤 - 沒有以分號作為結尾 */
.example {
    background: black;
    color: #fff
}
```

### 縮排

以4個空白(spaces)作為縮排。

```css
/* 正確 */
.example {
    color: #000;
    visibility: hidden;
}

/* 錯誤 - 寫在同一行 */
.example {color: #000; visibility: hidden;}
```

### 巢狀結構

LESS/SCSS 等等的 Pre-compiled CSS 允許巢狀結構，在巢狀結構中，其子選擇器還有樣式規則都應該縮排 (4 spaces)。巢狀結構以一行空行與上層結構作為間隔。

```css
/* 正確 */
.example {

    > li {
        float: none;

        + li {
        	margin-top: 2px;
        	margin-left: 0;
        }
    }
}
```

### 對齊

#### 選擇器及宣告

左大括號必須跟選擇器位於同一行，並前綴一空白字元。右大括號須存在於最後一個屬性規則之後並獨立於新的一行，且應與左大括號處於相同的縮排。

```css
/* 正確 */
.example {
    color: #fff;
}

/* 錯誤 - 右大括號的位置錯了，沒有正確縮排 */
.example {
    color: #fff;
    }
```

各個選擇器皆獨立在自己的一行，最後一個選擇器與左大括號位於同一行：

```css
.foo, 
.foo--bar,
.baz {
    display: block;
}
```

#### 樣式宣告對齊

同類型或類似的樣式宣告，建議使用空白來縮排對齊 (非強制)，例如：

```css
.foo {
    -webkit-border-radius: 3px;
       -moz-border-radius: 3px;
            border-radius: 3px;
}

.bar {
    position: absolute;
    top:    0;
    right:  0;
    bottom: 0;
    left:   0;
    margin-right: -10px;
    margin-left:  -10px;
    padding-right: 10px;
    padding-left:  10px;
}
```

### HEX 值

HEX 值應該使用小寫並以最小縮寫宣告：

```css
/* 正確 */
.example {
    color: #eee;
}

/* 錯誤 */
.example {
    color: #EEEEEE;
}
```

### HTML 屬性選擇器

屬性選擇器須以雙引號包含。

```css
/* 正確 */
input[type="button"] {
    ...
}

/* 錯誤 - 沒有雙引號 */
input[type=button] {
    ...
}

/* 錯誤 - 使用單引號 */
input[type='button'] {
    ...
}
```

### 零值的單位

零值不應該使用單位。

```css
/* 正確 */
.example {
   padding: 0;
}

/* 錯誤 - 使用單位 */
.example {
   padding: 0px;
}
```

## 註解

由於我們專案數量龐大，詳細的註解有助於在不同的專案間快速切換與理解。加上因為 CSS 語言特性的關係，CSS 有許多情況是無法立即被瞭解的，舉例來說：

- 當你的這段 CSS 程式碼相依於其他 CSS 的時候;
- 當你改變這段 CSS 會在何處照成影響;
- 這段 CSS 還被使用在什麼地方;
- 作者希望這段 CSS

總結來說，你必須對那些無法從該程式碼片段中一眼就瞭解的事。舉例來說，你不需要針對 `color: red;` 作註解。

### 主要段落

主要的程式碼段落必須以完整的註解區塊作為開頭，例如：

```css
/* ==========================================================================
   PRIMARY NAVIGATION
   ========================================================================== */
```

### 次要段落

次要段落須以開放式的註解區塊為開頭：

```css
/* Mobile navigation
   -------------------------------------------------------------------------- */
```

### 長篇幅文字註解
```css
/**
 * 長篇幅的文字是用來註解不能簡單被瞭解的程式碼片段：如不同的狀態，排列組合，使用條件，使用方式，使用時機等等。
 *
 * @tag This is a tag named 'tag'
 *
 * TODO : 這是一個註解區塊中 todo 的範例，描述之後需要被完成的項目。
 *   一行應小於 80 個字元，換行之後須以兩個空白作為縮排開頭
 */
```

如 [CSS Wizardry](http://csswizardry.com/) 裡的範例：

```css
/**
 * The site's main page-head can have two different states:
 *
 * 1) Regular page-head with no backgrounds or extra treatments; it just
 *    contains the logo and nav.
 * 2) A masthead that has a fluid-height (becoming fixed after a certain point)
 *    which has a large background image, and some supporting text.
 *
 * The regular page-head is incredibly simple, but the masthead version has some
 * slightly intermingled dependency with the wrapper that lives inside it.
 */
 ```

### Extend Style

不同的 CSS 元件可能被放在不同的檔案裡，你可能會遇到此某兩個 component 有互相關聯的情形。如，某一個 button 的 bass class 在另一
個檔案中被新的 button 所擴充加上新的 style，須在兩邊檔案都寫上註解。如下：

在 Base Object 的檔案中：
```css
/**
 * Extend `.btn {}` in _components.buttons.scss.
 */

.btn {}
```

在 擴充的主題檔案中：
```css
/**
 * These rules extend `.btn {}` in _objects.buttons.scss.
 */

.btn--positive {}

.btn--negative {}
```

命名規則
-----------------
好的命名規則，可以幫助你和你的團隊快速瞭解 [[3]]：

- 這個 class 在做什麼事
- 可以在什麼情境使用此 class
- 這個 class 可能跟什麼有關聯
