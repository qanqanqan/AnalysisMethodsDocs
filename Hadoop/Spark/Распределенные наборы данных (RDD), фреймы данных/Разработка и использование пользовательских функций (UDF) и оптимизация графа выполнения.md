
**Пользовательские функции (UDF)** в Apache Spark позволяют расширять возможности обработки данных, добавляя функции, которые не поддерживаются встроенными методами. Эти функции могут быть полезны для выполнения сложных операций на данных, которые требуют индивидуального подхода.

---
## 1. Создание и использование UDF

Чтобы создать пользовательскую функцию в Spark, необходимо выполнить следующие шаги:

1. **Определение функции**: Создайте функцию на Python или Scala, которая будет обрабатывать данные.Пример на Python:
```python
    from pyspark.sql.functions import udf 
    from pyspark.sql.types import IntegerType 
    def square(x):     
	    return x * x square_udf = udf(square, IntegerType())`
```
    
2. **Регистрация UDF**: Зарегистрируйте функцию для использования в DataFrame API или SQL-запросах.
```python
spark.udf.register("square_udf", square_udf)
```
    
3. **Применение UDF**: Используйте UDF для обработки данных в DataFrame.
```python
df = spark.createDataFrame([(1,), (2,), (3,)], ["value"]) 
df_with_square = df.withColumn("square_value", square_udf(df["value"])) df_with_square.show()
```
---

## 2. Пользовательские агрегатные функции (UDAF)
Помимо простых UDF, можно создавать пользовательские агрегатные функции (UDAF), которые обрабатывают несколько строк и возвращают одно значение.Пример создания UDAF:

```scala
import org.apache.spark.sql.expressions.Aggregator 
import org.apache.spark.sql.SparkSession 

case class Average(var sum: Double, var count: Long) 

object MyAverage extends Aggregator[Double, Average, Double] {     
	def zero: Average = Average(0.0, 0)    
	def reduce(b: Average, a: Double): Average = {        
		b.sum += a        
		b.count += 1        
		b    
	}    
	def merge(b1: Average, b2: Average): Average = {        
		b1.sum += b2.sum        
		b1.count += b2.count        
		b1    
	}    
	def finish(reduction: Average): Double = reduction.sum / reduction.count 
} 
// Регистрация UDAF 
spark.udf.register("myAverage", functions.udaf(MyAverage))`
```
---

## 3. Оптимизация графа выполнения

Оптимизация графа выполнения в Spark включает:

- **Catalyst Optimizer**: Автоматически оптимизирует планы выполнения запросов, применяя различные правила преобразования и оптимизации.
- **Устранение избыточных операций**: Удаление ненужных операций из плана выполнения может значительно улучшить производительность.
- **Использование фильтров**: Применение фильтров как можно раньше в процессе обработки данных позволяет уменьшить объем данных на последующих этапах.
- **Broadcast Join**: Используйте `broadcast` для небольших DataFrame при выполнении соединений. Это уменьшает затраты на перемешивание данных.
---
