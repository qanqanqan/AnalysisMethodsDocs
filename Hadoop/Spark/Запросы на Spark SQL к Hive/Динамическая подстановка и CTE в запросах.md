# Динамическая подстановка и CTE в запросах

В Spark SQL динамическая подстановка и общие табличные выражения (CTE - Common Table Expressions) могут быть использованы для создания более гибких и читаемых запросов. Вот как их можно применять:
Динамическая подстановка

#### Динамическая подстановка позволяет вставлять значения в SQL-запросы во время выполнения. Это полезно, когда вы хотите параметризовать ваши запросы.
Пример:
```py
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("DynamicSubstitution").getOrCreate()

# Предположим, у нас есть DataFrame
data = [("Alice", 2), ("Bob", 1), ("Cathy", 3)]
df = spark.createDataFrame(data, ["name", "department_id"])

# Динамическая подстановка в SQL запросе
department = 2
query = f"SELECT * FROM employees WHERE department_id = {department}"
result = spark.sql(query)
result.show()
```
Однако, стоит быть осторожным с SQL инъекциями при использовании строковой интерполяции. Лучше использовать параметризованные запросы, если это возможно:
```py
# Более безопасный способ с использованием параметров
query = "SELECT * FROM employees WHERE department_id = ?"
result = spark.sql(query, department)
result.show()
```
## Общие табличные выражения (CTE)

CTE позволяют вам определять временные именованные результирующие наборы, которые доступны в рамках выполнения одного оператора SELECT, INSERT, UPDATE, DELETE или MERGE.
Пример использования CTE:
```py
# Создаем CTE для подсчета количества сотрудников в каждом департаменте
cte_query = """
WITH department_count AS (
    SELECT department_id, COUNT(*) as employee_count
    FROM employees
    GROUP BY department_id
)
SELECT e.name, e.department_id, dc.employee_count
FROM employees e
JOIN department_count dc ON e.department_id = dc.department_id
"""

# Используем CTE в запросе
spark.sql(cte_query).show()
```
Заключение

    Динамическая подстановка: Это мощный инструмент для создания динамических запросов, но требует внимательного подхода к безопасности, чтобы избежать SQL инъекций. Использование параметризованных запросов предпочтительнее, когда это возможно.

    CTE: Они улучшают читаемость и поддерживаемость сложных запросов, позволяя разбить логику на более мелкие, понятные части. CTE особенно полезны в Spark SQL для создания цепочек зависимых запросов, где результат одного запроса используется в другом.

Использование CTE и динамической подстановки делает ваши SQL-запросы в Spark более гибкими и мощными, позволяя вам эффективно работать с данными в больших масштабах.