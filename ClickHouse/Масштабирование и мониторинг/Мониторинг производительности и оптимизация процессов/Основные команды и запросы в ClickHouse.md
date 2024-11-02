ClickHouse — это аналитическая колонночная база данных, которая поддерживает множество SQL-подобных команд для работы с данными. Ниже представлены основные команды и запросы, которые часто используются в ClickHouse.

## Основные команды ClickHouse

### 1. Создание таблицы

Для создания таблицы используется команда `CREATE TABLE`. Пример:

```sql
CREATE TABLE example (
    id UInt32,
    name String,
    created_at DateTime
) ENGINE = MergeTree()
ORDER BY id;
```

### 2. Вставка данных

Для вставки данных в таблицу используется команда `INSERT`. Пример:

```sql
INSERT INTO example (id, name, created_at) VALUES (1, 'Alice', '2023-01-01 10:00:00');
```

### 3. Запрос данных

Запрос данных выполняется с помощью команды `SELECT`. Пример:

```sql
SELECT * FROM example;
```

Можно также использовать фильтры:

```sql
SELECT name FROM example WHERE id = 1;
```

### 4. Объединение таблиц

ClickHouse поддерживает операции объединения (JOIN). Пример:

```sql
SELECT a.name, b.order_id
FROM users a
JOIN orders b ON a.id = b.user_id
WHERE a.name = 'Alice';
```

### 5. Группировка и агрегация

Для группировки данных и выполнения агрегатных функций используется команда `GROUP BY`. Пример:

```sql
SELECT COUNT(*) AS total_orders
FROM orders
GROUP BY user_id;
```

Можно также использовать `HAVING` для фильтрации агрегированных результатов:

```sql
SELECT user_id, COUNT(*) AS order_count
FROM orders
GROUP BY user_id
HAVING order_count > 10;
```

### 6. Сортировка результатов

Сортировка результатов выполняется с помощью `ORDER BY`. Пример:

```sql
SELECT * FROM example ORDER BY created_at DESC;
```

### 7. Использование подзапросов

ClickHouse поддерживает подзапросы в секции `FROM`. Пример:

```sql
SELECT *
FROM (SELECT id, COUNT(*) AS order_count FROM orders GROUP BY user_id) AS subquery
WHERE order_count > 5;
```

### 8. Общие табличные выражения (CTE)

ClickHouse поддерживает общие табличные выражения с помощью секции `WITH`. Пример:

```sql
WITH total_sales AS (
    SELECT SUM(amount) AS total FROM sales
)
SELECT *
FROM total_sales;
```

## Заключение

ClickHouse предоставляет мощный набор команд и запросов для работы с данными, включая создание таблиц, вставку данных, выполнение сложных запросов с объединениями и агрегациями, а также использование подзапросов и общих табличных выражений. Эти команды позволяют эффективно управлять большими объемами данных и обеспечивать высокую производительность при выполнении аналитических задач.

Citations:
[1] https://bigdataschool.ru/blog/clickhouse-playground-practice.html
[2] https://clickhouse.com/docs/ru/sql-reference/statements/select/with
[3] https://ivan-shamaev.ru/clickhouse-101-course-on-learn-clickhouse-com/
[4] https://habr.com/ru/articles/509540/
[5] https://stupin.su/wiki/clickhouse_config/
[6] https://clickhouse.com/docs/ru/introduction/distinctive-features
[7] https://clickhouse.com/docs/ru/engines/table-engines/mergetree-family/mergetree
[8] https://clickhouse.com/docs/ru/sql-reference/statements/select