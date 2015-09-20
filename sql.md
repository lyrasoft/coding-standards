# LYRASOFT SQL 建議寫法

## 基本概念

SQL 的保留字要全用大寫，其他的文字符號 (當然被引號包起來的文字為例外) 為小寫。

全部的 table 名稱必須要用 `#__` 的前綴字而不是用 `jos_` 這樣寫死的前綴字去讀取資料庫內容，並允許替換為使用者定義的資料庫前綴字。

例：

``` sql
SELECT id, title, alias FROM #__table WHERE id = 1;
```

## Query Builder

在 Joomla 環境下，盡量使用 Query Builder 來編寫 SQL:

``` php
// Get the database connector.
$db = JFactory::getDbo();

// Get the query from the database connector.
$query = $db->getQuery(true);

// Build the query programatically (using chaining if desired).
$query->select('u.*')
	// Use the qn alias for the quoteName method to quote table names.
	->from($db->qn('#__users').' AS u'));

// Tell the database connector what query to run.
$db->setQuery($query);

// Invoke the query or data retrieval helper.
$users = $db->loadObjectList();
```

## JOIN

Join 時使用的資料表 alias，請使用語意化的單數名稱如： article, user, category，避免使用 a, b, c 等令人混淆的別名。 

ON 條件判斷，請盡量以較少的表在左邊來比對較多的表，以增進比對效能。例如文章與分類：

``` php
$query->select('article.*, category.*')
	->from('#__articles AS article')
	->leftJoin('#__categories AS category ON category.id = article.catid');
```

## GROUP

LEFT JOIN 應該用在左表列數多於右表的情況（多對一），非必要，不應讓右表比左表多（一對多）。若不得已遇到這種情況，請使用 GROUP 來避免總數分頁計算錯誤。

## 跳脫與引號

對於動態插入的欄位名稱，或者明確與保留字衝突的名稱，皆需以反引號來包裹：

``` php
$query->where($query->quoteName('id') . ' = 1');

// 等同

$query->where($query->qn('id') . ' = 1');
```

對於內容字串，應該以字串引號來包裹並同時跳脫：

``` php
$query->where('title = ' . $query->quote('Foo'));

// 等同

$query->where('title = ' . $query->q('Foo'));
```

你可以直接跳脫單一字串：

``` php
$query->escape('content');

// 等同

$query->e('content');
```

或是用 format 功能

``` php
$query->where($query->format('%n = %q', 'title', 'Content'));
```

參考 format 說明：

``` php
/**
 * Find and replace sprintf-like tokens in a format string.
 * Each token takes one of the following forms:
 *     %%       - A literal percent character.
 *     %[t]     - Where [t] is a type specifier.
 *     %[n]$[x] - Where [n] is an argument specifier and [t] is a type specifier.
 *
 * Types:
 * a - Numeric: Replacement text is coerced to a numeric type but not quoted or escaped.
 * e - Escape: Replacement text is passed to $this->escape().
 * E - Escape (extra): Replacement text is passed to $this->escape() with true as the second argument.
 * n - Name Quote: Replacement text is passed to $this->quoteName().
 * q - Quote: Replacement text is passed to $this->quote().
 * Q - Quote (no escape): Replacement text is passed to $this->quote() with false as the second argument.
 * r - Raw: Replacement text is used as-is. (Be careful)
 *
 * Date Types:
 * - Replacement text automatically quoted (use uppercase for Name Quote).
 * - Replacement text should be a string in date format or name of a date column.
 * y/Y - Year
 * m/M - Month
 * d/D - Day
 * h/H - Hour
 * i/I - Minute
 * s/S - Second
 *
 * Invariable Types:
 * - Takes no argument.
 * - Argument index not incremented.
 * t - Replacement text is the result of $this->currentTimestamp().
 * z - Replacement text is the result of $this->nullDate(false).
 * Z - Replacement text is the result of $this->nullDate(true).
 *
 * Usage:
 * $query->format('SELECT %1$n FROM %2$n WHERE %3$n = %4$a', 'foo', '#__foo', 'bar', 1);
 * Returns: SELECT `foo` FROM `#__foo` WHERE `bar` = 1
 *
 * Notes:
 * The argument specifier is optional but recommended for clarity.
 * The argument index used for unspecified tokens is incremented only when used.
 */
```


