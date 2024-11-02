Агрегационные функции в ClickHouse играют ключевую роль в обработке и анализе данных, позволяя выполнять различные операции над наборами данных. ClickHouse поддерживает как стандартные агрегатные функции, так и специфические для своей архитектуры функции, что делает его мощным инструментом для аналитики.

## Основные агрегатные функции

ClickHouse предоставляет широкий набор агрегатных функций, включая:

- **Стандартные функции**:
  - `count()`: подсчет количества значений.
  - `sum()`: сумма значений.
  - `avg()`: среднее значение.
  - `min()`, `max()`: минимальное и максимальное значения.

- **Специфические функции ClickHouse**:
  - `uniq()`: подсчет уникальных значений (с оценкой).
  - `uniqExact()`: точный подсчет уникальных значений.
  - `quantile()`, `quantiles()`: вычисление квантилей.
  - `groupArray()`: сбор значений в массив по группам[4][5].

## Использование агрегатных функций

### Вставка данных

Для вставки данных с использованием агрегатных функций необходимо использовать функции с суффиксом `-State`, которые возвращают состояние агрегата. Например:
```sql
INSERT INTO table_name SELECT uniqState(UserID) AS state FROM source_table GROUP BY RegionID;
```

### Запросы на выборку

При выборке из таблиц, использующих агрегатные функции, необходимо применять соответствующие функции с суффиксом `-Merge` для получения окончательных результатов:
```sql
SELECT uniqMerge(state) FROM (SELECT uniqState(UserID) AS state FROM table_name GROUP BY RegionID);
```

## Комбинаторы агрегатов

ClickHouse также поддерживает комбинаторы, которые позволяют комбинировать несколько агрегатных функций. Это расширяет возможности аналитических запросов. Например:
- `sumIf()`: условная сумма.
- `countIf()`: условный подсчет.
- Использование комбинаторов с массивами: `sumArrayIf()`[2].

### Пример использования комбинаторов
```sql
SELECT sumIf(total_amount, status = 'confirmed') AS confirmed_total FROM payments;
```

## Таблицы AggregatingMergeTree

Для инкрементальной агрегации данных в ClickHouse используются таблицы типа `AggregatingMergeTree`. Эти таблицы позволяют хранить состояния агрегатных функций и эффективно обрабатывать данные. Пример создания таблицы:
```sql
CREATE TABLE test.agg_visits (
    StartDate DateTime64 NOT NULL,
    CounterID UInt64,
    Visits AggregateFunction(sum, Nullable(Int32)),
    Users AggregateFunction(uniq, Nullable(Int32))
) ENGINE = AggregatingMergeTree() ORDER BY (StartDate, CounterID);
```

### Создание материализованного представления
Для автоматической агрегации данных можно создать материализованное представление:
```sql
CREATE MATERIALIZED VIEW test.visits_mv TO test.agg_visits AS 
SELECT StartDate, CounterID, sumState(Sign) AS Visits, uniqState(UserID) AS Users 
FROM test.visits GROUP BY StartDate, CounterID;
```

## Заключение

Агрегационные функции в ClickHouse предоставляют мощные инструменты для анализа данных. Их использование позволяет эффективно обрабатывать большие объемы информации и получать аналитические результаты в реальном времени. Возможности комбинирования функций и использования специализированных таблиц делают ClickHouse идеальным выбором для аналитических задач.

Citations:
[1] https://clickhouse.com/docs/en/sql-reference/data-types/aggregatefunction
[2] https://clickhouse.com/blog/aggregate-functions-combinators-in-clickhouse-for-arrays-maps-and-states
[3] https://clickhouse.com/docs/en/engines/table-engines/mergetree-family/aggregatingmergetree
[4] https://clickhouse.com/docs/en/sql-reference/aggregate-functions/reference
[5] https://altinity.com/blog/clickhouse-aggregation-fun-part-2-exploring-and-fixing-performance
[6] https://clickhouse.com/docs/en/sql-reference/data-types/simpleaggregatefunction
[7] https://clickhouse.com/docs/ru/sql-reference/aggregate-functions/reference
[8] https://habr.com/ru/companies/otus/articles/569640/