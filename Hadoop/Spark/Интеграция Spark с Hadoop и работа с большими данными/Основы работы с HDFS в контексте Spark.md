Основы работы с HDFS в контексте Spark заключаются в интеграции Spark с Hadoop Distributed File System (HDFS) для работы с большими объемами данных. HDFS используется как распределенная файловая система для хранения данных, а Spark может легко взаимодействовать с ней для чтения и записи данных.

Основные шаги работы с HDFS в Spark:

 1. Чтение данных из HDFS: Spark поддерживает чтение данных из HDFS с использованием метода spark.read для различных форматов файлов, таких как текстовые файлы, CSV, Parquet и другие.
 2. Запись данных в HDFS: После обработки данных в Spark их можно сохранить обратно в HDFS с использованием метода write.

Пример работы с HDFS в PySpark:
```python
# Чтение данных из HDFS (например, CSV-файл)
df = spark.read.csv("hdfs://namenode:9000/user/hadoop/input/data.csv", header=True, inferSchema=True)

# Выполнение операций с данными (например, фильтрация)
filtered_df = df.filter(df["age"] > 30)

# Запись результата обратно в HDFS в формате Parquet
filtered_df.write.parquet("hdfs://namenode:9000/user/hadoop/output/filtered_data.parquet")
```
В этом примере Spark читает CSV-файл из HDFS, обрабатывает данные (фильтрует строки) и записывает результат обратно в HDFS в формате Parquet.