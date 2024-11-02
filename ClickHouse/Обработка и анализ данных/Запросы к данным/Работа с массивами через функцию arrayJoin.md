В ClickHouse функция `arrayJoin` и оператор `ARRAY JOIN` позволяют эффективно работать с массивами, раскручивая их значения в строки. Это особенно полезно при анализе данных, когда необходимо извлечь элементы из массивов и связать их с другими данными в таблицах. Давайте рассмотрим, как использовать эти функции для анализа данных.

## Основы работы с массивами

### Создание таблицы с массивами

Для начала создадим таблицу, которая содержит массивы:

```sql
CREATE TABLE example (
    id UInt32,
    tags Array(String),
    values Array(Float64)
) ENGINE = MergeTree()
ORDER BY id;
```

### Вставка данных

Теперь добавим некоторые данные в эту таблицу:

```sql
INSERT INTO example (id, tags, values) VALUES 
(1, ['tag1', 'tag2'], [10.0, 20.0]),
(2, ['tag3'], [30.0]),
(3, ['tag1', 'tag4'], [40.0, 50.0]);
```

## Использование arrayJoin

Функция `arrayJoin` позволяет "раскрутить" массивы в отдельные строки. Например, если мы хотим получить все теги и соответствующие им значения:

```sql
SELECT 
    id,
    arrayJoin(tags) AS tag,
    arrayJoin(values) AS value
FROM example;
```

Однако важно отметить, что использование `arrayJoin` таким образом приведет к созданию декартова произведения между массивами `tags` и `values`, что может не дать ожидаемых результатов.

## Использование ARRAY JOIN

Чтобы избежать проблемы с декартовым произведением и корректно сопоставить элементы из двух массивов, лучше использовать оператор `ARRAY JOIN`:

```sql
SELECT 
    id,
    tag,
    value
FROM example
ARRAY JOIN tags AS tag, values AS value;
```

Этот запрос создаст строки для каждого элемента массива `tags`, сопоставляя их с соответствующими элементами массива `values`.

### Результат

Результат выполнения запроса будет выглядеть следующим образом:

```
┌────id─┬─tag──┬─value─┐
│   1   │ tag1  │  10.0 │
│   1   │ tag2  │  20.0 │
│   2   │ tag3  │  30.0 │
│   3   │ tag1  │  40.0 │
│   3   │ tag4  │  50.0 │
└────────┴───────┴────────┘
```

## Применение пользовательских агрегатных функций

Вы также можете комбинировать `ARRAY JOIN` с пользовательскими агрегатными функциями для более сложного анализа данных. Например, если вы хотите подсчитать общее количество значений для каждого тега:

```sql
SELECT 
    tag,
    SUM(value) AS total_value
FROM example
ARRAY JOIN tags AS tag, values AS value
GROUP BY tag;
```

Этот запрос вернет сумму значений для каждого уникального тега.

## Заключение

Использование функции `arrayJoin` и оператора `ARRAY JOIN` в ClickHouse позволяет эффективно работать с массивами данных, раскручивая их в строки и связывая с другими данными в таблицах. Это значительно упрощает анализ данных и позволяет выполнять более сложные запросы для получения необходимой информации.

Citations:
[1] https://altinity.com/blog/harnessing-the-power-of-clickhouse-arrays-part-1
[2] https://clickhouse.com/docs/en/sql-reference/functions/array-join
[3] http://devdoc.net/database/ClickhouseDocs_19.4.1.3-docs/query_language/functions/array_join/
[4] https://stackoverflow.com/questions/66751468/how-to-use-array-join-in-clickhouse
[5] https://kb.altinity.com/altinity-kb-functions/array-like-memory-usage/
[6] https://clickhouse.com/docs/en/sql-reference/statements/select/array-join
[7] https://clickhouse.com/docs/ru/sql-reference/functions/array-join
[8] https://ivan-shamaev.ru/join-types-in-clickhouse-algorithms-and-optimization-of-sql-queries/