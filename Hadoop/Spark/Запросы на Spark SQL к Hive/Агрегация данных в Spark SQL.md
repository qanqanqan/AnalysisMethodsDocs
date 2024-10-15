# Агрегация данных в Spark SQL

Агрегация данных в Spark SQL является важной операцией при обработке и анализе больших объемов данных. Spark SQL предоставляет ряд встроенных агрегационных функций, которые позволяют вам выполнять различные виды агрегаций.

Вот некоторые из наиболее распространенных агрегационных функций в Spark SQL:

### COUNT: Возвращает количество строк, удовлетворяющих условию.

```py
df = spark.createDataFrame([
    (1, "Alice", 25),
    (2, "Bob", 30),
    (3, "Charlie", 35),
    (4, "David", 40)
], ["id", "name", "age"])

count_df = df.select(count("id")).collect()[0][0]
print(f"Total number of rows: {count_df}")
```
### SUM: Возвращает сумму значений в указанном столбце.

```py
sum_age = df.select(sum("age")).collect()[0][0]
print(f"Total age: {sum_age}")
```
### AVG: Возвращает среднее значение в указанном столбце.
```py
avg_age = df.select(avg("age")).collect()[0][0]
print(f"Average age: {avg_age}")
```
### MAX: Возвращает максимальное значение в указанном столбце.
```py
max_age = df.select(max("age")).collect()[0][0]
print(f"Maximum age: {max_age}")
```
### MIN: Возвращает минимальное значение в указанном столбце.

```py
min_age = df.select(min("age")).collect()[0][0]
print(f"Minimum age: {min_age}")
```
### GROUPBY: Позволяет сгруппировать данные по одному или нескольким столбцам и применить агрегационные функции.

```py
grouped_df = df.groupBy("name").agg(
    count("id").alias("count"),
    sum("age").alias("total_age"),
    avg("age").alias("avg_age")
)
grouped_df.show()
```
### ROLLUP и CUBE: Эти операции позволяют выполнять многомерные агрегации, создавая промежуточные итоги и общие итоги.

```py
df = spark.createDataFrame([
    (1, "A", "X", 10),
    (2, "A", "Y", 20),
    (3, "B", "X", 30),
    (4, "B", "Y", 40)
], ["id", "group1", "group2", "value"])

rolled_up_df = df.rollup("group1", "group2").agg(sum("value"))
rolled_up_df.show()
```
