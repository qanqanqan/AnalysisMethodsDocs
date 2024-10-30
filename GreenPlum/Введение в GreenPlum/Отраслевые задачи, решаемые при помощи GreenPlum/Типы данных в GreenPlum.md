# Типы данных в GreenPlum

Greenplum, основанный на PostgreSQL, поддерживает разнообразные типы данных, которые можно использовать для хранения и обработки информации. Основные категории типов данных в Greenplum включают:

## Числовые типы
- **boolean**: Логический тип, принимает значения `TRUE`, `FALSE` и `NULL`.
- **smallint**: 2-байтовое целое число, диапазон от -32,768 до 32,767.
- **integer** (или `int`): 4-байтовое целое число, диапазон от -2,147,483,648 до 2,147,483,647.
- **bigint**: 8-байтовое целое число, диапазон от -9,223,372,036,854,775,808 до 9,223,372,036,854,775,807.
- **real**: 4-байтовое число с плавающей запятой.
- **double precision**: 8-байтовое число с плавающей запятой.
- **numeric**: Число с фиксированной точностью.

## Символьные типы
- **character(n)**: Строка фиксированной длины.
- **varchar(n)**: Строка переменной длины с максимальной длиной `n`.
- **text**: Строка произвольной длины.

## Дата и время
- **date**: Хранит дату без времени.
- **timestamp**: Хранит дату и время.
- **interval**: Хранит временной интервал.

## Бинарные типы
- **bytea**: Для хранения двоичных данных.

## JSON и массивы
- **json**: Для хранения данных в формате JSON.
- **array**: Позволяет хранить массивы значений любого типа.

## Примечания
В Greenplum все базовые типы данных по умолчанию могут содержать значения `NULL`, что означает отсутствие данных в колонке. При проектировании схем данных рекомендуется использовать одинаковые типы данных для столбцов в операциях JOIN для повышения производительности запросов[1][2][3].

Citations:
[1] https://ydb.tech/docs/ru/concepts/federated_query/greenplum
[2] https://bigdataschool.ru/blog/greenplum-schema-design-best-practices.html
[3] https://datafinder.ru/products/postgresql-tipy-dannyh
[4] https://metanit.com/sql/postgresql/2.3.php
[5] https://postgrespro.ru/docs/postgresql/9.4/datatype
[6] https://postgrespro.ru/docs/postgrespro/10/datatype
[7] https://bigdataschool.ru/blog/greenplum-tables-and-data-types.html
[8] https://pgcookbook.ru/article/char_vs_varchar_vs_text.html