## Типы данных и внешние таблицы в Greenplum

Greenplum поддерживает разнообразные типы данных и предоставляет возможности работы с внешними таблицами, что делает его мощным инструментом для обработки и анализа больших объемов данных.

**Типы данных в Greenplum**

Greenplum предлагает широкий набор встроенных типов данных, которые можно использовать для создания таблиц. Основные категории типов данных включают:

- **Числовые типы**:
  - `integer`, `smallint`, `bigint`
  - `decimal`, `numeric`, `real`, `double precision`

- **Строковые типы**:
  - `character varying` (или `varchar`)
  - `character` (или `char`)
  - `text`

- **Дата и время**:
  - `date`
  - `time` (с возможностью указания часового пояса)
  - `timestamp` (с возможностью указания часового пояса)

- **Логические типы**:
  - `boolean`

- **Специальные типы**:
  - `json` и `jsonb` для работы с данными в формате JSON
  - Геометрические типы, такие как `point`, `line`, и другие.

Каждый из этих типов имеет свои особенности, включая размер хранения и диапазон значений.

**Внешние таблицы в Greenplum**

В Greenplum существуют два основных типа таблиц, которые обеспечивают доступ к данным за пределами базы данных: **внешние таблицы (external tables)** и **сторонние таблицы (foreign tables)**.

- **Внешние таблицы**:
  - Позволяют загружать и выгружать данные из внешних источников, таких как файлы или другие базы данных.
  - При создании внешней таблицы необходимо указать структуру данных, формат, протокол доступа и расположение источника.
  - Внешние таблицы могут быть как доступными только для чтения, так и для записи. Например, можно выгружать данные из Greenplum в файл или другую систему.

- **Сторонние таблицы**:
  - Это более сложный механизм, который позволяет работать с данными, находящимися вне базы данных, как если бы они были обычными таблицами.
  - Сторонние таблицы используют обертки внешних данных (FDW), которые определяют способ подключения к удаленным источникам.

Кроме того, Greenplum поддерживает **внешние веб-таблицы**, которые позволяют подключаться к динамическим данным на веб-ресурсах. Это особенно полезно для работы с актуальными данными, такими как курсы валют или котировки акций.

Эти возможности делают Greenplum гибким инструментом для интеграции различных источников данных и работы с ними в рамках аналитических задач.

Citations:
 https://bigdataschool.ru/blog/news/greenplum/external-and-foreign-tables-in-greenplum.html
 https://bigdataschool.ru/blog/news/greenplum/external-web-tables-in-greenplum.html
 https://docs.huihoo.com/greenplum/pivotal/4.3.6/ref_guide/data_types.html
 https://docs.vmware.com/en/VMware-Greenplum/7/greenplum-database/ref_guide-data_types.html
 https://docs.tibco.com/pub/sfire-analyst/latest/doc/html/en-US/TIB_sfire-analyst_UsersGuide/connectors/pivgp/pivgp_pivotal_greenplum_data_types.htm