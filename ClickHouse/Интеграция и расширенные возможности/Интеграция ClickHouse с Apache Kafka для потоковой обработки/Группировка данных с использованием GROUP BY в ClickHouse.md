Группировка данных с использованием `GROUP BY` в ClickHouse позволяет эффективно агрегировать и анализировать большие объемы информации. Этот оператор используется для объединения строк, имеющих одинаковые значения в указанных столбцах, и часто применяется в сочетании с агрегатными функциями для получения сводной информации.

## Основной синтаксис GROUP BY

Синтаксис для использования `GROUP BY` в ClickHouse выглядит следующим образом:

```sql
SELECT column_name(s), aggregate_function(column)
FROM table_name
WHERE condition
GROUP BY column_name(s);
```

### Пример использования

Рассмотрим таблицу `sales`, содержащую данные о продажах. Чтобы получить общую сумму продаж по каждому продукту, можно использовать следующий запрос:

```sql
SELECT product_id, SUM(amount) AS total_sales
FROM sales
GROUP BY product_id;
```

Этот запрос сгруппирует данные по `product_id` и вернет общую сумму продаж для каждого продукта.

## Группировка по нескольким столбцам

ClickHouse позволяет группировать данные по нескольким столбцам. Например, если вы хотите получить количество продаж по продуктам и месяцам, используйте:

```sql
SELECT product_id, toMonth(order_date) AS month, SUM(quantity) AS total_quantity
FROM sales
GROUP BY product_id, month
ORDER BY product_id, month;
```

В этом запросе используется функция `toMonth()`, чтобы извлечь месяц из даты заказа.

## Использование ROLLUP и CUBE

ClickHouse поддерживает расширенные операции группировки с помощью функций `ROLLUP` и `CUBE`, которые позволяют создавать иерархии подытогов.

### Пример с ROLLUP

Для подсчета количества просмотров страниц по браузерам и операционным системам можно использовать `ROLLUP`:

```sql
SELECT 
    CASE 
        WHEN user_agent LIKE '%Firefox%' THEN 'Firefox' 
        WHEN user_agent LIKE '%Chrome%' THEN 'Chrome' 
        ELSE 'Other' 
    END AS browser,
    CASE 
        WHEN user_agent LIKE '%Windows%' THEN 'Windows' 
        WHEN user_agent LIKE '%Mac OS%' THEN 'Mac OS' 
        ELSE 'Other' 
    END AS os,
    COUNT(*) AS page_views
FROM web_traffic
GROUP BY ROLLUP(browser, os)
ORDER BY browser, os;
```

Этот запрос создаст подытоги для каждого браузера и операционной системы.

### Пример с CUBE

Для получения средней зарплаты по отделам и должностям с подытогами можно использовать `CUBE`:

```sql
SELECT department, job_title, AVG(salary) AS avg_salary
FROM employees
GROUP BY CUBE(department, job_title)
ORDER BY department, job_title;
```

Это вернет среднюю зарплату для каждой комбинации отдела и должности, а также общие подытоги.

## Фильтрация групп с помощью HAVING

Чтобы фильтровать результаты после группировки, используйте оператор `HAVING`. Например:

```sql
SELECT department, AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000;
```

Этот запрос вернет только те отделы, где средняя зарплата превышает 50 000.

## Заключение

Оператор `GROUP BY` в ClickHouse предоставляет мощные возможности для анализа данных. Он позволяет агрегировать информацию по одному или нескольким столбцам и использовать функции `ROLLUP` и `CUBE` для создания подытогов. В сочетании с оператором `HAVING` вы можете фильтровать результаты группировки для получения более целенаправленной аналитики.

Citations:
[1] https://chistadata.com/groupby-groupings-rollups-cubes-clickhouse/
[2] https://www.w3schools.com/sql/sql_groupby.asp
[3] https://www.tutorialspoint.com/sql/sql-group-by.htm
[4] https://clickhouse.com/docs/en/sql-reference/statements/select/group-by
[5] https://kb.altinity.com/altinity-kb-queries-and-syntax/ttl/ttl-group-by-examples/
[6] https://stackoverflow.com/questions/65881507/clickhouse-aggregates-group-by-day-month-yeartimestamp
[7] https://kb.altinity.com/altinity-kb-queries-and-syntax/group-by/
[8] https://altinity.com/blog/clickhouse-aggregation-fun-part-2-exploring-and-fixing-performance