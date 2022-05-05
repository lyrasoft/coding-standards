# LYRASOFT CSS/SCSS 編碼規範

自 2018 起，LYRASOFT 轉用 SCSS 取代 LESS。相關編碼風格也遵循 [AirBnb CSS/Sass Styleguide](https://github.com/airbnb/css)

> 中文 https://github.com/ArvinH/css-style-guide

以下則針對 Airbnb 規範補充更多額外建議。由於 AirBnb CSS/Sass Styleguide 後半部大多為建議形式，並非強制規範，我們的補充內容，
有部分會與其衝突。

## 語法與格式

### 縮排

自 2018 年開始，統一 CSS, SASS, LESS 所有檔案之縮排為 2 spaces。

但寫在 HTML 或 PHP 內的可接受 4 spaces。 (因我們的 HTML / PHP 依然使用 4 spaces)

### 規則宣告

每個規則宣告都必須獨自一行。

```css
/* 正確 */
.example {
  color: #000;
  visibility: hidden;
}

/* 錯誤 - 寫在同一行 */
.example {color: #000; visibility: hidden;}
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
.foo-bar,
.baz {
  display: block;
}
```

#### 前綴字對齊

跨瀏覽器引擎的的樣式宣告，建議使用空白來縮排對齊 (非強制)，例如：

```css
.foo {
  -webkit-border-radius: 3px;
     -moz-border-radius: 3px;
          border-radius: 3px;
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

## 命名模式

Airbnb 中建議命名規範可採用 BEM 或 OOCSS，其在實作上的差別可能是這樣，以 `card` 來舉例:

BEM 風格

```css
.listing-card { }
.listing-card--blue { }
.listing-card__title { }
.listing-card__body { }
```

OOCSS 風格

```css
.listing-card { }
.listing-card.card--blue { }
.listing-card .card-title { }
.listing-card .card-body { }
```

目前我們在編寫時，建議以 BEM 為主。

## SCSS 風格

基本 SCSS 規範遵循 Airbnb 建議，不過我們可以允許三層以上的嵌套。由於我們以開發網站為主，並非開發重用的含式庫，多數大型專案的 HTML 將極其複雜且深入，三層嵌套在 OOCSS/BEM 風格下不足以使用，容易讓網站元素之間相互影響。

以下為 Bootstrap 4 的範例程式碼，某些複雜的多層次元件(例如下拉選單)需要用到多層的子代選擇器(` > `)來確保提取到正確的元素。

```scss
.navbar-expand {
  @each $breakpoint in map-keys($grid-breakpoints) {

    &#{$infix} {
      // ...

      @include media-breakpoint-up($next) {
        .navbar-nav {
          flex-direction: row;

          .dropdown-menu-right {
            right: 0;
            left: auto; // Reset the default from `.dropdown-menu`
          }
        }

        // For nesting containers, have to redeclare for alignment purposes
        > .container,
        > .container-fluid {
          flex-wrap: nowrap;
        }

        // ...
      }
    }
  }
}
```

但多數一般元件都可控制在 5 層以內，盡可能優化你的層次與結構讓代碼易讀。可以考慮以下規則 :

第一層為元件入口選擇器，我們把一整個需要獨立風格的區塊稱為元件 (component)，所有它的樣式都要被包在裡面:

```scss
header.main-header {
  // ...
}

section.feature {
  // ...
}
```

元件入口內的 class 與元素選擇器最高三層

```scss
header.main-header {
  ul.dropdown-menu {
    > li {
      > a {
        // ...
      }
    }
  }
}
```

後續的選擇器僅使用偽類選擇器 (`::`) 或 `&`選擇器來更改不同狀態下的樣式。

```scss
header.main-header {
  ul.dropdown-menu {
    > li {
      > a {
        &::before {
          // ...
        }

        &:hover {
          // ...
        }

        &.active {
          // ...
        }
      }
    }
  }
}
```

但若既有的 class 有根據層次變動，則可有效的降低必要層次，這是 Bootstrap 4 以後所提供的較佳結構。

```scss
header.main-header {
  .navbar-nav a.nav-link {
    // 主選單的項目
  }

  a.dropdown-item {
    // 子選單的項目
  }
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
 *  contains the logo and nav.
 * 2) A masthead that has a fluid-height (becoming fixed after a certain point)
 *  which has a large background image, and some supporting text.
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
