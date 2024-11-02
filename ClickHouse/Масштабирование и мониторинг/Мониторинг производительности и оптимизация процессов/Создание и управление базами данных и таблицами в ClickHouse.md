Создание и управление базами данных и таблицами в ClickHouse — это важные аспекты работы с этой высокопроизводительной аналитической СУБД. Ниже представлены основные команды и концепции, связанные с созданием и управлением данными в ClickHouse.

## Создание базы данных

Для создания новой базы данных в ClickHouse используется команда `CREATE DATABASE`. Пример:

```sql
CREATE DATABASE my_new_database;
```

Чтобы использовать уже созданную базу данных, применяется команда `USE`:

```sql
USE my_new_database;
```

## Создание таблиц

Таблицы создаются с помощью команды `CREATE TABLE`, где необходимо определить структуру таблицы, включая названия столбцов и их типы данных, а также движок хранения. Пример создания таблицы:

```sql
CREATE TABLE IF NOT EXISTS users (
    id UUID,
    name String,
    age Int32
) ENGINE = MergeTree()
ORDER BY id;
```

### Движки таблиц

ClickHouse поддерживает различные движки для хранения данных, среди которых наиболее популярным является `MergeTree`. Он оптимизирован для работы с большими объемами данных и предлагает функции, такие как партиционирование и индексация.

## Вставка данных

Для добавления данных в таблицу используется команда `INSERT`. Пример вставки одной записи:

```sql
INSERT INTO users (id, name, age) VALUES ('1', 'Мария', 25);
```

Можно также вставлять данные из файла или других источников. Например:

```sql
INSERT INTO users FORMAT CSV < data.csv;
```

## Запросы к данным

Запросы к данным выполняются с помощью команды `SELECT`. Примеры запросов:

- Получение всех записей:
  ```sql
  SELECT * FROM users;
  ```

- Фильтрация записей по условию:
  ```sql
  SELECT name FROM users WHERE age > 20;
  ```

## Обновление и удаление данных

ClickHouse не поддерживает стандартные операции обновления и удаления, как в реляционных базах данных. Вместо этого используются команды для замены или удаления данных:

- Для замены данных можно использовать оператор `REPLACE`:
  ```sql
  REPLACE TABLE users SELECT * FROM users WHERE age < 30;
  ```

- Для удаления записей используется команда `ALTER`:
  ```sql
  ALTER TABLE users DELETE WHERE age < 20;
  ```

## Управление таблицами

### Изменение структуры таблиц

ClickHouse позволяет изменять структуру существующих таблиц. Например, для добавления нового столбца используется команда `ALTER TABLE`:

```sql
ALTER TABLE users ADD COLUMN email String;
```

### Комментарии к таблицам

Можно добавлять комментарии к таблицам и столбцам для улучшения документации:

```sql
ALTER TABLE users COMMENT 'Таблица пользователей';
```

Для получения комментариев можно использовать запрос к системной таблице:

```sql
SELECT name, comment FROM system.tables WHERE name = 'users';
```

## Временные таблицы

ClickHouse поддерживает создание временных таблиц, которые существуют только в рамках текущего сеанса. Они создаются с помощью команды `CREATE TEMPORARY TABLE`:

```sql
CREATE TEMPORARY TABLE temp_users (
    id UInt32,
    name String
) ENGINE = Memory;
```

Эти таблицы автоматически удаляются при завершении сеанса.

## Заключение

Создание и управление базами данных и таблицами в ClickHouse включает в себя использование команд для создания баз данных, определения структуры таблиц, вставки и запроса данных. ClickHouse предлагает мощные инструменты для работы с большими объемами информации благодаря своей колонночной архитектуре и поддержке различных движков хранения.

Citations:
[1] https://timeweb.cloud/tutorials/clickhouse/clickhouse-sozdanie-tablicy
[2] https://blog.skillfactory.ru/clickhouse-baza-dannyh/
[3] https://clickhouse.com/docs/ru/getting-started/tutorial
[4] https://ivan-shamaev.ru/join-types-in-clickhouse-algorithms-and-optimization-of-sql-queries/
[5] https://clickhouse.com/docs/ru/operations/access-rights
[6] https://habr.com/ru/companies/otus/articles/773174/
[7] https://docs.arenadata.io/ru/ADQM/current/how-to/data-querying/query-types/table-functions.html
[8] https://bigdataschool.ru/blog/news/clickhouse/clickhouse-engines.html