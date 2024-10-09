Apache Spark и Hadoop — это два популярных инструмента для обработки и анализа больших данных.

 1. Apache Hadoop — это фреймворк для распределённого хранения и обработки больших данных. Основные компоненты Hadoop:
 • HDFS (Hadoop Distributed File System) — распределённая файловая система, которая хранит данные на кластере.
 • MapReduce — модель программирования для обработки данных в распределённом виде.
 • YARN (Yet Another Resource Negotiator) — система управления ресурсами в кластере.
 2. Apache Spark — это платформа для распределённой обработки данных, которая, в отличие от MapReduce, выполняет задачи быстрее за счёт того, что обрабатывает данные в оперативной памяти (in-memory). Основные особенности Spark:
 • Resilient Distributed Datasets (RDD) — базовая структура данных в Spark, которая позволяет распределённо хранить и обрабатывать большие объёмы информации.
 • Поддержка различных API для работы с SQL, потоками данных (Streaming), машинным обучением (MLlib) и графами (GraphX).
 • Интеграция с HDFS — Spark может работать с данными, хранящимися в HDFS, что делает его совместимым с Hadoop.

Пример: Чтение данных из HDFS с использованием PySpark
```python
from pyspark.sql import SparkSession

# Создание SparkSession
spark = SparkSession.builder \
    .appName("HDFS Example") \
    .getOrCreate()

# Чтение данных из HDFS
df = spark.read.csv("hdfs://namenode:8020/path/to/data.csv")

# Вывод первых 5 строк
df.show(5)
```
В этом примере Spark интегрируется с Hadoop через HDFS, позволяя эффективно работать с большими данными.