# 中文 CSS 排版原則指南

由於網路上的 CSS 排版探討大多著重於主流的西方拉丁語系文字，其在文字大小、行距等各方面並不完全適用中文文字。
故本文件提供一個易讀且清晰的常用中文網頁排版指南，供網頁設計師參考。

**目錄**

<!-- TOC -->

- [1. 注意事項](#1-注意事項)
- [2. 字體大小](#2-字體大小)
    - [2.1. 基礎 Body 字體大小](#21-基礎-body-字體大小)
    - [2.2. 標題字體大小](#22-標題字體大小)
    - [2.3. 選單、Label、Badge 等字體大小](#23-選單labelbadge-等字體大小)
- [3. 字型](#3-字型)
    - [3.1. 基本常用字體](#31-基本常用字體)
    - [3.2. 標題字體](#32-標題字體)
    - [3.3. 按鈕與 UI 元素](#33-按鈕與-ui-元素)
    - [3.4. LESS 編寫建議](#34-less-編寫建議)
- [4. 字重](#4-字重)
    - [4.1. Windows 注意事項](#41-windows-注意事項)
- [5. 行距](#5-行距)
    - [5.1. Feature 介紹行距](#51-feature-介紹行距)
    - [5.2. 標題行距](#52-標題行距)
- [6. 字距](#6-字距)
    - [6.1. 標題字距](#61-標題字距)
- [7. 內文排版](#7-內文排版)
    - [7.1. 字體大小](#71-字體大小)
    - [7.2. 間距](#72-間距)
    - [7.3. 行距](#73-行距)
    - [7.4. 顏色](#74-顏色)
    - [7.5. 樣式](#75-樣式)
- [8. 範例程式](#8-範例程式)
- [9. 其他](#9-其他)
    - [9.1. 參見](#91-參見)
    - [9.2. 目前在使用這個排版風格的網站](#92-目前在使用這個排版風格的網站)

<!-- /TOC -->

## 1. 注意事項

本文件僅提供 HTML/CSS 統一化排版樣式之標準建議，並不規範 HTML 結構，中英文之間是否空格等細部規定。

另外考量到我們常常是以 Bootstrap 佈景為基礎開發網站，本文件著重在如何將以有的佈景調整成合適的樣式。

## 2. 字體大小

### 2.1. 基礎 Body 字體大小

由於近年來 Full HD 螢幕逐漸普及，以 1080p 觀看網站的使用者越來越多，網站的字體大小建議以 `html, body { font-size: 16px }` 作為基準值。

> 目前網路上最普遍使用的 Bootstrap 4 代預設採用 16px，3 代則是 14px ，如果您使用的是 3 代，建議自行覆蓋 body 的字體大小。

### 2.2. 標題字體大小

當網站套用模板時，或者設計師獨立設計網站版面時，可能對標題大小有自己的想法，因此本文件不規範全域的標題大小。但另外在文章字體中有所建議。

> Bootstrap 3 中文標題大小是用 px 寫死的，建議設計師還是要自己把 h1 ~ h6 的大小重設一遍

### 2.3. 選單、Label、Badge 等字體大小

通常選單與特定 UI 元素的字體會稍小。一般來說建議以 Body 字體的百分比約略縮小，當有必要調整基礎字體大小時，所有文字都會根據比例一起變動。

若設計師考量到某些元素內的字體變更大小會影響到排版，也可以用像素 `px` 當作單位。

## 3. 字型

字型部分，將以未載入 Webfont 作為基準，提供適用於 Windows & Mac 作業系統的安全字體建議。

### 3.1. 基本常用字體

在 Mac 中，目前最常用的中文字體為**蘋方TC**，再來是**黑體TC**，要注意的是 Mac 上的 CSS 通常必須要把字體名稱寫成英文，所以會是 `PingFang TC` 與 `Heiti TC`。 (你想支援更古老的 Mac 別忘了 **LiHei Pro**)

Windows 上不意外的萬年**微軟正黑體**，你不用特別設定新細明體，因為當所有字體都抓不到時，最後就是回到新細明體來顯示。 (Windows 上字型必須用中文名稱)

當然別忘記預設的 `sans-serif` 在最後面。

故以下是我們常用的字體順序:

```css
font-family: '{主要的英文字型}', 'PingFang TC', 'Heiti TC', '微軟正黑體', sans-serif;
```

開頭的英文字型可以任意搭配，或是用 Google Webfont 皆可。

若要適應大陸網站，則建議加上雅黑體:

```css
font-family: '{主要的英文字型}', 'PingFang TC', 'Heiti TC', 'Microsoft JhengHei', 'Microsoft YaHei', sans-serif;
```

### 3.2. 標題字體

標題字體在 Mac 上可以加上例如**蘭亭黑體** `LantingHei TC` 之類的額外選擇。由於中文的標題與內文區隔較不明顯，建議在中文網頁上，通常要選比較粗的字體。若字型一樣，則嘗試加粗。

在 Windows 上沒有太多選擇，依然是微軟正黑體然後加粗。不過可以考慮採用一個蠻好看的字體: **Adobe繁黑體**，在 Windows 上顯示的平滑效果不輸給 Mac，缺點是有安裝 Adobe 全套的使用者才看的到。**(注意 Adobe 繁黑體 在 IE 顯示上 Baseline 會跑掉)**

### 3.3. 按鈕與 UI 元素

某些布景內的按鈕、Label 等許多 UI 元素都有另外寫字體樣式，要記得除了 `html, body` 以外也要替這些 UI 元素設定好字體以統一風格。

### 3.4. LESS 編寫建議

若全站字體變動不大，建議在 LESS 或 SCSS 的開頭就寫好相關變數，之後網站內只要套用變數即可。

```less
$main-font: '{主要的英文字型}', 'PingFang TC', 'Heiti TC', '微軟正黑體', sans-serif;
$title-font: '{標題的英文字型}', 'PingFang TC', 'Heiti TC', 'Adobe 繁黑體 Std', 'AdobeFanHeitiStd-Bold', '微軟正黑體', sans-serif;
```

使用方式

```less
.article-content .btn {
    font-family: $main-font;
}
```

## 4. 字重

原則上維持預設即可，設計師可根據設計需求調整字重。

### 4.1. Windows 注意事項

Windows 10 以後，字重 300 以下的字體會出現渲染模糊，應確認設定成 400 以上。

下圖是字重 300

![](https://i.imgur.com/NZQEv12.jpg)

下圖是字重 400

![](https://i.imgur.com/eAexMc7.jpg)

會明顯看到 300 的文字難以閱讀。

## 5. 行距

Bootstrap 預設行距較適合英文，對中文來說太窄。中文文字行距建議設為 `150%` 到 `175%`，根據設計所需要的空間感來定。 (可用 `1.5` 來表示)

下圖為字距範例圖片:

![](https://i.imgur.com/WtsvMvF.jpg)

### 5.1. Feature 介紹行距

首頁或是 Landing Page 的介紹文字，則可以採用 `175%` 到 `200%` 行距，建立空間感。

![](https://i.imgur.com/Pbn0O6r.jpg)

### 5.2. 標題行距

採用 Bootstrap 時，其沒有替 h1~h6 設定基本的行距，若碰到兩行標題時會黏在一起。可考慮預設為 1.3-1.5 之間的行距。 

另外，主副標與內文之間，各自之間應該要有不同的間距，建立層次感。

下圖是沒有層次感的範例，間距都一樣

![](https://i.imgur.com/1QXIRbr.jpg)

下圖則是建立起標題之間的層次差距

![](https://i.imgur.com/jceUS8k.jpg)

## 6. 字距

原則上基本字距都應該訂為 0，網站內的文字主要作為閱讀使用，無論中文還是英文，多餘的字距將會造成閱讀困難。

### 6.1. 標題字距

某些設計裝飾用的標題字體可考慮單獨加上字距，由設計師自行考量

以下是拉長字距的標題案例

![](https://i.imgur.com/NknyAE3.jpg)

## 7. 內文排版

內文排版章節主要規範一般部落格文章或網站靜態文章中，含有大量文字、標題與段落的樣式，可以與網站的其他區塊排版風格分開。內文排版主要以閱讀舒適度為最高優先，

### 7.1. 字體大小

可考慮 `16px` ~ `18px` 以上，較大的字體大小有助於長時間閱讀大量文字。

標題部分，由於 Bootstrap 皆把標題大小寫死，建議另行用 `em` 做調整。並將 h2 ~ h6 調大，不然 h6 會小於一般字體大小。

```less
h1 {
  font-size: 2.5rem;
}

h2 {
  font-size: 2rem;
}

h3 {
  font-size: 1.7rem;
}

h4 {
  font-size: 1.5rem;
}

h5 {
  font-size: 1.2rem;
}

h6 {
  font-size: 1rem;
}
```

別忘了標題最好加字重，否則中文的標題到 h4 以下就跟內文分不出來了。

### 7.2. 間距

建議 `<p>` 元素的上下 `margin` 皆設 `1em`，明確區隔段落。

標題可上寬下窄，上方 `1em`，下方 `0.6em` ~ `0.8em`，可建立出層次感。設計感較重的網站，上下方可以從 `1.3em` 以上起跳。

### 7.3. 行距

行距同樣維持 150% ~ 175% 擁有優良的閱讀感受。

### 7.4. 顏色

若網站採用較淡的灰黑色當作主要字體顏色，內文切記要加深，建議至少要比 `#666` 來得更深，閱讀較不吃力。

### 7.5. 樣式

在 LESS 編寫時，建議將內文專用樣式用一個 class 包起來，與網站的其他區域做區隔。我們較常用的是 `articlt-content` 這個 class。

以下的範例可以直接複製到專案中使用:

```less
.article-content {
    color: #666;
    line-height: 1.7;
    font-size: 18px;

    h1, h2, h3, h4, h5, h6 {
        margin-top: 1em;
        margin-bottom: .6em;
    }

    h1 {
        font-size: 2.5rem;
    }

    h2 {
        font-size: 2rem;
    }

    h3 {
        font-size: 1.7rem;
    }

    h4 {
        font-size: 1.5rem;
    }

    h5 {
        font-size: 1.2rem;
    }

    h6 {
        font-size: 1rem;
    }
    
    p {
      margin-top: 1em;
      margin-bottom: 1em;
    }

    hr {
        margin-top: 50px;
        margin-bottom: 50px;
    }

    pre {
        margin-top: 35px;
        margin-bottom: 35px;
    }
}
```

## 8. 範例程式

本文的排版範例可以在這裡看到 DEMO: https://jsfiddle.net/asika32764/L3dx987L/

以下的 LESS / SCSS 程式碼可以直接 Copy 到專案中使用 (將不定期更新)

```less
$main-font: 'Helvetica Neue', 'PingFang TC', 'Heiti TC', '微軟正黑體', sans-serif;
$title-font: 'Helvetica Neue', 'PingFang TC', 'Heiti TC', 'Adobe 繁黑體 Std', 'AdobeFanHeitiStd-Bold', '微軟正黑體', sans-serif;

html, body {
  font-size: 16px;
  font-family: $main-font;
  line-height: 1.75;
}

h1, h2, h3, h4, h5, h6 {
  font-weight: 600;
  font-family: $title-font;
  line-height: 1.5;
}

.article-content {
    color: #666;
    line-height: 1.75;
    font-size: 18px;

    h1, h2, h3, h4, h5, h6 {
        margin-top: 1em;
        margin-bottom: .6em;
    }

    h1 {
        font-size: 2.5rem;
    }

    h2 {
        font-size: 2rem;
    }

    h3 {
        font-size: 1.7rem;
    }

    h4 {
        font-size: 1.5rem;
    }

    h5 {
        font-size: 1.2rem;
    }

    h6 {
        font-size: 1rem;
    }
    
    p {
        margin-top: 1em;
        margin-bottom: 1em;
    }

    hr {
        margin-top: 50px;
        margin-bottom: 50px;
    }

    pre {
        margin-top: 35px;
        margin-bottom: 35px;
    }
}

```

## 9. 其他

### 9.1. 參見

- [設計中怎麼處理好中文排版](http://www.uiux.cn/design/128.html)
- [幾個中文排版訣竅，有效改善閱讀體驗](https://www.slideshare.net/bobby3302/ss-67861880)
- [漢字標準格式 CSS 框架](https://css.hanzi.co/)
- [簡單做好中文排版：十項讓長文章更容易閱讀的原則](https://www.beforafter.org/blog/ten-rules-that-make-articles-better-understood)

### 9.2. 目前在使用這個排版風格的網站

- [LYRASOFT](https://lyrasoft.net/)
- [飛鳥學院](http://asukademy.com/)
- [夏木樂網站知識](https://simular.co/knowledge/)
- [AddMaker](https://addmaker.tw/)
- [AnimApp 動畫社群](http://animapp.tw/)
- [台灣美語通](https://www.english4tw.com/)
- [Datavideo Virtualset](http://www.datavideovirtualset.com/tw)
- [iHealth](http://www.ihealth.com.tw/)
- [澎坊](https://www.profond.com.tw/)
