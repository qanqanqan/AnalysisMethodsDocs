В ClickHouse существует функция `arrayJoin`, которая позволяет «развернуть» массивы в табличную форму, что очень удобно для работы с данными, содержащими коллекционные поля. Функция `arrayJoin` генерирует новую таблицу, в которой каждый элемент массива становится отдельной строкой, а другие столбцы копируются неизменными.

## Синтаксис и примеры использования

### Синтаксис

Синтаксис функции `arrayJoin` следующий:

```sql
SELECT <выражения>
FROM <левая подзапрос>
[LEFT] ARRAY JOIN <массив>;
```

### Примеры использования

#### Базовый пример

Например, если у вас есть таблица `data` с одним столбцем `arr`, содержащим массивы:

```sql
CREATE TABLE data (
    arr Array(Int32),
    str String
);
```

И вам нужно развернуть этот массив в отдельные строки, можно использовать следующий запрос:

```sql
SELECT 
    arrayJoin(arr) AS num,
    str
FROM 
    data;
```

Этот запрос приведёт к выводу следующей таблицы:

```
┌─num─┬─str─┐
│ 1   │ text │
│ 2   │ text │
│ ... │ ... │
└─────┴─────┘
```

#### Пример с LEFT ARRAY JOIN

Если хочется включить пустые массивы в результат (`LEFT ARRAY JOIN`):

```sql
SELECT 
    arrayJoin(arr) AS num,
    str
FROM 
    data
LEFT ARRAY JOIN arr;
```

Теперь даже если в столбце `arr` нет значений, он будет включён в результат с заполнителями по умолчанию (обычно ноль или null).

#### Пример с WHERE

Можно добавить фильтр с помощью WHERE:

```sql
SELECT 
    arrayJoin(arr) AS num,
    str
FROM 
    data
WHERE str LIKE '%text%'
LEFT ARRAY JOIN arr;
```

Этот запрос выберет только строки, где значение в столбце `str` начинается с `'text%'`, а затем развернёт массивы в новые строки.

## Другие способы работы с массивами

Кроме функции `arrayJoin`, существуют ещё несколько функций и операторов для работы с массивами в ClickHouse:

- **arrayEnumerate**: Генерирует индексные значения для каждого элемента массива.
- **arrayMap**: Применяет лямбда-функцию ко всему массиву.
- **arrayReduce**: Агрегирует значения в массиве согласно заданной функции.

### Пример использования arrayMap

Например, чтобы умножить каждый элемент массива на константу:

```sql
SELECT 
    arrayMap(x -> x * 2, arr) AS doubled_arr,
    str
FROM 
    data;
```

Этот запрос преобразует каждый элемент массива в удвоенное число.

### Пример использования arrayReduce

Например, чтобы найти сумму всех элементов массива:

```sql
SELECT 
    arrayReduce(sum, arr) AS sum_of_elements,
    str
FROM 
    data;
```

Этот запрос calculates the sum of all elements in the array.

## Заключение

Функция `arrayJoin` в ClickHouse является мощнейшим инструментом для работы с коллекционными полями, позволяя превратить эти структуры данных в традиционный формат таблиц. Кроме того, имеющиеся функции и операторы (`arrayEnumerate`, `arrayMap`, `arrayReduce`) дают широкие возможности для обработки и трансформации массивов, что делает ClickHouse идеальной платформой для сложных аналитических задач.

Citations:
[1] https://altinity.com/blog/harnessing-the-power-of-clickhouse-arrays-part-1
[2] https://clickhouse.com/docs/en/sql-reference/functions/array-join
[3] http://devdoc.net/database/ClickhouseDocs_19.4.1.3-docs/query_language/functions/array_join/
[4] https://stackoverflow.com/questions/66751468/how-to-use-array-join-in-clickhouse
[5] https://kb.altinity.com/altinity-kb-functions/array-like-memory-usage/
[6] https://clickhouse.com/docs/en/sql-reference/statements/select/array-join
[7] https://clickhouse.com/docs/ru/sql-reference/functions/array-join
[8] https://clickhouse.com/docs/ru/sql-reference/statements/select/array-join