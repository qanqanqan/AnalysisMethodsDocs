ClickHouse использует SQL-подобный язык для обработки данных, который включает в себя множество стандартных операторов и конструкций. Ниже представлен базовый синтаксис SQL-запросов в ClickHouse, включая примеры использования.

## Основные операторы SQL в ClickHouse

### 1. Создание таблицы

Для создания таблицы используется команда `CREATE TABLE`. Синтаксис похож на стандартный SQL:

```sql
CREATE TABLE IF NOT EXISTS users (
    id UUID,
    name String,
    age Int32
) ENGINE = MergeTree()
ORDER BY id;
```

В этом примере создается таблица `users` с тремя полями: `id`, `name` и `age`.

### 2. Вставка данных

Для добавления данных в таблицу используется команда `INSERT INTO`:

```sql
INSERT INTO users (id, name, age) VALUES ('1', 'Мария', 25);
```

Эта команда добавляет нового пользователя с именем "Мария" и возрастом 25 лет.

### 3. Запрос данных

Для выборки данных используется команда `SELECT`. Примеры запросов:

- Выбор всех полей из таблицы:
  
```sql
SELECT * FROM users;
```

- Выбор определенного поля с условием:

```sql
SELECT name FROM users WHERE age = 25;
```

### 4. Обновление данных

ClickHouse поддерживает обновление данных через команду `ALTER TABLE` с использованием подзапроса:

```sql
ALTER TABLE users UPDATE age = 26 WHERE name = 'Мария';
```

### 5. Удаление данных

Удаление записей также осуществляется через команду `ALTER TABLE`:

```sql
ALTER TABLE users DELETE WHERE name = 'Мария';
```

### 6. Группировка и агрегация

ClickHouse позволяет использовать агрегатные функции и группировку:

```sql
SELECT age, COUNT(*) AS count FROM users GROUP BY age;
```

Этот запрос подсчитывает количество пользователей по каждому возрасту.

### 7. Соединение таблиц

ClickHouse поддерживает соединения таблиц через оператор `JOIN`:

```sql
SELECT u.name, o.order_id 
FROM users AS u 
JOIN orders AS o ON u.id = o.user_id 
WHERE u.age > 30;
```

Этот запрос соединяет таблицы `users` и `orders`, выбирая имена пользователей старше 30 лет вместе с их идентификаторами заказов.

### 8. Использование секции WITH

ClickHouse поддерживает общие табличные выражения (Common Table Expressions, CTE) с помощью секции `WITH`, что позволяет использовать результаты подзапросов в основном запросе:

```sql
WITH (SELECT COUNT(*) FROM users) AS total_users
SELECT name, age FROM users WHERE age > (total_users / 2);
```

## Заключение

Синтаксис SQL-запросов в ClickHouse включает в себя основные конструкции для работы с данными, такие как создание таблиц, вставка, обновление и удаление данных, а также выполнение сложных запросов с агрегацией и соединением таблиц. Эти возможности делают ClickHouse мощным инструментом для аналитической обработки больших объемов данных в реальном времени.

Citations:
[1] https://bigdataschool.ru/blog/clickhouse-playground-practice.html
[2] https://bigdataschool.ru/blog/news/clickhouse/clickhouse-sharding.html
[3] https://clickhouse.com/docs/ru/sql-reference/statements/select/with
[4] https://blog.skillfactory.ru/clickhouse-baza-dannyh/
[5] https://habr.com/ru/companies/smi2/articles/317682/
[6] https://dzen.ru/a/ZrCLnHNvQTDonheR
[7] https://clickhouse.com/docs/ru/introduction/distinctive-features
[8] https://stupin.su/wiki/clickhouse_cluster/