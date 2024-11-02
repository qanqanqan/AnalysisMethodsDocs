Фильтрация данных в ClickHouse с использованием оператора `WHERE` позволяет эффективно отбирать нужные строки из таблиц на основе заданных условий. Оператор `WHERE` является важной частью SQL-запросов, так как он определяет, какие записи будут включены в результирующий набор.

## Основы использования оператора WHERE

### Синтаксис

Оператор `WHERE` используется в запросах `SELECT`, чтобы задать условия фильтрации:

```sql
SELECT columns
FROM table
WHERE condition;
```

### Пример

Допустим, у вас есть таблица `numbers`, и вы хотите выбрать все числа, которые больше 10 и кратны 3:

```sql
SELECT number 
FROM numbers(20) 
WHERE (number > 10) AND (number % 3 = 0);
```

Этот запрос вернет следующие результаты:

```
┌─number─┐
│ 12     │
│ 15     │
│ 18     │
└────────┘
```

### Условия фильтрации

В условии `WHERE` можно использовать различные операторы, такие как:

- **Сравнения**: `=`, `!=`, `<`, `>`, `<=`, `>=`.
- **Логические операторы**: `AND`, `OR`, `NOT`.
- **Проверка на NULL**: Используйте `IS NULL` и `IS NOT NULL` для проверки значений на наличие или отсутствие.

#### Пример проверки на NULL

Если у вас есть таблица с nullable значениями, например:

```sql
CREATE TABLE t_null(x Int8, y Nullable(Int8)) ENGINE=MergeTree() ORDER BY x;
INSERT INTO t_null VALUES (1, NULL), (2, 3);
```

Вы можете выполнить следующий запрос для фильтрации по NULL значениям:

```sql
SELECT * FROM t_null WHERE y IS NULL;
```

Результат будет таким:

```
┌─x─┬────y─┐
│ 1 │ ᴺᵁᴸᴸ │
└───┴──────┘
```

## Оптимизация фильтрации с помощью PREWHERE

ClickHouse предлагает оптимизацию фильтрации под названием **PREWHERE**, которая позволяет более эффективно обрабатывать запросы. Она автоматически перемещает часть условий из секции `WHERE` в стадию предварительной фильтрации.

### Пример использования PREWHERE

```sql
SELECT count()
FROM mydata 
PREWHERE B = 0 
WHERE C = 'x';
```

В этом примере сначала будут прочитаны только те строки, где условие `B = 0` истинно, что может значительно сократить объем данных для дальнейшей обработки.

## Заключение

Оператор `WHERE` в ClickHouse является мощным инструментом для фильтрации данных. Он позволяет задавать сложные условия с использованием различных операторов и оптимизировать выполнение запросов с помощью механизма PREWHERE. Правильное использование этих возможностей помогает значительно улучшить производительность запросов и снизить нагрузку на систему.

Citations:
[1] https://clickhouse.com/docs/en/sql-reference/statements/select/where
[2] https://clickhouse.com/docs/en/sql-reference/statements/select/prewhere
[3] https://clickhouse.com/docs/en/sql-reference/statements/select/join
[4] https://clickhouse.com/docs/en/sql-reference/operators/in
[5] https://stackoverflow.com/questions/62972939/whereclause-with-substring-in-clickhouse
[6] https://clickhouse.com/docs/ru/sql-reference/statements/select/where
[7] https://altinity.com/blog/harnessing-the-power-of-clickhouse-arrays-part-1
[8] https://clickhouse.com/docs/en/sql-reference/statements/select/array-join