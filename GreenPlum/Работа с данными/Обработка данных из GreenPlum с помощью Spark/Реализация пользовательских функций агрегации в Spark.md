## Реализация пользовательских функций агрегации в Spark

В Apache Spark вы можете реализовать пользовательские функции агрегации, чтобы проводить более сложные операции над данными в DataFrame или во время выполнения SQL-запросов. Это позволяет адаптировать методы агрегации под конкретные бизнес-требования и логики обработки данных.

Ниже представлен алгоритм реализации пользовательских функций агрегации в Spark:

### 1. Пользовательские функции агрегации с использованием udaf

Вы можете создать пользовательскую функцию агрегации, используя UserDefinedAggregateFunction (UDAF). Это позволяет вам описать логику агрегации в виде класса.

#### Пример: создание UDAF

Вот пример, где мы создаём функцию, которая вычисляет среднее значение по произвольной колонке:

```py
from pyspark.sql import SparkSession
from pyspark.sql.types import StructType, StructField, DoubleType, LongType, StringType
from pyspark.sql.functions import col
from pyspark.sql import Row
from pyspark.sql.types import DoubleType, LongType
from pyspark.sql.expressions import UserDefinedAggregateFunction

# Создание SparkSession
spark = SparkSession.builder \
    .appName("Custom Agg Function Example") \
    .getOrCreate()

# Определение пользовательской функции агрегации
class AverageAggregator(UserDefinedAggregateFunction):
    def __init__(self):
        self.inputSchema = StructType([StructField("value", DoubleType())])
        self.bufferSchema = StructType([StructField("sum", DoubleType()), StructField("count", LongType())])
        self.dataType = DoubleType()
        self.deterministic = True

    def initialize(self):
        return Row(0.0, 0)

    def update(self, buffer, input):
        if input is not None:
            buffer.sum += input
            buffer.count += 1

    def merge(self, buffer1, buffer2):
        buffer1.sum += buffer2.sum
        buffer1.count += buffer2.count

    def evaluate(self, buffer):
        if buffer.count == 0:
            return None
        return buffer.sum / buffer.count

# Определение функции
average_udaf = AverageAggregator()

# Создание тестовых данных
data = [(1.0,), (2.0,), (3.0,), (4.0,), (None,)]
schema = StructType([StructField("value", DoubleType())])
df = spark.createDataFrame(data, schema)

# Применение пользовательской функции агрегации
result_df = df.agg(average_udaf(col("value")).alias("average_value"))
result_df.show()

# Завершение работы сессии
spark.stop()
```

### 2. Использование пользовательских функций в SQL

Если вы хотите использовать пользовательские функции в SQL, вам нужно зарегистрировать UDAF:

```py
spark.udf.register("average_udaf", average_udaf)
result_sql_df = spark.sql("SELECT average_udaf(value) as average_value FROM my_table")
result_sql_df.show()
```

### 3. Альтернативный способ: использование agg с groupBy

Иногда в качестве альтернативы пользовательским функциям можно использовать встроенные функции, чтобы достичь аналогичного результата. Например, если вам нужно посчитать сумму и количество:

```py
from pyspark.sql import functions as F

# Пример с использованием встроенных функций
result_df = df.groupBy("some_column").agg(
    F.sum("value").alias("total"),
    F.count("value").alias("count")
).withColumn("average", F.col("total") / F.col("count"))
result_df.show()
```
