Группировка данных в SQL — это важный инструмент для анализа и обработки информации, позволяющий объединять строки с одинаковыми значениями в определенных столбцах. Основной оператор для этого — `GROUP BY`, который часто используется вместе с агрегатными функциями.

## Основы оператора GROUP BY

### Синтаксис

Основной синтаксис оператора `GROUP BY` выглядит следующим образом:

```sql
SELECT column_name(s), aggregate_function(column)
FROM table_name
WHERE condition
GROUP BY column_name(s);
```

- **column_name(s)** — это имена столбцов, по которым происходит группировка.
- **aggregate_function(column)** — агрегатная функция, такая как `SUM()`, `COUNT()`, `AVG()` и т.д., применяемая к другим столбцам.

### Пример использования

Если у вас есть таблица `SALES_DATA` с данными о продажах, вы можете использовать `GROUP BY`, чтобы получить общую сумму продаж по каждому году:

```sql
SELECT YEAR, SUM(SALES) AS TOTAL_SALES
FROM SALES_DATA
GROUP BY YEAR;
```

## Группировка по нескольким столбцам

Вы можете группировать данные по нескольким столбцам, что позволяет создавать более сложные агрегаты. Например:

```sql
SELECT ADDRESS, AGE, SUM(SALARY) AS TOTAL_SALARY
FROM CUSTOMERS
GROUP BY ADDRESS, AGE;
```

В этом примере данные группируются по адресу и возрасту клиентов.

## Использование ORDER BY с GROUP BY

Часто результаты группировки сортируются для удобства анализа. Для этого используется оператор `ORDER BY`:

```sql
SELECT AGE, COUNT(*) AS NUMBER_OF_CUSTOMERS
FROM CUSTOMERS
GROUP BY AGE
ORDER BY NUMBER_OF_CUSTOMERS DESC;
```

Этот запрос вернет количество клиентов по возрасту, отсортированное по убыванию.

## Фильтрация групп с помощью HAVING

Иногда необходимо фильтровать результаты после группировки. Для этого используется оператор `HAVING`, который позволяет задавать условия для агрегатных функций:

```sql
SELECT ADDRESS, AVG(SALARY) AS AVG_SALARY
FROM CUSTOMERS
GROUP BY ADDRESS
HAVING AVG(SALARY) > 50000;
```

Этот запрос вернет адреса и средние зарплаты только для тех адресов, где средняя зарплата превышает 50 000.

## Заключение

Оператор `GROUP BY` является мощным инструментом в SQL для агрегирования данных. Он позволяет не только группировать строки по одному или нескольким столбцам, но и применять к ним различные агрегатные функции. Использование операторов `ORDER BY` и `HAVING` расширяет возможности анализа данных, позволяя получать более информативные результаты.

Citations:
[1] https://www.tutorialspoint.com/sql/sql-group-by.htm
[2] https://www.geeksforgeeks.org/sql-group-by/
[3] https://www.w3schools.com/sql/sql_groupby.asp
[4] https://learn.microsoft.com/ru-ru/sql/t-sql/queries/select-group-by-transact-sql?view=sql-server-ver16
[5] https://altinity.com/blog/clickhouse-aggregation-fun-part-2-exploring-and-fixing-performance
[6] https://clickhouse.com/blog/aggregate-functions-combinators-in-clickhouse-for-arrays-maps-and-states
[7] https://clickhouse.com/docs/en/engines/table-engines/mergetree-family/aggregatingmergetree
[8] https://clickhouse.com/docs/en/sql-reference/data-types/aggregatefunction