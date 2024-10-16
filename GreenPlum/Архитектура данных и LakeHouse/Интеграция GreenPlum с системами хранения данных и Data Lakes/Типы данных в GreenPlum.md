## Типы данных в GreenPlum

В Greenplum, как и в PostgreSQL, поддерживаются различные типы данных, которые могут быть использованы для определения структуры таблиц и управления данными. Вот основные категории и примеры типов данных, доступных в Greenplum:

### 1. Числовые типы:
- integer: Целое число (4 байта).
- bigint: Большое целое число (8 байт).
- smallint: Малое целое число (2 байта).
- decimal(p, s): Децимальный тип с фиксированной точкой, где p — общее количество разрядов, а s — количество разрядов после запятой.
- numeric(p, s): Аналогичен decimal, но с возможностью использования большей точности.
- real: Тип с плавающей точкой одинарной точности (4 байта).
- double precision: Тип с плавающей точкой двойной точности (8 байт).

### 2. Строковые типы:
- varchar(n): Строка переменной длины с максимальной длиной n.
- char(n): Строка фиксированной длины n (заполняется пробелами).
- text: Строка произвольной длины (без ограничения).

### 3. Даты и время:
- date: Дата (год, месяц, день).
- time: Время (часы, минуты, секунды).
- timestamp: Дата и время (с точностью до наносекунд).
- timestamptz: Дата и время с временной зоной.
- interval: Интервал времени (различные единицы времени).

### 4. Логические типы:
- boolean: Логический тип (принимает значения TRUE, FALSE или NULL).

### 5. Типы данных для геометрии:
- geometry: Тип для хранения геометрических данных (точки, линии, полигоны и т. д.).
- geography: Тип для хранения географических данных с учетом кривизны Земли.

### 6. Массивы:
- Поддержка массивов, позволяющая создавать таблицы с колонками, содержащими массивы любого из вышеперечисленных типов данных.

### 7. JSON и HSTORE:
- json: Тип для хранения JSON-данных.
- jsonb: Бинарный формат для хранения JSON-данных, поддерживающий индексацию и более быстрые операции.
- hstore: Тип для хранения пар ключ-значение в формате, аналогичном JSON.

### 8. Идентификаторы и UUID:
- serial: Автоинкрементное целое число (обычно используется для первичных ключей).
- bigserial: Автоинкрементное большое целое число.
- uuid: Универсальный уникальный идентификатор.

### 9. Типы данных для системного времени:
- pg_lsn: Тип для представления логической записи в системной базе данных.
- pg_statistic: Системный тип для хранения статистики по столбцам и данным.
