## Таблицы и хранение данных в Hive

В Apache Hive организации таблиц и хранения данных играют ключевую роль в управлении большими объемами данных и оптимизации производительности запросов. Hive использует концепцию таблиц для представления данных, что позволяет пользователю работать с ними через SQL-подобный язык HiveQL. Ниже приведены основные аспекты, связанные с таблицами и хранением данных в Hive.

### Основные концепции таблиц в Hive:

1. Типы таблиц:
- Внутренние таблицы (Managed Tables): Hive полностью управляет данными, которые находятся в этих таблицах. При удалении внутренней таблицы Hive также удаляет связанные с ней данные из HDFS.
- Внешные таблицы (External Tables): Эти таблицы ссылаются на данные, находящиеся вне Hive. Если внешняя таблица удаляется, сами данные не удаляются. Это позволяет использовать одно и то же множество данных из разных источников.

2. Определение таблицы:
- Таблицы создаются с помощью команды `CREATE TABLE`, после чего задаются столбцы и их типы данных, а также другие параметры, такие как порядок хранения и партиционирование.

```sql
CREATE TABLE example
 (id INT, name STRING)
 ROW FORMAT DELIMITED
 FIELDS TERMINATED BY ',&039;
 STORED AS TEXTFILE;
```

3. Партиционирование:
- Hive поддерживает партиционирование, что позволяет разбивать таблицы на подкатегории для улучшения производительности запросов. Например, можно разбить таблицу по дате или региону. Партиционирование помогает существенно уменьшить количество данных для обработки во время выполнения запросов.

```sql
CREATE TABLE sales
 (id INT, amount FLOAT)
 PARTITIONED BY (year INT, month INT);
```

- Для работы с партиционированными таблицами необходимо использовать команды `ALTER TABLE` и `ADD PARTITION` для добавления новых партиций.

4. Кластеризация:
- Кластеризация позволяет разбивать данные в пределах партиции для более эффективного доступа. При создании таблицы можно указать порядок хранения данных в кластерах с использованием CLUSTERED BY и SORTED BY.

5. Форматы хранения данных:
- Hive поддерживает несколько форматов хранения данных, включая:
- TextFile: стандартный текстовый файл, разделенный запятыми или другими символами.
- SequenceFile: бинарный формат, эффективный для хранения больших объемов данных.
- ORC (Optimized Row Columnar): формат, оптимизированный для обработки больших данных, обеспечивает высокую производительность и лучшее сжатие.
- Parquet: колонно-ориентированный формат, также обеспечивающий хорошее сжатие и оптимизацию чтения.
- Avro: формат, подходящий для пациентов и обработки данных.

### Основные команды для работы с таблицами в Hive:

1. Создание таблицы:

```sql
CREATE TABLE table_name (column1 datatype, column2 datatype) STORED AS file_format;
```

2. Загрузка данных в таблицу:

```sql
LOAD DATA INPATH '/path/to/data' INTO TABLE table_name;
```

3. Удаление таблицы:

```sql
DROP TABLE table_name;
```

4. Запрос данных:

```sql
SELECT * FROM table_name WHERE condition;
```