
---

## Операции с RDD

Операции над RDD делятся на два типа:

1. **Трансформации (Transformations)**: Эти операции создают новый RDD из существующего. Примеры трансформаций включают `map()`, `filter()`, `flatMap()` и другие. Трансформации являются "ленивыми", то есть они не выполняются до тех пор, пока не будет вызвано действие.
2. **Действия (Actions)**: Эти операции выполняют вычисления и возвращают результат обратно в программу-драйвер. Примеры действий включают `collect()`, `count()`, `take(n)` и другие.

### Примеры трансформаций

- **map()**: Применяет функцию к каждому элементу RDD и возвращает новый RDD.
```python
squared_RDD = my_inner_RDD.map(lambda x: x * x)
```
- **filter()**: Возвращает новый RDD, содержащий только элементы, соответствующие заданному условию.
```python
filtered_RDD = my_inner_RDD.filter(lambda x: x > 5)  # [6, 7, 8, 9, 10]
```
- **distinct()**: Удаляет дубликаты из RDD.
```python
duplicates_RDD = sc.parallelize([1, 1, 3, 4, 4, 6]) 
distinct_RDD = duplicates_RDD.distinct() # [1, 3, 4, 6]
```

### Примеры действий

- **collect()**: Возвращает все элементы RDD как список.
```python
result = my_inner_RDD.collect()  # [1, 2, 3, ..., 10]
```
- **count()**: Возвращает количество элементов в RDD.
```python
count_result = my_inner_RDD.count()  # 10
```
- **take(n)**: Возвращает первые n элементов RDD.
    
```python
first_three = my_inner_RDD.take(3)  # [1, 2, 3]
```
---

## Создание RDD

Существует два основных способа создания RDD:
- **Загрузка из внешних источников**: Например, можно создать RDD из текстового файла с помощью метода `textFile()`:
```python
from pyspark import SparkContext 
sc = SparkContext("local", "RDD Example") 
rdd_from_file = sc.textFile("hdfs://path/to/file.txt")
```
- **Создание из коллекции**: Используя метод `parallelize()`, можно создать RDD из существующей коллекции данных:
```python
 data = [1, 2, 3, 4, 5] 
 rdd_from_collection = sc.parallelize(data)
 ```
---
 

