
## Соединение таблиц
Hive поддерживает различные типы соединений, которые позволяют объединять данные из нескольких таблиц. Основные типы соединений:

1. Внутреннее соединение (INNER JOIN)
Возвращает только те строки, которые имеют совпадения в обеих таблицах.
```sql
SELECT c.Id, c.Name, o.Amount  
  FROM sample_joins c  
  JOIN sample_joins1 o ON c.Id = o.Id;
```
2. Левое внешнее соединение (LEFT OUTER JOIN)
Возвращает все строки из левой таблицы и совпадающие строки из правой. Если совпадений нет, в результате будут NULL-значения для правой таблицы.

```sql
SELECT c.Id, c.Name, o.Amount  
  FROM sample_joins c  
  LEFT OUTER JOIN sample_joins1 o ON c.Id = o.Id;
```
3. Правое внешнее соединение (RIGHT OUTER JOIN)
Возвращает все строки из правой таблицы и совпадающие строки из левой. Если совпадений нет, в результате будут NULL-значения для левой таблицы.

```sql
SELECT c.Id, c.Name, o.Amount  
  FROM sample_joins c  
 RIGHT OUTER JOIN sample_joins1 o ON c.Id = o.Id;
```
4. Полное внешнее соединение (FULL OUTER JOIN)
Возвращает все строки из обеих таблиц, с NULL-значениями для отсутствующих совпадений.
``` sql
SELECT c.Id, c.Name, o.Amount  
  FROM sample_joins c  
  FULL OUTER JOIN sample_joins1 o ON c.Id = o.Id;
```

---

## Подзапросы
Подзапрос — это запрос, вложенный в другой запрос. Подзапросы могут использоваться в предложениях `SELECT`, `FROM` и `WHERE`.

1. Подзапрос в предложении FROM
Подзапрос может возвращать набор данных, который затем используется в основном запросе.
```sql
SELECT avg_amount  
  FROM (SELECT AVG(amount) AS avg_amount FROM sales) AS avg_sales;
```
2. Подзапрос в предложении WHERE
Подзапрос может использоваться для фильтрации данных на основе результатов другого запроса.
```sql
SELECT *  
  FROM sales  
 WHERE product_id IN (SELECT product_id FROM products WHERE category = 'Electronics');
```

---

## Оптимизация соединений

Для повышения производительности запросов с использованием соединений можно применять следующие методы:

- **Использование автоматического преобразования JOIN**: Установите параметр `hive.auto.convert.join` в значение `true`, чтобы автоматически использовать Map-side joins для малых таблиц.

```sql
SET hive.auto.convert.join=true;
```

- **Использование бакетирования**: При выполнении соединений с бакетированными таблицами включите параметр `hive.enforce.bucketing`.

``` sql
SET hive.enforce.bucketing=true;
```
---

