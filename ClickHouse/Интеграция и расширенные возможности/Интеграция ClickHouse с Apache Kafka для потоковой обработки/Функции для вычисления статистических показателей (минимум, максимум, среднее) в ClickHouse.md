В ClickHouse, работающем с данными из Apache Kafka, можно использовать различные функции для вычисления статистических показателей, таких как минимум, максимум и среднее значение. Эти функции позволяют эффективно анализировать данные в реальном времени. Ниже представлены основные функции и примеры их использования.

## 1. Основные агрегатные функции

### MIN и MAX

- **MIN()**: Вычисляет минимальное значение в наборе данных.
- **MAX()**: Вычисляет максимальное значение в наборе данных.

### Пример использования

```sql
SELECT 
    product_id,
    MIN(sales) AS min_sales,
    MAX(sales) AS max_sales
FROM sales_data
GROUP BY product_id;
```

Этот запрос возвращает минимальные и максимальные продажи для каждого продукта.

### AVG

- **AVG()**: Вычисляет среднее значение в наборе данных.

### Пример использования

```sql
SELECT 
    product_id,
    AVG(sales) AS avg_sales
FROM sales_data
GROUP BY product_id;
```

Этот запрос возвращает среднее значение продаж для каждого продукта.

## 2. Использование функций с данными из Kafka

При интеграции ClickHouse с Kafka можно использовать эти агрегатные функции для анализа данных, поступающих из потоков. Например, создайте таблицу для чтения данных из Kafka:

```sql
CREATE TABLE kafka_table (
    event_time DateTime,
    product_id Int32,
    sales UInt64
) ENGINE = Kafka 
SETTINGS 
    kafka_broker_list = 'localhost:9092',
    kafka_topic_list = 'sales_data',
    kafka_group_name = 'clickhouse_group',
    kafka_format = 'JSONEachRow';
```

### Пример анализа данных из Kafka

После создания таблицы вы можете использовать стандартные агрегатные функции для анализа полученных данных:

```sql
SELECT 
    product_id,
    MIN(sales) AS min_sales,
    MAX(sales) AS max_sales,
    AVG(sales) AS avg_sales
FROM kafka_table 
GROUP BY product_id;
```

Этот запрос вычисляет минимальные, максимальные и средние продажи для каждого продукта на основе данных, поступающих из Kafka.

## 3. Пользовательские агрегационные функции

Если вам нужны более специфические статистические показатели, вы можете создать пользовательские агрегационные функции. Это может быть полезно для выполнения более сложных расчетов или для оптимизации производительности.

### Пример создания пользовательской функции

Для создания пользовательской функции потребуется написать код на C++, который реализует логику агрегации. Например, для вычисления медианы или других специфических показателей.

## 4. Оптимизация производительности

Для повышения производительности при использовании агрегатных функций в ClickHouse можно применять следующие рекомендации:

- **Используйте `SimpleAggregateFunction`**: Для простых агрегатов, таких как `max` и `min`, использование `SimpleAggregateFunction` может снизить потребление памяти и улучшить производительность.
  
```sql
SELECT 
    product_id,
    maxSimpleState(sales) AS max_sales_state,
    minSimpleState(sales) AS min_sales_state
FROM kafka_table 
GROUP BY product_id;
```

- **Создание материализованных представлений**: Это позволяет заранее агрегировать данные и уменьшить объем вычислений во время реальных запросов.

```sql
CREATE MATERIALIZED VIEW mv_sales_summary 
ENGINE = MergeTree() 
ORDER BY (event_time, product_id) AS 
SELECT 
    event_time,
    product_id,
    SUM(sales) AS total_sales,
    MIN(sales) AS min_sales,
    MAX(sales) AS max_sales,
    AVG(sales) AS avg_sales
FROM kafka_table 
GROUP BY event_time, product_id;
```

## Заключение

ClickHouse предоставляет мощные инструменты для вычисления статистических показателей, таких как минимум, максимум и среднее значение. Интеграция с Apache Kafka позволяет эффективно обрабатывать потоковые данные в реальном времени. Использование стандартных и пользовательских агрегатных функций дает возможность адаптировать систему под специфические аналитические задачи, а оптимизация производительности через `SimpleAggregateFunction` и материализованные представления позволяет значительно улучшить скорость обработки запросов.

Citations:
[1] https://altinity.com/blog/clickhouse-aggregation-fun-part-2-exploring-and-fixing-performance
[2] https://kb.altinity.com/altinity-kb-queries-and-syntax/simplestateif-or-ifstate-for-simple-aggregate-functions/
[3] https://clickhouse.com/docs/en/sql-reference/functions/other-functions
[4] https://www.tencentcloud.com/document/product/1129/44435
[5] https://clickhouse.com/docs/en/sql-reference/aggregate-functions/reference
[6] https://clickhouse.com/blog/aggregate-functions-combinators-in-clickhouse-for-arrays-maps-and-states
[7] https://clickhouse.com/docs/en/sql-reference/aggregate-functions/combinators
[8] https://engineering.oden.io/blog/how-to-write-a-clickhouse-aggregate-function