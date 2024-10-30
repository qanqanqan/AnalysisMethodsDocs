# Использование функций окна и условий HAVING в Spark SQL
Функции окна (Window Functions) и условие HAVING являются мощными инструментами в SQL для выполнения сложных аналитических запросов. В Spark SQL они используются для выполнения вычислений через набор строк, связанных с текущей строкой, и для фильтрации агрегированных результатов соответственно. Вот как их можно использовать:
Функции окна

Функции окна позволяют выполнять агрегатные вычисления (например, SUM, AVG, RANK) через окно строк, которое может быть определено по определенным критериям.
Пример использования:
```py
from pyspark.sql import SparkSession
from pyspark.sql import functions as F
from pyspark.sql.window import Window

spark = SparkSession.builder.appName("WindowFunctions").getOrCreate()
```
#### Создаем DataFrame для примера
```py
data = [
    ('Alice', 'Sales', 3000),
    ('Bob', 'Sales', 2500),
    ('Cathy', 'Marketing', 4000),
    ('David', 'Marketing', 3500),
    ('Eve', 'Sales', 2800)
]

df = spark.createDataFrame(data, ["name", "department", "salary"])
```
#### Определяем окно по департаменту
```py
windowSpec = Window.partitionBy("department").orderBy("salary")
```
#### Применяем функцию окна для расчета ранга сотрудника в своем департаменте по зарплате
```py
df_with_rank = df.withColumn("rank", F.rank().over(windowSpec))

df_with_rank.show()
```
## Условие HAVING

Условие HAVING используется в SQL для фильтрации результатов агрегатных функций, аналогично тому, как WHERE используется для фильтрации отдельных строк.
Пример использования:

#### Используем HAVING для фильтрации департаментов с суммарной зарплатой больше 6000
```py
result = df.groupBy("department").agg(
    F.sum("salary").alias("total_salary")
).filter(F.col("total_salary") > 6000)

result.show()
```
#### То же самое, но с использованием SQL и HAVING
```py
df.createOrReplaceTempView("employees")

sql_result = spark.sql("""
SELECT department, SUM(salary) as total_salary
FROM employees
GROUP BY department
HAVING SUM(salary) > 6000
""")

sql_result.show()
```
Комбинирование оконных функций и HAVING

Вы можете комбинировать оконные функции с HAVING для более сложных аналитических запросов:

#### Создаем окно для расчета средней зарплаты по департаменту
```py
windowSpecAvg = Window.partitionBy("department")
```
#### Добавляем столбец со средней зарплатой по департаменту
```py
f_with_avg = df.withColumn("avg_dept_salary", F.avg("salary").over(windowSpecAvg))
```
#### Теперь используем HAVING, чтобы отфильтровать департаменты, где средняя зарплата выше определенного порога
```py
df_with_avg.createOrReplaceTempView("employees_with_avg")

result = spark.sql("""
SELECT department, AVG(salary) as dept_avg_salary
FROM employees_with_avg
GROUP BY department
HAVING AVG(salary) > 3000
""")

result.show()
```