### TTL и стратегии оптимизации хранения данных в ClickHouse

**TTL (Time-to-Live)** в ClickHouse — это механизм, который позволяет автоматически управлять жизненным циклом данных, включая их удаление, перемещение между дисками и агрегацию. Это особенно полезно для оптимизации хранения и повышения производительности запросов.

## Основные возможности TTL

1. **Автоматическое удаление данных**: Вы можете настроить автоматическое удаление строк, которые старше определенного времени. Например, чтобы удалить записи старше одного месяца:

   ```sql
   CREATE TABLE events (
       event String,
       time DateTime,
       value UInt64
   ) ENGINE = MergeTree
   ORDER BY (event, time)
   TTL time + INTERVAL 1 MONTH DELETE;
   ```

2. **Перемещение данных**: TTL также позволяет перемещать данные между дисками или томами по мере их старения. Например, вы можете переместить данные на менее быстрые диски после определенного времени:

   ```sql
   TTL time + INTERVAL 1 MONTH TO VOLUME 'slow_volume';
   ```

3. **Агрегация данных**: Вы можете агрегировать данные перед их удалением, сохраняя только необходимые метрики. Например:

   ```sql
   CREATE TABLE hits (
       timestamp DateTime,
       count UInt64
   ) ENGINE = MergeTree
   ORDER BY timestamp
   TTL timestamp + INTERVAL 1 MONTH 
   GROUP BY toStartOfMonth(timestamp) 
   SELECT SUM(count) AS total_count;
   ```

## Стратегии оптимизации хранения данных

### 1. Использование многотомного хранилища

ClickHouse поддерживает многотомное хранилище, что позволяет распределять данные по различным дискам в зависимости от их скорости и ёмкости. Это может быть полезно для реализации архитектуры "горячего/тёплого/холодного" хранения.

```xml
<storage_configuration>
    <disks>
        <hot_disk>
            <path>/data/hot/</path>
        </hot_disk>
        <warm_disk>
            <path>/data/warm/</path>
        </warm_disk>
        <cold_disk>
            <path>/data/cold/</path>
        </cold_disk>
    </disks>
    <policies>
        <default>
            <volumes>
                <hot_volume>
                    <disk>hot_disk</disk>
                </hot_volume>
                <warm_volume>
                    <disk>warm_disk</disk>
                </warm_volume>
                <cold_volume>
                    <disk>cold_disk</disk>
                </cold_volume>
            </volumes>
        </default>
    </policies>
</storage_configuration>
```

### 2. Настройка параметров таблицы

Некоторые параметры таблицы могут помочь оптимизировать производительность и управление данными:

- **`merge_with_ttl_timeout`**: Определяет минимальное время между выполнением операций удаления по TTL. По умолчанию это значение составляет 24 часа, но его можно уменьшить для более частого выполнения операций удаления.

```sql
ALTER TABLE my_table MODIFY SETTING merge_with_ttl_timeout = 3600; -- каждые 60 минут
```

- **`min_rows_for_wide_part`** и **`min_bytes_for_wide_part`**: Эти настройки помогают избежать ненужных преобразований между компактным и широким форматами при вставке больших объемов данных.

### 3. Пакетная вставка данных

Использование пакетной вставки вместо индивидуальных вставок может значительно снизить нагрузку на CPU и улучшить производительность запросов:

```sql
INSERT INTO sales (product_id, sale_amount, sale_date) VALUES 
(1, 100.0, '2024-01-01'),
(2, 150.0, '2024-01-02'),
(3, 200.0, '2024-01-03');
```

### 4. Мониторинг состояния таблиц

Регулярный мониторинг состояния таблиц с помощью запросов к системным таблицам может помочь выявить проблемы с производительностью и управлением данными:

```sql
SELECT name, disk_name 
FROM system.parts 
WHERE table = 'my_table' AND active = 1;
```

## Заключение

TTL в ClickHouse предоставляет мощные инструменты для управления жизненным циклом данных, включая автоматическое удаление и перемещение данных между разными уровнями хранения. Оптимизация хранения данных с использованием многотомного хранилища и правильной настройки параметров таблиц может значительно повысить производительность системы и упростить управление данными.

Citations:
[1] https://altinity.com/blog/2020-3-23-putting-things-where-they-belong-using-new-ttl-moves
[2] https://www.highlight.io/blog/lw5-clickhouse-performance-optimization
[3] https://clickhouse.com/blog/using-ttl-to-manage-data-lifecycles-in-clickhouse
[4] https://clickhouse.com/docs/en/guides/developer/ttl
[5] https://kb.altinity.com/altinity-kb-queries-and-syntax/ttl/modify-ttl/
[6] https://chistadata.com/knowledge-base/set-ttl-for-system-tables-for-storage-optimisation-and-managing-data-retention/
[7] https://kb.altinity.com/altinity-kb-functions/array-like-memory-usage/
[8] https://clickhouse.com/docs/en/engines/table-engines/mergetree-family/mergetree