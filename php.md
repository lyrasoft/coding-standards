# LYRASOFT PHP 編碼規範

LYRASOFT 採用 [PSR-12](https://www.php-fig.org/psr/psr-12/) 標準作為主要開發風格。

> 中文請參考 [PSR-12 中文翻譯](https://github.com/memochou1993/PSR/blob/master/PSR-12.md)

另外，本文件後續將補充一些 PSR-12 中沒有規範，但我們另行規範的風格，所有未列出或出現衝突的規範皆以 PSR-12 為準。

## 程式寫作規範

### 引入程式

當手動直接載入程式檔案時，請用 `require` 與 `require_once`，若採用程式動態載入（例如使用 `Factory` 模式），則使用 `include` 與 `include_once`。

引入程式請以關鍵字寫法，不要用函式寫法。

正確：

``` php
require_once 'foo.php';
```

不應使用括號：

``` php
require_once('foo.php');
```

一般來說，我們應該盡量用絕對路徑，搭配 Joomla! 常數來引入檔案，避免位置變更時出錯：

``` php
require_once JPATH_LIBRARIES . '/foo/bar.php';
```

若要採用相對路徑，也應該使用 `__DIR__` 來當做路徑基礎：

``` php
require_once __DIR__ . '/../bar.php';
```

## 程式排版

### 換行

#### 控制結構

所有控制結構 `if`, `for`, `foreach` 等等，皆須在前後增加一行空行。

```php
$foo = 'bar';

if ($foo) {
    //
}

$baz = 123;
```

#### `return` 前空行

所有的 `return` 關鍵字上方必要空一行。

```php
$result = $db->loadObject();

return $result;
```

#### 程式邏輯運作過程

編碼過程，建議每一行不同的邏輯控制皆空行，並且使用括號區隔的 Scope 空間上下也需要空行。

若連續變數賦值則不需空行。

範例：

``` php
if ($avoid) {
    $filter = $this->getState('filter', array());
    $search = $this->getState('search');
    $data   = $this->getState('data', array());
    $table  = $this->getTable();

    $table->bind($data);

    $table->check();

    $table->store();
}
```

## 參照

當使用變數參照時，參照符號應該距離等號一個空白字元，且跟後面的變數緊鄰，沒有空白。

範例:

```php
$ref1 = &$this->sql;
```

## 空白間距

### 字串連接

當使用字串連接時， `.` 符號的左右兩旁應該要有一格空白。例如：

``` php
$id = 1;
echo 'index.php?option=com_foo&task=foo.edit&id=' . (int) $id;
```

多行字串，串接符號應該要在下一行的開頭，左方除了縮排以外不需額外的空格：

``` php
$id = 1
echo 'index.php?option=com_foo&task=foo.edit&id=' . (int) $id
    . '&layout=special';
```

### 型別轉換

使用 `()` 語法進行型別轉換時一定要在後方加上一個空白

```php
$sql = (string) $query;
```

### 設定值或運算常數

某些演算法，或固定數值的參數，我們不應該直接將數值寫死在運算中，例如：

``` php
foreach ($array as $k => $value)
{
    if ($k == 3)
    {
        break;
    }

    // Do some stuff....
}
```

這會讓使用者混淆，不知道 `3` 這個數字是怎麼來的，當多組數字共存時，會造成日後修改參數的麻煩。我們應該預先宣告這些數字，並給予語意化的名稱：

``` php
$limit = 3;

foreach ($array as $k => $value)
{
    if ($k == $limit)
    {
        break;
    }

    // Do some stuff....
}
```

或是建立設定擋來方便修改：

``` php
class ImageThumb
{
    protected $width;

    protected $height;

    public function __construct(JRegistry $config)
    {
        $this->width  = $config->get('thumb.width', 640);
        $this->height = $config->get('thumb.height', 480);
    }
}
```

## 判斷語法

### 強行別判斷

只要能確保型別不是變動的，我們皆以三等號 (`===` or `!==`) 來判斷，對效能有幫助，除非幫下判斷變數可能存在
型別的變動(例如 `'1'` 與 `1`)，才使用 `==` 或 `!=`。

可以用 `$var === null` 取代 `is_null()`，效能較好。

確定是陣列時，可以用 `$var === []` 取代 `!count($var)` 或 `empty($var)` 來判斷空陣列 ，效能也較好。

其餘未列出的使用情境皆可用此規則來判斷。

### 存在判斷

當我們為了避免因變數或陣列元素未宣告而造成 Notice 時，可考慮用專用的判斷函式在 if 中。

#### isset()

用在判斷變數或陣列是否宣告過，且值不為 `null`：

``` php
if (isset($array['eggs']) && $array['eggs'] == 5)
{
}
```

#### empty()

用在判斷變數或陣列元素是否被宣告過且值不為否（含 `null`, `false`, `0`, `''`, `array()`）等，一般作為布林值判斷，且內容是動態的，可能不曾被初始化時才用：

``` php
if (!empty($array['online']))
{
}
```

注意 `isset()` 在變數為 `null` 時會回傳否，因此判斷物件屬性是否被宣告過，應該用 `property_exists()`。

#### 簡單易懂

避免多重否定寫法混淆使用者：

``` php
if (!$allowNull && !(($value !== null) && ($value !== '')))
{
}
```

## 例外處理

例外處理應當只用於錯誤處理上。

下面的章節將指出如何照語意使用 [PHP 標準函式庫 (SPL) 的 Exceptions](http://php.net/manual/en/spl.exceptions.php)。

### 邏輯例外 LogicException

在 API 的使用方式上發生的明確問題會丟出 LogicException 的例外。例如：如果有個依賴參數失效時（你試圖操作一個未被載入的物件）。

下方的子類別也可被用於適當的情境下：

#### BadFunctionCallException

如果一個 callback 參照到一個未定義的函式(function) 或是缺少了一些參數時可以丟出該例外。例如：如果 `is_callable()` 或其他類似的函式用在一個函式(function)上時得到失敗的結果。

#### BadMethodCallException

如果一個 callback 參照到一個未定義的方法(method) 或是缺少了一些參數時可以丟出該例外。例如：如果 `is_callable()` 或其他類似的函式用在一個方法(method)上時得到失敗的結果。 另一個例子可能是找不到某些使用魔術方法的參數。

#### InvalidArgumentException

當有不合規格的輸入發生時丟出該例外。

#### DomainException

該例外和 InvalidArgumentException 類似，但會在一數值未依附在一已定義的有效資料群集的情形下丟出該例外。例如：試圖載入一個"mongodb"的資料庫引擎，但該引擎尚未在 API 中實作。

#### LengthException

當作一參數的長度檢查失敗時丟出該例外。例如：一個檔案的雜湊值不是一個指定長度的字串。

#### OutOfRangeException

該例外較少被實際的應用，但可在存取一個非法的索引值時丟出該例外。

### 執行期例外 RuntimeException

當外部實體或環境造成你無法控制的問題因而產生錯誤時，便丟出 RuntimeException 的例外。當一個錯誤的產生不能明確地被判斷出來時，預設丟出該例外。例如：你試圖連到資料庫但資料庫卻無法使用 (伺服器掛掉之類的)。另一個例子可能為 SQL 查詢語句的錯誤。

#### UnexpectedValueException

當一個非預期的結果發生時應當丟出該種類的例外。例如：當一個函式預期是要回傳布林值卻回傳字串時。

#### OutOfBoundsException

該例外較少實際的應用，但也許當某個值不為合法的索引值時丟出該例外。

#### OverflowException

該例外較少實際的應用，但也許當你加入一個元素到已經滿載的容器物件時丟出該例外。

#### RangeException

該例外較少實際的應用，但也許在程式執行期間丟出該例外以指出一個範圍的錯誤。通常這是指除了運算虧位/溢位之外的錯誤。該例外為 DomainException 的 runtime 版本。

#### UnderflowException

該例外較少實際的應用，但也許當你從一個空的容器物件中移除一個元素時丟出該例外。

### 例外處理的文件撰寫

每個函式(function)或方法(method)必須要註解使用到的例外，用 @throws 的標籤和丟出例外的物件名稱作註解。每種例外只須備被註解一次。註解的說明非必須。

> 對於每一種例外的使用方式，請參考此文章: [PHP Exceptions 種類與使用情境說明](http://asika.windspeaker.co/post/3503-php-exceptions)

## 註解

行內註解可用 C 語言風格的 `/* ... */` 或 C++ 的單行註解 `// ...`。

C 風格的註解通常被用在文件開頭、類別、函式等等的文件標頭。而 C++ 風格單行註解則常用在程式解釋與提醒。註解對於幫助其他開發人員理解程式碼的目的有非常大的幫助，甚至包含您自己也能受惠。當程式碼開始進行複雜操作時，永遠記得要加上註解。

至於 Perl 與 Shell 的井號 (`#`)註解則不建議使用，目前在 PHP 中也不再允許此類註解。

## 區塊註解 （Docblock）

所有 php 檔案文件、函式、類別與方法「一定」需要加註 Docblock。

### 文件表頭

採用 LYRASOFT PHPStorm File-Template 內的預設格式：

``` php
<?php
/**
 * Part of {PROJECT_NAME} project.
 *
 * @copyright  Copyright (C) 2011 - 2015 {ORGANIZATION}, Inc. All rights reserved.
 * @license    GNU General Public License version 2 or later; see LICENSE
 */

```

並根據需要修改 `PROJECT_NAME`、`ORGANIZATION` 與年份。

### 類別區塊註解

包含類別說明與起始版本，版本的 `DEPLOY_VERSION` ，在正式發佈時由 Script 批次取代。範例：

``` php
/**
 * Description of this class.
 *
 * @since  {DEPLOY_VERSION}
 */
class Windwalker
{
    // ...
}
```

所有標籤標題與內容的距離最小以兩格以上為主（非強制），可依需求自行加上：

- `@link`  參照連結
- `@note`  注意事項
- `@see`   參考資料
- `@deprecated` 被棄用

等常用 phpDoc 標準標籤。

### 函式與方法區塊註解

函式定義需開始並獨立於一新行，開始與結束的括號也需分別被放置於一新行。在負責處理回傳值的程式碼前應插入一空行。

函數定義必須包含註解說明，根據本文件中的註解章節所規範之規則予以撰寫。

-   簡易說明 (必要，結束需空一行)
-   完整說明 (選填，結束需空一行)
-   `@param` (若函式有參數則此欄位為必要，並於最後一個`@param`標籤之後新增一空行)
-   `@return` (必要，結束需空一行，就算無回傳值也要補上 void)
-   `@throws` 有要丟出的例外 （IDE 會自動補上）
-   其餘標籤依據字母順序排列。

```php
/**
 * An utility method.
 *
 * @param   string  $path  The library path in dot notation.
 *
 * @return  void
 *
 * @since   1.0
 */
public function bloom($path)
{
    // Body of method.
}
```

### 屬性區塊註解

類別屬性註解區塊包含以下必要及非必要欄位，並依照下列順序排列。

-   簡易說明(必要，除非該檔案擁有兩個以上之類別或函數)
-   `@var` (必要，於該屬性後其後註明屬性種類(property type))
-   `@deprecated` (選填)
-   `@since` (選填)

範例：

``` php
class Foo
{
    /**
     * Description of Bar.
     *
     * @var    string
     *
     * @since  1.0
     */
    public protected $bar;
}
```

### 常數區塊註解

類別內的常數區塊註解並非必要，但可適度補上。範例如下

``` php
class Foo
{
    /**
     * Description of Constant.
     *
     * @const  string
     */
    const SAKURA = 'sakura';
}
```
