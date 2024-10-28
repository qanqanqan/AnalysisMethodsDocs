# Соединение таблиц (JOIN) в Spark SQL

## Шаг 1: Инициализация SparkSession

Первым делом нужно создать SparkSession, которая является входной точкой для работы с Spark SQL.
```py
from pyspark.sql import SparkSession


spark = SparkSession.builder \
    .appName("JoinExample") \
    .getOrCreate()
```

## Шаг 2: Создание DataFrame

Допустим, у нас есть две таблицы: одна с информацией о пользователях, другая с информацией о заказах.

### Создаем DataFrame для пользователей
``py
users_data = [(1, 'Alice'), (2, 'Bob'), (3, 'Cathy')]
users_df = spark.createDataFrame(users_data, ["id", "name"])
```
### Создаем DataFrame для заказов
```py
orders_data = [(1, 1, 'Product A'), (2, 1, 'Product B'), (3, 2, 'Product C')]
orders_df = spark.createDataFrame(orders_data, ["order_id", "user_id", "product"])
```
## Шаг 3: Выполнение JOIN

Теперь мы можем выполнить операцию JOIN. Допустим, мы хотим сделать INNER JOIN по user_id.

### Выполняем INNER JOIN
```py
joined_df = users_df.join(orders_df, users_df.id == orders_df.user_id, how='inner')
```

## Шаг 4: Различные типы JOIN

Spark SQL поддерживает несколько типов JOIN:

    inner: возвращает строки, когда есть совпадение в обеих таблицах.
    left_outer: все строки из левой таблицы, и совпадающие строки из правой.
    right_outer: все строки из правой таблицы, и совпадающие строки из левой.
    full_outer: все строки, когда есть совпадение в одной из таблиц.
    left_semi: строки из левой таблицы, для которых есть совпадение в правой.
    left_anti: строки из левой таблицы, для которых нет совпадения в правой.

### Пример использования left_outer:
```py
left_outer_joined_df = users_df.join(orders_df, users_df.id == orders_df.user_id, how='left_outer')
left_outer_joined_df.show()
```
## Шаг 5: Использование SQL

Вы также можете использовать SQL для выполнения JOIN:

### Регистрируем DataFrame как временные представления
```py
users_df.createOrReplaceTempView("users")
orders_df.createOrReplaceTempView("orders")
```

### SQL запрос
```py
sql_result = spark.sql("""
SELECT u.id, u.name, o.product 
FROM users u 
JOIN orders o ON u.id = o.user_id
""")
