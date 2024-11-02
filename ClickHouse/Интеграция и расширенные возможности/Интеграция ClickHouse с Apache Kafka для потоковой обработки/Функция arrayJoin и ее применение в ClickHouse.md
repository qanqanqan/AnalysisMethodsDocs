Функция `arrayJoin` в ClickHouse используется для "разворачивания" массивов, что позволяет преобразовать массивные данные в строки. Это особенно полезно при работе с таблицами, содержащими массивы, так как позволяет легко анализировать и обрабатывать данные внутри этих массивов. Ниже рассмотрены основные аспекты использования `arrayJoin` и примеры его применения.

## Основы функции arrayJoin

### Синтаксис

Функция `arrayJoin` принимает массив в качестве аргумента и создает новые строки для каждого элемента массива, дублируя значения других столбцов:

```sql
SELECT arrayJoin(array_column) AS new_column
FROM table_name;
```

### Пример использования

Предположим, у вас есть таблица с массивами:

```sql
CREATE TABLE example (
    id Int32,
    values Array(Int32)
) ENGINE = Memory;

INSERT INTO example VALUES (1, [10, 20, 30]), (2, [40, 50]);
```

Чтобы развернуть массив `values` в строки, используйте `arrayJoin`:

```sql
SELECT id, arrayJoin(values) AS value
FROM example;
```

**Результат:**

```
┌─id─┬─value─┐
│  1 │    10 │
│  1 │    20 │
│  1 │    30 │
│  2 │    40 │
│  2 │    50 │
└─────┴────────┘
```

## Использование ARRAY JOIN

ClickHouse также поддерживает оператор `ARRAY JOIN`, который позволяет объединять данные из нескольких массивов одновременно. Это может быть полезно для работы с параллельными массивами.

### Пример использования ARRAY JOIN

Если у вас есть два массива в одной таблице:

```sql
CREATE TABLE multi_example (
    id Int32,
    cities Array(String),
    populations Array(Int32)
) ENGINE = Memory;

INSERT INTO multi_example VALUES (1, ['New York', 'Los Angeles'], [8419600, 3980400]), (2, ['Chicago'], [2716000]);
```

Вы можете использовать `ARRAY JOIN`, чтобы развернуть оба массива:

```sql
SELECT id, ARRAY JOIN cities AS city, populations AS population
FROM multi_example;
```

**Результат:**

```
┌─id─┬───────────────┬──────────────┐
│  1 │ New York      │      8419600 │
│  1 │ Los Angeles   │      3980400 │
│  2 │ Chicago       │      2716000 │
└─────┴───────────────┴──────────────┘
```

## Применение в аналитической обработке данных

Функция `arrayJoin` и оператор `ARRAY JOIN` особенно полезны при работе с данными из Apache Kafka. Например, если вы получаете данные о продажах с тегами в виде массивов, вы можете легко развернуть их для анализа.

### Пример с данными из Kafka

Предположим, у вас есть таблица Kafka с данными о продажах:

```sql
CREATE TABLE kafka_sales (
    sale_id Int32,
    tags Array(String),
    amounts Array(Float64)
) ENGINE = Kafka 
SETTINGS 
    kafka_broker_list = 'localhost:9092',
    kafka_topic_list = 'sales_data',
    kafka_group_name = 'clickhouse_group',
    kafka_format = 'JSONEachRow';
```

Для анализа данных вы можете использовать `ARRAY JOIN`, чтобы получить каждую продажу с соответствующими тегами и суммами:

```sql
SELECT sale_id, ARRAY JOIN tags AS tag, amounts AS amount
FROM kafka_sales;
```

## Заключение

Функция `arrayJoin` и оператор `ARRAY JOIN` в ClickHouse предоставляют мощные инструменты для работы с массивами. Они позволяют эффективно развертывать и анализировать данные внутри массивов, что особенно полезно при интеграции с потоковыми источниками данных, такими как Apache Kafka. Использование этих функций позволяет значительно упростить процесс анализа сложных структур данных и получать ценные аналитические инсайты.

Citations:
[1] https://altinity.com/blog/harnessing-the-power-of-clickhouse-arrays-part-1
[2] https://clickhouse.com/docs/en/sql-reference/functions/array-join
[3] http://devdoc.net/database/ClickhouseDocs_19.4.1.3-docs/query_language/functions/array_join/
[4] https://stackoverflow.com/questions/66751468/how-to-use-array-join-in-clickhouse
[5] https://kb.altinity.com/altinity-kb-functions/array-like-memory-usage/
[6] https://clickhouse.com/docs/en/sql-reference/statements/select/array-join
[7] https://clickhouse.com/blog/aggregate-functions-combinators-in-clickhouse-for-arrays-maps-and-states
[8] https://clickhouse.com/docs/ru/sql-reference/functions/array-join