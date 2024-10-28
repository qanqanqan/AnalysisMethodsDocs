Компоненты и архитектура Spark включают в себя следующие ключевые элементы:

 1. Driver — основная программа, которая управляет выполнением задач. Driver определяет DAG (Directed Acyclic Graph) выполнения и распределяет задачи по рабочим узлам.
 2. Cluster Manager — отвечает за управление ресурсами кластера. Spark поддерживает несколько менеджеров кластеров, таких как YARN (часть Hadoop), Mesos или собственный Standalone Cluster Manager.
 3. Executors — это процессы на рабочих узлах, которые выполняют задачи (tasks). Каждый executor отвечает за выполнение задач и возвращает результаты обратно в драйвер. Также он хранит данные в памяти или на диске для последующего использования.
 4. Tasks — это минимальные единицы работы, которые выполняются в рамках одного executor. Они являются результатом разбиения операций на RDD.

Пример работы со Spark и YARN (интеграция с Hadoop):
```pyspark
from pyspark import SparkConf, SparkContext

# Конфигурация для работы с YARN
conf = SparkConf().setAppName("Hadoop Integration").setMaster("yarn")
sc = SparkContext(conf=conf)

# Пример простой задачи
data = sc.parallelize([1, 2, 3, 4, 5])
result = data.map(lambda x: x * 2).collect()

print(result)
sc.stop()
```
Этот код показывает простую задачу, где Spark использует YARN в качестве менеджера ресурсов для распределения задач по кластеру.