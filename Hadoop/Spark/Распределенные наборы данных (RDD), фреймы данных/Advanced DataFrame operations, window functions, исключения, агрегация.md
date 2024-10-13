
## 1. Окна (Window Functions)

Оконные функции позволяют выполнять вычисления по подмножествам строк, связанным с текущей строкой, без необходимости группировки данных. Это полезно для выполнения операций, таких как ранжирование, вычисление скользящих средних и т.д.
**Пример использования оконной функции**:
```python
from pyspark.sql import SparkSession 
from pyspark.sql.window import Window 
from pyspark.sql.functions import rank 
# Создание сессии Spark 
spark = SparkSession.builder.appName("Window Functions Example").getOrCreate() 
# Пример данных 
data = [("Alice", 1), ("Bob", 2), ("Cathy", 3), ("Alice", 4)] 
df = spark.createDataFrame(data, ["Name", "Score"]) 
# Определение окна 
window_spec = Window.partitionBy("Name").orderBy("Score") 
# Применение оконной функции 
ranked_df = df.withColumn("Rank", rank().over(window_spec)) 
ranked_df.show()
```

## 2. Исключения
При работе с DataFrame могут возникать различные исключения, такие как `AnalysisException` или `ClassNotFoundException`. Важно правильно обрабатывать эти исключения для обеспечения надежности приложений.
**Обработка исключений**:
```python
try:     
# Пример SQL-запроса    
df.createOrReplaceTempView("my_table")    
	result_df = spark.sql("SELECT * FROM my_table WHERE non_existing_column > 10") 
except Exception as e:     
	print(f"An error occurred: {e}")`
```

## 3. Аггрегация

Аггрегация позволяет выполнять операции над группами данных, такие как подсчет, сумма, среднее и т.д. Это особенно полезно для анализа больших объемов данных.
**Пример агрегации**:
```python
from pyspark.sql.functions import avg 
# Пример данных 
data = [("Alice", 1), ("Bob", 2), ("Cathy", 3), ("Alice", 4)] 
df = spark.createDataFrame(data, ["Name", "Score"]) 
# Выполнение агрегации 
agg_df = df.groupBy("Name").agg(avg("Score").alias("Average_Score")) 
agg_df.show()
```
---

