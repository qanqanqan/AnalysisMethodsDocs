В ClickHouse функция `COUNT(DISTINCT)` используется для подсчета уникальных значений в указанном столбце, исключая дубликаты и значения NULL. Это полезно для анализа данных, когда необходимо узнать, сколько разных значений присутствует в колонке.

## Синтаксис

Синтаксис функции `COUNT(DISTINCT)` выглядит следующим образом:

```sql
SELECT COUNT(DISTINCT column_name) FROM table_name;
```

### Пример использования

Предположим, у вас есть таблица `orders`, содержащая информацию о заказах:

```sql
CREATE TABLE orders (
    order_id UInt32,
    customer_id UInt32,
    product_id UInt32,
    order_date Date
) ENGINE = MergeTree()
ORDER BY order_date;
```

Если вы хотите подсчитать количество уникальных клиентов, сделавших заказы, вы можете использовать следующий запрос:

```sql
SELECT COUNT(DISTINCT customer_id) AS unique_customers
FROM orders;
```

Этот запрос вернет количество уникальных идентификаторов клиентов из таблицы `orders`.

### Подсчет уникальных комбинаций

Вы также можете использовать `COUNT(DISTINCT)` для подсчета уникальных комбинаций значений из нескольких столбцов. Например, если вы хотите узнать, сколько уникальных комбинаций клиентов и продуктов было заказано, используйте следующий запрос:

```sql
SELECT COUNT(DISTINCT (customer_id, product_id)) AS unique_orders
FROM orders;
```

Этот запрос подсчитает количество уникальных пар `customer_id` и `product_id`.

### Обработка NULL значений

Функция `COUNT(DISTINCT)` игнорирует значения NULL. Это означает, что если в столбце есть NULL значения, они не будут учтены в итоговом подсчете.

Пример:

```sql
CREATE TABLE products (
    product_id UInt32,
    category String,
    price Nullable(Float32)
) ENGINE = MergeTree()
ORDER BY product_id;

INSERT INTO products VALUES (1, 'Electronics', 299.99), (2, 'Electronics', NULL), (3, 'Furniture', 199.99), (4, 'Furniture', 199.99);

SELECT COUNT(DISTINCT price) AS unique_prices
FROM products;
```

В этом случае запрос вернет 2, так как только два уникальных значения цены (299.99 и 199.99) присутствуют в таблице.

## Заключение

Функция `COUNT(DISTINCT)` в ClickHouse является мощным инструментом для анализа данных, позволяя получать точные подсчеты уникальных значений в столбцах. Она полезна для выявления разнообразия данных и может использоваться как с одиночными столбцами, так и с комбинациями нескольких столбцов.

Citations:
[1] https://www.pdq.com/blog/sql-count-distinct-vs-distinct/
[2] https://www.airops.com/sql-guide/how-to-count-distinct-values-in-sql
[3] https://www.simplilearn.com/count-distinct-sql-article
[4] https://docs.aws.amazon.com/clean-rooms/latest/sql-reference/count-function.html
[5] https://www.w3resource.com/sql/aggregate-functions/count-with-distinct.php
[6] https://www.w3schools.com/sql/sql_distinct.asp
[7] https://stackoverflow.com/questions/1521605/selecting-count-with-distinct
[8] https://learn.microsoft.com/ru-ru/sql/t-sql/functions/count-transact-sql?view=sql-server-ver16