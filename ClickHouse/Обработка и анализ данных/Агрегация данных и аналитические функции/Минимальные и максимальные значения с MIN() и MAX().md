В ClickHouse функции `MIN()` и `MAX()` используются для вычисления минимальных и максимальных значений в заданном столбце. Эти функции являются важными инструментами для анализа данных, позволяя быстро получать ключевые метрики.

## Синтаксис функций

### MIN()

Функция `MIN()` возвращает наименьшее значение в указанном столбце. Синтаксис:

```sql
MIN(column_name)
```

### MAX()

Функция `MAX()` возвращает наибольшее значение в указанном столбце. Синтаксис:

```sql
MAX(column_name)
```

## Примеры использования

### Пример 1: Получение минимального и максимального значения

Предположим, у нас есть таблица `sales`, содержащая информацию о продажах:

```sql
CREATE TABLE sales (
    product_id UInt32,
    sale_amount Float32,
    sale_date Date
) ENGINE = MergeTree()
ORDER BY sale_date;
```

Чтобы получить минимальную и максимальную сумму продаж, можно использовать следующий запрос:

```sql
SELECT 
    MIN(sale_amount) AS min_sale,
    MAX(sale_amount) AS max_sale
FROM 
    sales;
```

Этот запрос вернет минимальное и максимальное значение в столбце `sale_amount`.

### Пример 2: Группировка по категориям

Если необходимо получить минимальные и максимальные значения по каждому продукту, можно использовать оператор `GROUP BY`:

```sql
SELECT 
    product_id,
    MIN(sale_amount) AS min_sale,
    MAX(sale_amount) AS max_sale
FROM 
    sales
GROUP BY 
    product_id;
```

В этом запросе данные группируются по `product_id`, и для каждого продукта вычисляются минимальная и максимальная сумма продаж.

### Пример 3: Использование с другими агрегационными функциями

Можно комбинировать функции `MIN()` и `MAX()` с другими агрегационными функциями для более комплексного анализа:

```sql
SELECT 
    product_id,
    COUNT(*) AS total_sales,
    MIN(sale_amount) AS min_sale,
    MAX(sale_amount) AS max_sale,
    AVG(sale_amount) AS avg_sale
FROM 
    sales
GROUP BY 
    product_id;
```

Этот запрос возвращает общее количество продаж, а также минимальное, максимальное и среднее значение суммы продаж для каждого продукта.

## Обработка NULL значений

Функции `MIN()` и `MAX()` игнорируют значения NULL при вычислении. Если все значения в столбце NULL, результат будет NULL.

### Пример 4: Работа с NULL значениями

Если в таблице есть значения NULL, их можно обработать следующим образом:

```sql
CREATE TABLE test (
    value Nullable(Float32)
) ENGINE = MergeTree()
ORDER BY value;

INSERT INTO test VALUES (1.0), (2.0), (NULL), (4.0), (NULL);

SELECT 
    MIN(value) AS min_value,
    MAX(value) AS max_value
FROM 
    test;
```

В этом случае результатом будет минимальное значение 1.0 и максимальное значение 4.0, поскольку NULL значения игнорируются.

## Заключение

Функции `MIN()` и `MAX()` в ClickHouse являются мощными инструментами для анализа данных, позволяя быстро находить крайние значения в наборах данных. Их использование в сочетании с оператором `GROUP BY` и другими агрегационными функциями позволяет проводить глубокий анализ данных и получать полезные бизнес-инсайты.

Citations:
[1] https://clickhouse.com/docs/en/sql-reference/aggregate-functions/reference/argmin
[2] https://stackoverflow.com/questions/64448582/find-min-max-with-other-attribute
[3] https://clickhouse.com/docs/en/sql-reference/aggregate-functions/reference/minmap
[4] https://kb.altinity.com/altinity-kb-queries-and-syntax/simplestateif-or-ifstate-for-simple-aggregate-functions/
[5] https://clickhouse.com/docs/en/sql-reference/aggregate-functions/reference/count
[6] https://altinity.com/blog/clickhouse-aggregation-fun-part-2-exploring-and-fixing-performance
[7] https://clickhouse.com/blog/aggregate-functions-combinators-in-clickhouse-for-arrays-maps-and-states
[8] https://stackoverflow.com/questions/44307934/how-to-make-clickhouse-count-function-return-0-in-case-of-zero-matches