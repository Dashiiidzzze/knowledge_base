**Функция пользователя** представляет собой группу произвольных операторов SQL, предназначенных для выполнения некоторой задачи. Эти функции не поставляются из коробки и обычно создаются для обработки специфичных сценариев.

Функция в PostgreSQL может создаваться на любом языке, таком как SQL, C, PL/pgSQL, Python и т.д.

### Структура функции
В этом синтаксисе после предложения CREATE OR REPLACE FUNCTION указывается имя функции (function_name) со списком аргументов или параметров. Затем после ключевого слова RETURNS объявляется тип данных возвращаемого значения (return_datatype ). return_datatype может быть одним из типов данных PostgreSQL, например, character, integer, double и т.п. Так же из функции PostgreSQL можно вернуть таблицу.  
  
Далее после ключевого слова DECLARE объявляются используемые в функции переменные IN, OUT. Далее в блоке BEGIN-END задается тело функции (function_body). function_body обычно содержит бизнес-логику функции. Затем после ключевого слова RETURN указывается имя переменной (variable_name), которая содержит возвращаемое из функции значение.  
  
Наконец, после ключевого слова LANGUAGE указывается язык (language_name), на котором написана функция.

```PostgreSQL
CREATE [OR REPLACE] FUNCTION function_name (arguments) 
RETURNS return_datatype AS $variable_name$
   DECLARE
      declaration;
      [...]
   BEGIN
      < function_body >
      [.. logic]
      RETURN { variable_name | value }
   END; 
LANGUAGE language_name;
```
Запросы можно выполнять либо в оболочке PostgreSQL (PSQL), либо в среде PgAdmin (query tool).

### Как создать функцию в PGAdmin
Заходим на сервер, затем Schemas, затем Public, затем нажимаем правой кнопкой мыши на Function. В окне выбираем PSQL tool или Query tool и вводим функцию в окно.

По хорошему она должна появиться во вкладке Function, но у меня это не работает.

Пример функции
```PostgreSQL
CREATE OR REPLACE FUNCTION count_incidents(p_start_date TIMESTAMP, p_end_date TIMESTAMP)
RETURNS INT AS $$
BEGIN
    RETURN (
        SELECT COUNT(*) 
        FROM Инциденты
        WHERE Дата_и_время BETWEEN p_start_date AND p_end_date
    );
END;
$$ LANGUAGE plpgsql;
```
Пример вызова функции
```PostgreSQL
SELECT count_incidents('2024-01-01', '2024-03-01'); -- Количество инцидентов за указанный период
```
