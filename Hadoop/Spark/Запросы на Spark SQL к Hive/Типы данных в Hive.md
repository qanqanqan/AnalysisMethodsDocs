# Типы данных в Hive
В Hive предусмотрены различные типы данных, которые используются для определения формата и обработки данных в таблицах. Основные типы данных делятся на несколько категорий: простые типы, комплексные типы и пользовательские типы. Вот подробный обзор каждого из них:

### 1. Простые типы данных

- Текстовые типы:
```sql
- STRING: Строка произвольной длины.
- CHAR(n): Строка фиксированной длины. Если длина строки меньше n, она будет дополнена пробелами.
- VARCHAR(n): Строка переменной длины с максимальной длиной n.
```


- Числовые типы:
```sql
- TINYINT: 1-байтовое целое число (от -128 до 127).
- SMALLINT: 2-байтовое целое число (от -32,768 до 32,767).
- INT: 4-байтовое целое число (от -2,147,483,648 до 2,147,483,647).
- BIGINT: 8-байтовое целое число (от -9,223,372,036,854,775,808 до 9,223,372,036,854,775,807).
- FLOAT: 4-байтовое число с плавающей запятой.
- DOUBLE: 8-байтовое число с плавающей запятой.
- DECIMAL(p, s): Число с фиксированной запятой, где p — это общая длина, а s — количество знаков после запятой. Например, DECIMAL(10, 2).
```


- Даты и временные типы:
```sql
- DATE: Дата (год, месяц, день).
- TIMESTAMP: Дата и время (год, месяц, день, часы, минуты, секунды).
- INTERVAL: Интервалы времени, используемые для представления относительных временных значений.
```

### 2. Комплексные типы данных

- Массивы:
- ARRAY<T>: Коллекция элементов одного типа T. Например: ARRAY<STRING>.

- Структуры:
- STRUCT<field1: type1, field2: type2, ...>: Комплексный тип, содержащий набор полей. Например: STRUCT<name: STRING, age: INT>.

- Словари (Map):
- MAP<keyType, valueType>: Словарь, в котором ключи и значения могут иметь разные типы. Например: MAP<STRING, INT>.

### 3. Пользовательские типы данных

- Основываясь на простых и комплексных типах, пользовательские типы данных могут быть определены с помощью STRUCT, ARRAY и MAP, что позволяет создавать более сложные структуры данных в таблицах Hive.

### Пример определения таблицы в Hive

Вот пример создания таблицы с использованием различных типов данных:

```sql
CREATE TABLE employee (
    id INT,
    name STRING,
    salary DECIMAL(10, 2),
    hire_date DATE,
    attributes MAP<STRING, STRING>,
    skills ARRAY<STRING>,
    personal_info STRUCT<age: INT, address: STRING>
) ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE;
```


### Заключение

Знание типов данных в Hive крайне важно для корректной работы с данными и обеспечения правильного анализа и обработки. Выбор подходящих типов данных позволяет оптимизировать хранение, уменьшить объемы данных и повысить производительность запросов.