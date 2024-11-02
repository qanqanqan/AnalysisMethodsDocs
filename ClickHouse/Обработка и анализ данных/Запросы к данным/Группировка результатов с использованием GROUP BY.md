В ClickHouse оператор `GROUP BY` используется для группировки результатов выборки по одному или нескольким столбцам, что позволяет выполнять агрегацию данных. Это особенно полезно для анализа и получения сводной информации из больших наборов данных. Рассмотрим основные аспекты использования `GROUP BY` в ClickHouse.

## Основы использования GROUP BY

### Синтаксис

Основной синтаксис оператора `GROUP BY` выглядит следующим образом:

```sql
SELECT 
    column1, 
    aggregate_function(column2) AS alias_name
FROM 
    table_name
WHERE 
    condition
GROUP BY 
    column1;
```

### Пример

Предположим, у вас есть таблица `sales` с полями `product_id`, `amount` и `sale_date`. Чтобы получить общую сумму продаж для каждого продукта, вы можете использовать следующий запрос:

```sql
SELECT 
    product_id, 
    SUM(amount) AS total_sales
FROM 
    sales
GROUP BY 
    product_id;
```

Этот запрос вернет уникальные идентификаторы продуктов вместе с суммой продаж для каждого продукта.

## Группировка по нескольким столбцам

Вы можете группировать результаты по нескольким столбцам. Например, если вы хотите получить общую сумму продаж по продуктам и месяцам, вы можете использовать следующий запрос:

```sql
SELECT 
    product_id, 
    toStartOfMonth(sale_date) AS sale_month, 
    SUM(amount) AS total_sales
FROM 
    sales
GROUP BY 
    product_id, sale_month;
```

В этом примере данные группируются по `product_id` и месяцу продажи.

## Использование агрегатных функций

ClickHouse поддерживает множество агрегатных функций, таких как:

- `SUM()`: Сумма значений.
- `COUNT()`: Подсчет количества строк.
- `AVG()`: Среднее значение.
- `MIN()`: Минимальное значение.
- `MAX()`: Максимальное значение.

### Пример с несколькими агрегатами

Вы можете использовать несколько агрегатных функций в одном запросе:

```sql
SELECT 
    product_id,
    COUNT(*) AS number_of_sales,
    SUM(amount) AS total_sales,
    AVG(amount) AS average_sale
FROM 
    sales
GROUP BY 
    product_id;
```

Этот запрос вернет количество продаж, общую сумму и среднюю сумму продаж для каждого продукта.

## Модификаторы ROLLUP и CUBE

ClickHouse поддерживает модификаторы `ROLLUP` и `CUBE`, которые позволяют вычислять подытоги на разных уровнях агрегации.

### Пример использования ROLLUP

Используя `ROLLUP`, вы можете получить итоги по каждому продукту и общий итог:

```sql
SELECT 
    product_id,
    SUM(amount) AS total_sales
FROM 
    sales
GROUP BY 
    ROLLUP(product_id);
```

Этот запрос вернет общую сумму продаж для каждого продукта и общий итог по всем продуктам.

### Пример использования CUBE

Используя `CUBE`, вы можете получить суммы для всех комбинаций столбцов:

```sql
SELECT 
    product_id,
    toStartOfMonth(sale_date) AS sale_month,
    SUM(amount) AS total_sales
FROM 
    sales
GROUP BY 
    CUBE(product_id, sale_month);
```

Этот запрос вернет суммы продаж для каждой комбинации продукта и месяца.

## Заключение

Оператор `GROUP BY` в ClickHouse является мощным инструментом для агрегации данных и получения сводной информации. Он позволяет группировать данные по одному или нескольким столбцам и использовать различные агрегатные функции для анализа. Модификаторы `ROLLUP` и `CUBE` добавляют дополнительные возможности для получения подытогов на разных уровнях агрегации, что делает ClickHouse эффективным инструментом для аналитики данных.

Citations:
[1] https://stackoverflow.com/questions/65881507/clickhouse-aggregates-group-by-day-month-yeartimestamp
[2] https://kb.altinity.com/altinity-kb-queries-and-syntax/group-by/
[3] https://bigdataschool.ru/blog/news/clickhouse/aggregations-in-clickhouse.html
[4] https://clickhouse.com/docs/en/sql-reference/statements/select/group-by
[5] https://clickhouse.com/docs/en/sql-reference/aggregate-functions/grouping_function
[6] https://clickhouse.com/docs/en/sql-reference/aggregate-functions/reference/grouparray
[7] https://clickhouse.com/docs/en/observability/managing-data
[8] https://juicefs.com/en/blog/solutions/clickhouse-disaggregated-storage-and-compute-practice