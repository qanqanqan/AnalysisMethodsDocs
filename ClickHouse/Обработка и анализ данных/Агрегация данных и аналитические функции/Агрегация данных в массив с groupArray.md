Агрегация данных в массив с помощью функции `groupArray` в ClickHouse позволяет собирать значения из определенного столбца в массив для каждой группы, определенной условием `GROUP BY`. Эта функция полезна для анализа данных и создания отчетов.

## Синтаксис

Функция `groupArray` имеет следующий синтаксис:

```sql
groupArray(x) 
```
или
```sql
groupArray(max_size)(x)
```

- **`x`** — это значение или выражение, которое будет добавлено в массив.
- **`max_size`** — необязательный параметр, который ограничивает размер результирующего массива до указанного количества элементов. Например, `groupArray(10)(x)` создаст массив, содержащий не более 10 элементов.

## Пример использования

Рассмотрим таблицу с данными:

```
┌─id─┬─name─────┐
│ 1 │ zhangsan │
│ 1 │ ᴺᵁᴸᴸ │
│ 1 │ lisi     │
│ 2 │ wangwu   │
└────┴──────────┘
```

Для агрегации имен по идентификатору можно использовать следующий запрос:

```sql
SELECT id, groupArray(name) FROM default.ck GROUP BY id;
```

Результат будет следующим:

```
┌─id─┬─groupArray(name)───────┐
│ 1 │ ['zhangsan', 'lisi']    │
│ 2 │ ['wangwu']              │
└────┴─────────────────────────┘
```

Обратите внимание, что функция `groupArray` автоматически исключает значения `NULL` из результирующего массива. В данном примере значение `ᴺᵁᴸᴸ` было проигнорировано.

## Применение в аналитике

Функция `groupArray` может быть полезна в различных сценариях анализа данных, таких как:

- **Составление отчетов**: Сбор значений по категориям для дальнейшего анализа.
- **Анализ последовательностей событий**: Например, для отслеживания действий пользователей на веб-сайте или в приложении.
- **Фуннельный анализ**: Определение этапов, через которые проходит пользователь до выполнения целевого действия.

### Пример с ограничением размера

Если необходимо ограничить размер массива, можно использовать следующую конструкцию:

```sql
SELECT id, groupArray(2)(name) FROM default.ck GROUP BY id;
```

Это создаст массив с максимум двумя элементами для каждого идентификатора.

## Заключение

Функция `groupArray` в ClickHouse является мощным инструментом для агрегации данных и создания массивов значений по группам. Она позволяет эффективно обрабатывать и анализировать большие объемы данных, что делает ее незаменимой в аналитических приложениях.

Citations:
[1] https://clickhouse.com/docs/en/sql-reference/aggregate-functions/reference/grouparray
[2] https://altinity.com/blog/harnessing-the-power-of-clickhouse-arrays-part-2
[3] https://clickhouse.com/docs/en/sql-reference/aggregate-functions/reference/grouparraysample
[4] https://clickhouse.com/docs/en/sql-reference/aggregate-functions
[5] https://stackoverflow.com/questions/59316326/how-initialize-the-result-of-grouparray-function-of-the-clickhouse-to-the-array
[6] https://clickhouse.com/docs/en/sql-reference/aggregate-functions/combinators
[7] https://github.com/grafana/clickhouse-datasource/issues/166
[8] https://clickhouse.com/docs/ru/sql-reference/aggregate-functions/reference/grouparray