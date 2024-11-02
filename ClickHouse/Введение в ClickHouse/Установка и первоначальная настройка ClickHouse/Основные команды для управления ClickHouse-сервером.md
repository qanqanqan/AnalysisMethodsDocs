Управление ClickHouse-сервером включает в себя различные команды для установки, настройки, управления базами данных, таблицами и пользователями. Ниже приведены основные команды для работы с ClickHouse-сервером, используя как утилиту командной строки (clickhouse-client), так и системные команды для управления сервером.

### 1. Старт и остановка ClickHouse-сервера

Зависит от используемой операционной системы:

#### Для систем на базе Debian/Ubuntu:
```sh
sudo service clickhouse-server start   # Запустить сервер
sudo service clickhouse-server stop    # Остановить сервер
sudo service clickhouse-server restart  # Перезапустить сервер
sudo service clickhouse-server status    # Проверить статус сервера
```

#### Для систем на базе CentOS/Red Hat:
```sh
sudo systemctl start clickhouse-server   # Запустить сервер
sudo systemctl stop clickhouse-server    # Остановить сервер
sudo systemctl restart clickhouse-server  # Перезапустить сервер
sudo systemctl status clickhouse-server    # Проверить статус сервера
```

### 2. Подключение к ClickHouse

Запустите клиент ClickHouse:
```sh
clickhouse-client
```

Для подключения к удаленному серверу:
```sh
clickhouse-client --host <адрес_сервера> --user <пользователь> --password <пароль>
```

### 3. Основные команды SQL для работы с базами данных и таблицами

#### a. Управление базами данных:
```sql
CREATE DATABASE имя_базы;               -- Создание новой базы данных
SHOW DATABASES;                          -- Показать все базы данных
DROP DATABASE имя_базы;                 -- Удаление базы данных (если она пуста)
```

#### b. Управление таблицами:
```sql
CREATE TABLE имя_таблицы (               -- Создание новой таблицы
    id UInt32,
    name String
) ENGINE = MergeTree() 
ORDER BY id;

SHOW TABLES;                             -- Показать все таблицы в текущей базе данных
DROP TABLE имя_таблицы;                  -- Удаление таблицы
```

### 4. Работа с данными

#### a. Вставка данных в таблицы:
```sql
INSERT INTO имя_таблицы (id, name) VALUES (1, 'Example');
```

#### b. Запрос данных:
```sql
SELECT * FROM имя_таблицы;               -- Получить все данные из таблицы
SELECT name FROM имя_таблицы WHERE id = 1;  -- Получить данные с условием
```

### 5. Управление пользователями и привилегиями

#### a. Создание пользователя:
```sql
CREATE USER имя_пользователя IDENTIFIED WITH plaintext_password BY 'пароль';  -- Создание пользователя
```

#### b. Предоставление прав:
```sql
GRANT SELECT ON база.* TO имя_пользователя;   -- Предоставить права на выборку
```

#### c. Удаление пользователя:
```sql
DROP USER имя_пользователя;                   -- Удаление пользователя
```

### 6. Техническое обслуживание

#### a. Оптимизация таблиц:
```sql
OPTIMIZE TABLE имя_таблицы;                 -- Оптимизация таблицы
```

#### b. Мониторинг производительности:
```sql
SHOW TABLE имя_таблицы;                     -- Показать информацию о таблице
SELECT * FROM system.tables WHERE database = 'имя_базы';  -- Показать все таблицы в базе
SELECT * FROM system.query_log;              -- Лог выполненных запросов
```

