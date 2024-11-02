Оконные функции в ClickHouse предоставляют мощный инструмент для выполнения аналитических вычислений на наборе строк, позволяя выполнять сложные операции, такие как ранжирование, вычисление скользящих средних и нарастающих итогов. Эти функции особенно полезны при анализе данных, поступающих из потоковых источников, таких как Apache Kafka.

## Основы оконных функций в ClickHouse

### Синтаксис оконных функций

Синтаксис для использования оконных функций в ClickHouse выглядит следующим образом:

```sql
SELECT
    column_list,
    window_function_name(argument_list) OVER (
        [PARTITION BY partition_column_list]
        [ORDER BY order_column_list]
        [ROWS BETWEEN start_offset AND end_offset]
    ) AS alias_name
FROM table_name;
```

- **column_list**: Список возвращаемых столбцов.
- **window_function_name**: Имя оконной функции (например, `ROW_NUMBER()`, `SUM()`, `AVG()` и т.д.).
- **PARTITION BY**: Разделяет данные на группы для выполнения вычислений.
- **ORDER BY**: Указывает порядок строк в каждой группе.
- **ROWS BETWEEN**: Определяет диапазон строк для вычисления.

### Примеры использования

#### 1. Ранжирование

Чтобы ранжировать продукты по объему продаж, можно использовать функцию `ROW_NUMBER()`:

```sql
SELECT 
    product_id,
    sales,
    ROW_NUMBER() OVER (ORDER BY sales DESC) AS rank
FROM sales_data;
```

Этот запрос вернет список продуктов с их объемом продаж и рангом по убыванию.

#### 2. Скользящее среднее

Для расчета скользящего среднего цен акций за последние 50 дней можно использовать:

```sql
SELECT 
    date,
    close,
    AVG(close) OVER (ORDER BY date ROWS BETWEEN 49 PRECEDING AND CURRENT ROW) AS moving_average
FROM stock_prices;
```

Этот запрос вычисляет 50-дневную скользящую среднюю цены закрытия акций.

#### 3. Нарастающий итог

Чтобы получить нарастающий итог по продажам:

```sql
SELECT 
    date,
    SUM(sales) OVER (ORDER BY date) AS cumulative_sales
FROM sales_data;
```

Этот запрос возвращает дату и нарастающий итог продаж по дням.

## Интеграция с Kafka

При работе с данными из Kafka в ClickHouse использование оконных функций может быть особенно полезным для анализа потоков данных. Например, вы можете создать таблицу, которая будет получать данные из Kafka и затем применять к ним оконные функции.

### Пример создания таблицы Kafka

Создайте таблицу для чтения данных из топика Kafka:

```sql
CREATE TABLE kafka_table (
    event_time DateTime,
    product_id Int32,
    sales UInt64
) ENGINE = Kafka 
SETTINGS 
    kafka_broker_list = 'localhost:9092',
    kafka_topic_list = 'sales_topic',
    kafka_group_name = 'clickhouse_group',
    kafka_format = 'JSONEachRow';
```

### Использование оконных функций с данными из Kafka

После создания таблицы вы можете использовать оконные функции для анализа данных, поступающих из Kafka:

```sql
CREATE MATERIALIZED VIEW mv_sales_analysis 
ENGINE = MergeTree() 
ORDER BY event_time AS 
SELECT 
    event_time,
    product_id,
    SUM(sales) OVER (PARTITION BY product_id ORDER BY event_time) AS cumulative_sales
FROM kafka_table;
```

Этот материализованный вид будет автоматически обновляться при поступлении новых данных из Kafka и вычислять нарастающий итог продаж для каждого продукта.

## Заключение

Оконные функции в ClickHouse представляют собой мощный инструмент для выполнения сложного анализа данных. Их использование в сочетании с данными из Apache Kafka позволяет эффективно обрабатывать и анализировать потоки информации в реальном времени. Благодаря возможности ранжирования, вычисления скользящих средних и нарастающих итогов, аналитики могут принимать более обоснованные решения на основе актуальных данных.

Citations:
[1] https://nuancesprog.ru/p/17433/
[2] https://docs.arenadata.io/ru/ADQM/current/tutorials/integrations/ads/kafka.html
[3] https://bigdataschool.ru/blog/news/clickhouse/from-kafka-to-clickhouse-integration-latency.html
[4] https://bigdataschool.ru/blog/clickhouse-kafka-integration.html
[5] https://habr.com/ru/companies/otus/articles/569640/
[6] https://bigdataschool.ru/blog/real-time-analytics-on-clickhouse-kafka-spark-and-aws-s3.html
[7] https://clickhouse.com/docs/ru/engines/table-engines/integrations/kafka
[8] https://slurm.io/blog/tpost/sjfdm0r0m1-kafka-lamoda-i-nepreodolimoe-zhelanie-uc