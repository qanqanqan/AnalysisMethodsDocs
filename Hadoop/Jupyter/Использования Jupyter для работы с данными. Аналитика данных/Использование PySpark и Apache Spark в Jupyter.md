## Использование PySpark и Apache Spark в Jupyter

Использование PySpark и Apache Spark в Jupyter Notebook предоставляет мощные возможности для работы с большими данными. PySpark — это интерфейс Python для Apache Spark, который позволяет легко обрабатывать данные и выполнять параллельные вычисления. Вот шаги по настройке и использованию PySpark в Jupyter Notebook:

### Шаг 1: Установка необходимых компонентов

1. Установите Java:
Apache Spark требует наличие Java. Установите OpenJDK:

```sh
   sudo apt update
   sudo apt install openjdk-11-jdk
```

2. Установите Apache Spark:
Скачайте Apache Spark с [официального сайта](https://spark.apache.org/downloads.html). Выберите версию с моментальным поддерживаемым Hadoop (например, pre-built for Apache Hadoop 3.2.x).

```sh
   wget https://downloads.apache.org/spark/spark-<version>/spark-<version>-bin-hadoop3.2.tgz
   tar -xvf spark-<version>-bin-hadoop3.2.tgz
   mv spark-<version>-bin-hadoop3.2 /opt/spark
```

3. Установите PySpark:
Используйте pip для установки PySpark, который включает необходимые библиотеки для работы с Spark в Python:

```sh
   pip install pyspark
```

4. Настройте переменные окружения:
Добавьте следующие строки в ваш `~/.bashrc` или `~/.profile` файл, чтобы добавить Spark в ваш `PATH`:

```
   export SPARK_HOME=/opt/spark
   export PATH=$PATH:$SPARK_HOME/bin
```

После этого выполните:

```sh
   source ~/.bashrc
```

### Шаг 2: Установка Jupyter Notebook

Если Jupyter не установлен, установите его с помощью pip:

```sh
pip install notebook
```

### Шаг 3: Запуск Jupyter Notebook

Запустите Jupyter Notebook:

```sh
jupyter notebook
```

### Шаг 4: Использование PySpark в Jupyter Notebook

1. Импортируйте необходимые библиотеки:

Создайте новый ноутбук и импортируйте PySpark:

```py
from pyspark.sql import SparkSession

# Создание SparkSession
spark = SparkSession.builder \
    .appName("MyApp") \
    .getOrCreate()
```

2. Загрузка данных:

Вы можете загружать данные из различных источников, таких как CSV, Parquet, или HDFS:

```py
# Загрузка данных из CSV файла
df = spark.read.csv("path/to/your/data.csv", header=True, inferSchema=True)

# Показ первых 5 строк
df.show(5)
```

3. Обработка данных:

Вы можете использовать DataFrame API для выполнения различных операций:

```py
# Выбор определённых колонок
selected_data = df.select("column1", "column2")

# Фильтрация данных
filtered_data = df.filter(df.column1 > 100)

# Группировка и агрегация
aggregated_data = df.groupBy("column2").count()

# Показ результатов
aggregated_data.show()
```

4. Использование SQL запросов:

Вы можете зарегистрировать DataFrame как временную таблицу и выполнять SQL-запросы:

```py
# Регистрация DataFrame как временной таблицы
df.createOrReplaceTempView("my_table")

# Выполнение SQL-запроса
result = spark.sql("SELECT column1, COUNT(*) as count FROM my_table GROUP BY column1")
result.show()
```

5. Сохранение результатов:

Вы можете сохранить результаты в различных форматах:

```py
# Сохранение в CSV
result.write.csv("path/to/output.csv", header=True)
```

### Шаг 5: Завершение работы

Не забудьте завершить работу сессии Spark после завершения анализа:

```py
spark.stop()
```