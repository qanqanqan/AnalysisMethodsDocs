Для подключения к серверу ClickHouse и проверки его статуса можно использовать различные методы, включая командную строку и Python. Ниже приведены основные шаги для выполнения этих операций.

## Подключение к серверу ClickHouse

### 1. Использование командной строки

Для подключения к ClickHouse через командную строку используется клиент `clickhouse-client`. Вот основные параметры для подключения:

- **HOST**: адрес сервера (по умолчанию `localhost`).
- **PORT**: порт сервера (по умолчанию `9000` для TCP и `8123` для HTTP).
- **USERNAME**: имя пользователя (по умолчанию `default`).
- **PASSWORD**: пароль пользователя (если установлен).

#### Пример команды подключения:

```bash
clickhouse-client --host localhost --port 9000 --user default --password
```

Если вы используете ClickHouse Cloud или подключаетесь через HTTPS, команда будет выглядеть так:

```bash
clickhouse-client --host your_host.clickhouse.cloud --secure --port 9440 --user your_user --password
```

### 2. Проверка статуса сервера

После подключения вы можете выполнить простой SQL-запрос, чтобы проверить статус сервера. Например, запрос на получение версии сервера:

```sql
SELECT version();
```

Если сервер работает правильно, вы получите ответ с версией ClickHouse.

## 3. Подключение через Python

Вы также можете подключиться к ClickHouse с помощью Python, используя библиотеку `clickhouse_connect`. Вот пример кода для подключения:

```python
import clickhouse_connect

# Создание клиента
client = clickhouse_connect.get_client(host='localhost', username='default', password='your_password')

# Проверка версии сервера
server_version = client.server_version()
print(f"Connected to ClickHouse server version: {server_version}")
```

Если вы подключаетесь к удаленному серверу, просто измените параметры `host`, `port`, `username`, и `password` в соответствии с вашими настройками.

## Заключение

Подключение к серверу ClickHouse можно осуществить как через командную строку с использованием клиента `clickhouse-client`, так и через Python с помощью библиотеки `clickhouse_connect`. После подключения можно выполнять запросы для проверки статуса сервера и получения информации о его работе.

Citations:
[1] https://clickhouse.com/docs/en/integrations/python
[2] https://clickhouse.com/docs/en/install
[3] https://kb.altinity.com/altinity-kb-setup-and-maintenance/connection-problems/
[4] https://clickhouse.com/docs/en/interfaces/cli
[5] https://stackoverflow.com/questions/52660150/connect-to-remote-clickhouse-db-via-clickhouse-command-line
[6] https://github.com/mymarilyn/clickhouse-driver/issues/185
[7] https://clickhouse.com/docs/en/guides/troubleshooting
[8] https://habr.com/ru/articles/582034/