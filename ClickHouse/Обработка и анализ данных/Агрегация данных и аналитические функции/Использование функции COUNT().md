Функция `COUNT()` в ClickHouse используется для подсчета количества строк или ненулевых значений в заданном столбце. Она поддерживает несколько синтаксисов и является одной из основных агрегатных функций, используемых в SQL-запросах.

## Синтаксис

ClickHouse поддерживает следующие варианты использования функции `COUNT()`:

1. **Подсчет всех строк**:
   - `COUNT(*)`: Подсчитывает все строки в таблице.
   - `count()`: Альтернативный синтаксис, специфичный для ClickHouse, который также подсчитывает все строки.

2. **Подсчет ненулевых значений**:
   - `COUNT(expr)`: Подсчитывает количество ненулевых значений в указанном выражении.
   - `COUNT(DISTINCT expr)`: Подсчитывает количество уникальных ненулевых значений в указанном выражении.

## Возвращаемое значение

- Если функция вызывается без параметров (например, `COUNT(*)`), она возвращает общее количество строк в таблице.
- Если передано выражение, функция возвращает количество строк, где это выражение не равно NULL. Если все значения NULL, результат будет равен 0.
- Возвращаемый тип значения всегда является `UInt64`.

## Примеры использования

### Пример 1: Подсчет всех строк

```sql
SELECT COUNT(*) FROM my_table;
```

Этот запрос вернет общее количество строк в таблице `my_table`.

### Пример 2: Подсчет ненулевых значений

```sql
SELECT COUNT(column_name) FROM my_table;
```

Этот запрос подсчитает количество ненулевых значений в столбце `column_name`.

### Пример 3: Подсчет уникальных значений

```sql
SELECT COUNT(DISTINCT column_name) FROM my_table;
```

Этот запрос вернет количество уникальных ненулевых значений в столбце `column_name`.

## Оптимизация запросов

ClickHouse оптимизирует выполнение запросов с использованием функции `COUNT()`. Например:

- Запросы типа `SELECT COUNT(*) FROM table` могут быть оптимизированы с использованием метаданных из системы хранения данных (MergeTree).
- Для подсчета ненулевых значений в столбцах с типом Nullable можно включить настройку `optimize_functions_to_subcolumns`, что позволит читать только подстолбец NULL вместо обработки всего столбца.

## Заключение

Функция `COUNT()` является важной частью анализа данных в ClickHouse, позволяя быстро и эффективно подсчитывать строки и значения. Понимание различных синтаксисов и возможностей оптимизации поможет улучшить производительность запросов и сделать анализ данных более эффективным.

Citations:
[1] https://clickhouse.com/docs/en/sql-reference/aggregate-functions/reference/count
[2] https://chistadata.com/aggregate-functions-via-clickhouse/
[3] https://clickhouse.com/blog/aggregate-functions-combinators-in-clickhouse-for-arrays-maps-and-states
[4] https://stackoverflow.com/questions/44307934/how-to-make-clickhouse-count-function-return-0-in-case-of-zero-matches
[5] https://clickhouse.com/docs/ru/sql-reference/aggregate-functions/reference/count
[6] https://kb.altinity.com/altinity-kb-queries-and-syntax/slow_select_count/
[7] https://altinity.com/blog/clickhouse-aggregation-fun-part-2-exploring-and-fixing-performance
[8] https://clickhouse.com/blog/clickhouse_vs_elasticsearch_mechanics_of_count_aggregations