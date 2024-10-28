Работа с Spark SQL и Hadoop HDFS включает в себя интеграцию Spark с файловой системой HDFS для обработки и анализа больших данных. Вот основные шаги для работы с данными в HDFS через Spark SQL:

 1. Настройка окружения: Убедитесь, что у вас установлены Spark и Hadoop, и они правильно сконфигурированы. Spark должен иметь доступ к HDFS.
 2. Загрузка данных из HDFS: Вы можете загрузить данные из HDFS в Spark DataFrame с помощью метода spark.read. Например, для загрузки CSV-файла:
```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("HDFS Example") \
    .getOrCreate()

# Загрузка данных из HDFS
df = spark.read.csv("hdfs://namenode:port/path/to/file.csv", header=True, inferSchema=True)
df.show()
```

 3. Использование Spark SQL: После загрузки данных в DataFrame вы можете выполнять SQL-запросы. Для этого необходимо зарегистрировать DataFrame как временную таблицу:
```python
df.createOrReplaceTempView("my_table")

# Выполнение SQL-запроса
result = spark.sql("SELECT * FROM my_table WHERE some_column > 100")
result.show()
```

 4. Запись данных обратно в HDFS: После обработки данных вы можете записать результаты обратно в HDFS. Например, для записи DataFrame в CSV:
```python
result.write.csv("hdfs://namenode:port/path/to/output.csv", header=True)
```


Эти шаги позволяют эффективно интегрировать Spark SQL с HDFS для работы с большими объемами данных.