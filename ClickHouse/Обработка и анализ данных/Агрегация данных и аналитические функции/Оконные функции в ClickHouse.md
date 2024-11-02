В ClickHouse оконные функции позволяют выполнять вычисления над наборами строк, сохраняя при этом отдельные строки в результирующем наборе данных. Это мощный инструмент для аналитической обработки, который может использоваться для расчета нарастающих итогов, скользящих средних и других статистических показателей.

## Основные аспекты оконных функций в ClickHouse

### Синтаксис оконных функций

Оконные функции в ClickHouse используют следующий синтаксис:

```sql
function_name(column) OVER (
    [PARTITION BY partition_expression]
    [ORDER BY order_expression]
    [ROWS | RANGE frame_specification]
)
```

- **`function_name`**: Название функции, например, `SUM()`, `AVG()`, `ROW_NUMBER()`.
- **`PARTITION BY`**: Определяет, как данные будут разбиты на группы (окна).
- **`ORDER BY`**: Указывает порядок строк в пределах каждой группы.
- **`ROWS` или `RANGE`**: Определяет размер окна.

### Примеры использования оконных функций

#### Пример 1: Нарастающий итог

Для вычисления нарастающего итога по столбцу `sales_amount` можно использовать функцию `SUM()` с оконной конструкцией:

```sql
SELECT 
    sale_date,
    sales_amount,
    SUM(sales_amount) OVER (ORDER BY sale_date) AS cumulative_sales
FROM 
    sales;
```

Этот запрос вернет дату продажи, сумму продаж и накопленную сумму продаж по дате.

#### Пример 2: Скользящее среднее

Для вычисления скользящего среднего за последние 3 дня можно использовать следующий запрос:

```sql
SELECT 
    sale_date,
    sales_amount,
    AVG(sales_amount) OVER (ORDER BY sale_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS moving_average
FROM 
    sales;
```

Здесь `ROWS BETWEEN 2 PRECEDING AND CURRENT ROW` определяет окно из трех строк (текущая и две предыдущие).

#### Пример 3: Ранжирование

Чтобы присвоить ранги строкам на основе значения в столбце `sales_amount`, можно использовать функцию `ROW_NUMBER()`:

```sql
SELECT 
    product_id,
    sales_amount,
    ROW_NUMBER() OVER (ORDER BY sales_amount DESC) AS rank
FROM 
    sales;
```

Этот запрос вернет идентификатор продукта, сумму продаж и ранг продукта по убыванию суммы продаж.

### Ограничения и особенности

- **Отсутствие полноценной поддержки оконных функций**: В отличие от некоторых других СУБД, ClickHouse не поддерживает все возможности оконных функций. Например, отсутствуют такие функции как `LEAD()` и `LAG()`, но их можно эмулировать с помощью других методов.
  
- **Работа с массивами**: В ClickHouse можно использовать функции для работы с массивами, такие как `arraySlice()` и `arrayMap()`, чтобы эмулировать поведение оконных функций.

### Эмуляция оконных функций

Хотя ClickHouse не поддерживает все стандартные оконные функции, можно эмулировать их поведение через комбинацию агрегатных функций и работы с массивами. Например, для вычисления скользящего среднего можно использовать:

```sql
SELECT 
    sale_date,
    sales_amount,
    arrayAvg(arraySlice(sales_amounts, x - 2, 3)) AS moving_average
FROM 
    (SELECT sale_date, sales_amount FROM sales ORDER BY sale_date) 
ARRAY JOIN arrayEnumerate(sales_amounts) AS x;
```

Этот подход требует предварительной подготовки данных и может быть менее производительным.

## Заключение

Оконные функции в ClickHouse предоставляют мощные возможности для аналитической обработки данных. Несмотря на некоторые ограничения по сравнению с другими СУБД, такие как отсутствие некоторых стандартных функций, ClickHouse предлагает гибкие инструменты для выполнения сложных аналитических задач. Эмуляция оконных функций с использованием массивов и агрегатных функций позволяет пользователям получать необходимые результаты для анализа данных.

Citations:
[1] https://habr.com/ru/articles/515606/
[2] https://clickhouse.com/docs/ru/sql-reference/functions
[3] https://clickhouse.com/docs/ru/sql-reference/functions/other-functions
[4] https://thisisdata.ru/blog/uchimsya-primenyat-okonnyye-funktsii/
[5] https://docs.arenadata.io/ru/ADQM/current/how-to/data-querying/query-types/window-functions.html
[6] https://habr.com/ru/articles/842078/
[7] https://bigdataschool.ru/blog/news/clickhouse/aggregations-in-clickhouse.html
[8] https://clickhouse.com/docs/ru/sql-reference/statements/select/limit-by