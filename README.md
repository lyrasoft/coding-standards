# LYRASOFT 軟體工程編碼標準文件

## 前言

好用的功能造就優秀的軟體，高水準的程式碼品質造就偉大的軟體。要讓我們的產品從優質進步到偉大，我們必須專注在外人看不到的苦工上，讓軟體更健壯、好維護。

編碼標準是軟體工程中的重要組成部分，用以規範工程師的寫作、排版與命名，並保持一致性，使程式碼可讀性提高，讓軟體易於維護。本標準文件為 LYRASOFT 工程師參考用編碼標準，基於 Joomla! & Windwalker 編碼標準，簡化部分非 Open source 所需之繁雜規定，並以範例形式展示。

## 關於 PSR 標準

在 PSR 盛行的今日，我們暫時未轉進 PSR-2 標準。由於我們的軟體工程知識有其歷史，遠從 PHP 尚未全面標準化的年代，我們就致力於與國際 Opensource 接軌，遵循我們所熟悉的生態圈的標準，並建立屬於自己的開發規範與流程。LYRASOFT 的編碼風格承襲自 PEAR 與 Joomla，經過長年的檢驗與調整，是非常優秀且易讀性高的標準。也因此 LYRASOFT 將繼續維持這個標準，並隨時根據 PHP 社群的習慣過作微幅調整。

本標準中若有未盡完善的規則，依然可以優先參考 PSR-1 與 PSR-2 取而代之，臨時需要採用他方標準時，可善用 PHPStorm 的 Code Style 功能作調整。

### 原始 Joomla! 編碼標準

請參照 [Joomla-Coding-Stanard-Chinese](https://github.com/asika32764/Joomla-Coding-Stanard-Chinese) （本文件少部分已過期，詳情請以[官方文件](http://joomla.github.io/coding-standards/)為主）

## 其他編碼標準

- [Javascript 標準](javascript.md)
- [CSS 標準](css.md)
- [SQL 建議寫法](sql.md)

## 檔案格式

### 編碼

請一律使用未帶 BOM 的 UTF-8 編碼格式。

### 換行

編輯器請設定成以 `LF` (\n) 作為換行字元。請勿使用 Mac 的回車符 `CR` (\r) 或 Windows 的 `CRLF` (\r\n) 組合。

Windows 安裝 Git 時，請選擇 `Checkout Windows-style, commit Unix-style line endings` 選項。

![git-crlf](http://cl.ly/T2xQ/git-crlf.png)

參見： [Git 在 Windows 平台處理斷行字元 (CRLF) 的注意事項](http://blog.miniasp.com/post/2013/09/15/Git-for-Windows-Line-Ending-Conversion-Notes.aspx)


## 基礎編輯設定

### PHP 標籤

永遠使用長標籤： `<?php ?>` 來包裹 PHP 程式碼，不使用短標籤 `<? ?>`。這可以確保您的程式碼可執行在大多數未經設定的主機環境。

如果檔案中只包含PHP程式碼，則不應該包含結尾標籤 `?>`。這個標籤在 PHP 中不是必要的。這樣子可以避免在系統輸出前，不小心讓任何空白預先送出 header ，這會造成 Joomla 的 Session 功能錯誤 (參見 PHP 手冊的說明 [Instruction separation](http://php.net/basic-syntax.instruction-separation))。

永遠保留一行空行在檔案結尾，範例：

``` php
<?php

class Foo
{

}

```

### 縮排與對齊

程式縮進請使用 Tab 而非空白字元，註解與變數指派的排版對齊應使用空白字元。

範例：

``` php
/**
 * The foo function.
 *
 * @return void
 */
function foo()
{
	$a    = 'a';
	$bar  = 'bar';
	$cool = 'cool';
}
```

### 行寬

原則上每行程式碼並沒有最大長度限制，但根據國際標準值，150字元能夠在不需要水平滾動的狀況下達到最好的可讀性。
若遇到換行會影響輸出結果的情況(例如 PHP/HTML 混合的結構檔案)，更長的程式碼也是被允許的。


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

### 控制結構

所有的控制結構必須在關鍵字與起始括號之間放置一個空白字元，而開頭、結尾括號與邏輯判斷式之間無空格。這是為了區隔控制結構與函式以方便辨認，而括號內必須要包含邏輯。

控制結構關鍵字，如： `if`, `else`, `do`, `for`, `foreach`, `try`, `catch`, `switch` 與 `while` 皆採 [Allman](http://en.wikipedia.org/wiki/Indent_style#Allman_style) 風格，關鍵字本身必須在一個新行，而語言區塊（大括號）的開頭與結束也接需再一個新行中。

### _if-else_ 範例

```php
if ($test)
{
	echo 'True';
}
// 註解可寫在此
// 要注意的是 "elseif" 採用單一單字而非拆開成 "else if"
elseif ($test === false)
{
	echo 'Really false';
}
else
{
	echo 'A white lie';
}
```

如果控制結構的判斷式需要多行，則第二行開始需要一個 Tab 縮排，結尾括號需要再同一行上。

```php
if ($test1
	&& $test2)
{
	echo 'True';
}
```

### _do-while_ 範例


```php
do
{
	$i++;
}
while ($i < 10);
```

### _for_ 範例

```php
for ($i = 0; $i < $n; $i++)
{
	echo 'Increment = ' . $i;
}
```

### _foreach_ 範例

```php
foreach ($rows as $index => $row)
{
	echo 'Index = ' . $id . ', Value = ' . $row;
}
```

### _while_ 範例

```php
while (!$done)
{
	$done = true;
}
```

### _switch_ 範例

當使用 `switch` 時， `case` 關鍵字必須縮排一次，而 `break` 關鍵字必須獨立在新的一行，且相較於 `case` 再縮排一次。

```php
switch ($value)
{
	case 'a':
		echo 'A';
		break;

	default:
		echo 'I give up';
		break;
}
```

#### 關於 Allman 風格

採用 Allman 風格的優點在於，我們可以隨時註解或替換控制結構，而不會造成語言錯誤。例如：

``` php
// if ($egg)
{
	echo 'coffee';
}
```

``` php
foreach ($array as $i => $value)
// for ($i = 0; $i <= count($array); $i++)
{
	// Do something...
}
```

以上寫法皆可以正常運作。

除此之外，我們不必為了程式碼可讀性刻意在控制結構前後增加空行：

``` php
if ($yoo)
{
	echo '123';
}
else
{
	echo '321';
}
```

比起：

``` php
if ($yoo) {
	echo '123';
} else {
	echo '321';
}
```

有較佳可讀性，雖然可能造成程式碼文件較長。

### 換行

#### 類別、方法與控制結構

在 Allman 風格之下，控制結構與類別方法的大括號前後都不需刻意增加空行，只有方法與方法之間空一行以區隔。

#### 程式邏輯運作過程

編碼過程，建議每一行不同的邏輯控制皆空行，並且使用括號區隔的 Scope 空間上下也需要空行。

若連續變數賦值則不需空行。

範例：

``` php
if ($avoid)
{
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

> **注意**
>
> 在 PHP 5 ，物件不需要參照符號，所有物件皆是參照。

## 字串連接的間距

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

## 類別與其成員宣告

### 類別

類別採用駝峰式 (CamelCase) 命名法，開頭大寫，例如： `ContentModelArticle` 或 `FGHElper`

前後括號各自為一個新行，括號與內容之間不應該有空行。除了 Alias 以外每個檔案應該只有一個類別，`extends` 與 `implement` 應該與類別名稱在同一行。

抽象物件應加上 `abstract` 在 `class` 之前。

### 函式與方法

採用 "studly caps" 風格 (也稱作 "bumpy case" 或 "camel caps")，起始字母小寫，每個單字開頭大寫，範例： `getItems()`。

前後括號各自為一個新行，方法與方法之間（包含其註解區塊）應該只有一行空行。

方法宣告應搭配權限關鍵字，非公開介面應該盡量以 `protected` 為主，讓子代類別可以取用父代方法，公開介面則應該搭配 `public`。靜態調用的方法還須再加上 `static` 關鍵字。

如果一個函數定義橫跨數行，第二行起的每一行皆需以一個 tab 作縮排，結束括號需與最後一個參數存在於同一行。

```php
function fooBar($param1, $param2,
	$param3, $param4)
{
	// Body of method.
}
```

### 類別屬性

Joomla! CMS 中，類別屬性以底線分隔單字，如： `$this->default_view`，以跟方法坐區隔。但在 Framework 中，屬性也改以"studly caps" 風格。因此僅規範專案統一即可。

屬性宣告應搭配權限關鍵字，如 `public`、`protected`、`private`。若非特別必要，預宣告屬性應該以 `protected` 為主，讓子代類別可以存取父代屬性，而外部則依靠 getter 與 setter 來存取。

若屬性沒有初始值，則一定為 `null`，沒有特殊目的則不需強制指派初始值。

### 常數

常數應永遠為大寫，並用底線分隔單字，根據專案加上前綴字。例： `BAMAHOME_ADMIN`。

### 宣告範例

``` php
<?php

class ContentModelArticle extends JModelLegacy
{
	public $foo;

	public function getFoo()
	{
		return $this->foo;
	}
}
```

## 一般變數宣告及使用

### 變數命名原則

在 LYRASOFT 的專案中，變數優先採用 "studly caps" 風格 (也稱作 "bumpy case" 或 "camel caps")。名稱起始的字母必須是小寫，而每個新單字的首個字母為大寫。

### 賦值

一般變數建議集中於方法開頭進行宣告或賦值，可依據變數長度適度排版，等號距離最長變數應該只有一個空格：

``` php
public function getItems($pk = null)
{
	$pk    = (int) $pk ?: $this->input->getInt('id');
	$db    = JFactory::getDbo();
	$app   = JFactory::getApplication();
	$query = $db->getQuery(true);

	// 函式主體
}
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

## 陣列

陣列元素的指派可稍微排版，當多行時，可用 Tab 縮排。每行跟隨一個都逗號結尾，最後一行可包含逗號，這是 PHP 允許的寫法，對於程式碼 diff 比對時也有所幫助。

範例:
陣列
```php
$options = array(
	'foo'  => 'foo',
	'spam' => 'spam',
);
```

若確保環境為 PHP 5.4 ，可使用短宣告語法：

``` php
$array = ['a', 'b', ['c', 'd']];
```

## 函式與方法呼叫

### 一般函式與方法調用

呼叫函式時, 函式名稱跟參數括號中不能有空白, 而第一個帶入參數也不得有空白; 每個參數逗點後一個空白隔開 (如果有的話), 最後一個參數跟括號沒有空格. 參數帶值等號前後需有空白. 可以多行對齊.

``` php
// 單函式呼叫
$item = $foo->getItem($pk);

// 多函式呼叫
$short  = bar('short');
$medium = bar('medium');
$long   = bar('long');
```

### 鏈狀調用

若物件支援鏈狀調用，除了第一次調用以外，需要換行並縮排一次，無需對齊物件變數，且結尾直接跟著分號：

``` php
$query->select('u.*')
	->from($db->qn('#__users') . ' AS u'))
	->where($db->qn('id') . ' = ' . $db->q($pk));
```

調用後行寬較短時可考慮不換行：

``` php
$db->setQuery($query)->loadResult();
```

## 判斷式

### 運算子

使用 `&&` 與 `||` 運算子而非 `and` 與 `or` 運算子。

### 大小於判斷

根據語意化決定使用 `> 0` 還是 `>= 1` ，以易懂為主。

### 三元運算

運算子前後應空格，分號前不空格。若過長的三元運算子，可考慮換行處理，但運算子必須在開頭，換行後縮進一次。

範例：

``` php
echo $alias ? 'Alias' : 'Title';
```

``` php
echo ($config->get('item.alias') == $alias)
	? $this->item->get('alias')
	: $this->item->get('title', $default);
```

確保為 PHP 5.3 的環境可用簡寫：

``` php
$foo = $bar ?: $yoo;
```

### 存在判斷

當我們為了避免因變數或陣列元素未宣告而造成 Notice 時，可考慮用專用的判斷函式在 if 中。

#### isset()

用在判斷變數或陣列是否宣告過，且值不為 null：

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

#### 直接判斷

若我們能夠確保變數或物件元素一定存在，請大膽直接將他放在判斷式中，此為慣例寫法，表達性佳不會有混淆問題：

``` php
if ($yoo)
{
	echo 'GO';
}
elseif (!$hoo)
{
	echo 'Back';
}
```

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
