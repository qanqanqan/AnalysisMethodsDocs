# Сложные соединения и множественные условия в Spark SQL

В Spark SQL, как и в традиционном SQL, вы можете выполнять сложные соединения таблиц и использовать множественные условия для фильтрации данных. Давайте рассмотрим, как это можно сделать:
Сложные соединения

Сложные соединения могут включать в себя не только равенство, но и другие операторы сравнения, а также комбинации условий.
Пример:
```py
from pyspark.sql import SparkSession
from pyspark.sql import functions as F

spark = SparkSession.builder.appName("ComplexJoins").getOrCreate()
```
#### Создаем два DataFrame для примера
```py
employees = spark.createDataFrame([
    (1, 'Alice', 50000),
    (2, 'Bob', 60000),
    (3, 'Cathy', 55000)
], ["id", "name", "salary"])

departments = spark.createDataFrame([
    (1, 'Engineering', 50000),
    (2, 'Marketing', 60000),
    (3, 'Sales', 55000)
], ["id", "name", "min_salary"])
```
### Сложное соединение с использованием неравенства и логического И
```py
complex_join = employees.join(departments, 
                              (employees.id == departments.id) & 
                              (employees.salary >= departments.min_salary), 
                              "inner")

complex_join.show()
```

## Множественные условия в WHERE и HAVING

Множественные условия можно использовать в WHERE для фильтрации строк перед агрегацией и в HAVING для фильтрации после агрегации.
Использование WHERE:

### Фильтрация до агрегации
```py
filtered_df = employees.where((F.col("salary") > 50000) & (F.col("name").like('A%')))
filtered_df.show()
```
Использование HAVING:

### Фильтрация после агрегации
```py
agg_result = employees.groupBy("name").agg(
    F.avg("salary").alias("avg_salary")
).having((F.col("avg_salary") > 50000) & (F.col("name").like('A%')))

agg_result.show()
```
## Комбинирование JOIN с множественными условиями

Вы можете комбинировать JOIN с дополнительными условиями в WHERE для более сложных запросов:

### Пример соединения с дополнительными условиями в WHERE
```py
join_with_conditions = employees.join(departments, employees.id == departments.id, "left") \
    .where((employees.salary >= departments.min_salary) & 
           (departments.name != "Marketing"))

join_with_conditions.show()
```
### Использование SQL для сложных запросов

Если вы предпочитаете SQL, то Spark SQL позволяет писать запросы в более традиционном стиле:

```py
employees.createOrReplaceTempView("employees")
departments.createOrReplaceTempView("departments")

sql_result = spark.sql("""
SELECT e.name, e.salary, d.name as dept_name
FROM employees e
JOIN departments d ON e.id = d.id AND e.salary >= d.min_salary
WHERE d.name != 'Marketing'
""")

sql_result.show()
```
### Заключение

Spark SQL предоставляет гибкие инструменты для выполнения сложных операций соединения и фильтрации данных. Используя DataFrame API или SQL, вы можете создавать запросы, которые включают множественные условия для JOIN, WHERE, и HAVING, что позволяет глубже анализировать и обрабатывать данные в соответствии с бизнес-логикой ваших задач.