В ClickHouse функция `AVG()` используется для вычисления среднего значения в наборе данных. Эта функция является одной из стандартных агрегатных функций и позволяет быстро получить среднее значение по указанному столбцу.

## Синтаксис функции AVG()

Функция `AVG()` имеет следующий синтаксис:

```sql
AVG(column_name)
```

- **`column_name`** — это имя столбца, для которого нужно вычислить среднее значение.

## Примеры использования функции AVG()

### Пример 1: Вычисление среднего значения

Предположим, у вас есть таблица `sales`, содержащая информацию о продажах:

```sql
CREATE TABLE sales (
    product_id UInt32,
    sale_amount Float32,
    sale_date Date
) ENGINE = MergeTree()
ORDER BY sale_date;
```

Чтобы вычислить среднюю сумму продаж, вы можете использовать следующий запрос:

```sql
SELECT AVG(sale_amount) AS average_sale
FROM sales;
```

Этот запрос вернет среднюю сумму всех продаж в таблице.

### Пример 2: Среднее значение с группировкой

Вы также можете использовать `AVG()` вместе с оператором `GROUP BY`, чтобы получить средние значения для каждой группы. Например, чтобы узнать среднюю сумму продаж по каждому продукту, выполните следующий запрос:

```sql
SELECT 
    product_id,
    AVG(sale_amount) AS average_sale
FROM 
    sales
GROUP BY 
    product_id;
```

Этот запрос вернет среднюю сумму продаж для каждого продукта.

### Пример 3: Обработка NULL значений

Функция `AVG()` игнорирует значения NULL при вычислении среднего. Если в столбце есть NULL значения, они не будут учитываться в расчете.

```sql
CREATE TABLE products (
    product_id UInt32,
    price Nullable(Float32)
) ENGINE = MergeTree()
ORDER BY product_id;

INSERT INTO products VALUES (1, 100.0), (2, NULL), (3, 200.0), (4, 300.0);

SELECT AVG(price) AS average_price
FROM products;
```

В этом случае запрос вернет среднюю цену как 200.0, так как значение NULL будет проигнорировано.

### Пример 4: Использование COALESCE для замены NULL

Если вы хотите заменить NULL на другое значение перед вычислением среднего, можно использовать функцию `COALESCE()`:

```sql
SELECT AVG(COALESCE(price, 0)) AS average_price
FROM products;
```

Этот запрос заменит все NULL значения на 0 перед расчетом среднего, что может быть полезно в некоторых сценариях анализа данных.

## Заключение

Функция `AVG()` в ClickHouse является мощным инструментом для анализа данных, позволяя быстро и эффективно вычислять средние значения. Она поддерживает работу с группировкой и игнорирует NULL значения по умолчанию, что делает её удобной для использования в различных аналитических задачах.

Citations:
[1] https://altinity.com/blog/harnessing-the-power-of-clickhouse-arrays-part-1
[2] https://clickhouse.com/docs/en/sql-reference/aggregate-functions
[3] https://clickhouse.com/docs/en/sql-reference/data-types/aggregatefunction
[4] https://kb.altinity.com/altinity-kb-functions/array-like-memory-usage/
[5] https://chistadata.com/aggregate-functions-via-clickhouse/
[6] https://bigdataschool.ru/blog/news/clickhouse/aggregations-in-clickhouse.html
[7] https://clickhouse.com/docs/en/sql-reference/aggregate-functions/reference
[8] https://clickhouse.com/docs/ru/sql-reference/functions