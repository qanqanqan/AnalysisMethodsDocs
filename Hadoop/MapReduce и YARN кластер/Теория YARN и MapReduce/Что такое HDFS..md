## Что такое HDFS?

HDFS (Hadoop Distributed File System) — это распределённая файловая система, разработанная для хранения и обработки больших объёмов данных в рамках экосистемы Apache Hadoop. HDFS является одной из основных компонентов Hadoop и предназначена для обеспечения надежности, масштабируемости и высокой доступности данных. Вот основные характеристики и особенности HDFS:

### Основные характеристики HDFS:

1. Распределенное хранение данных:
- HDFS разбивает файлы на блоки фиксированного размера (обычно 128 МБ или 64 МБ) и распределяет их по различным узлам кластера. Это позволяет обрабатывать большие объёмы данных, которые не помещаются на одном узле.

2. Высокая доступность и отказоустойчивость:
- Для каждого блока данных HDFS сохраняет несколько копий (реплик) на разных узлах, что обеспечивает защиту от сбоев. По умолчанию количество реплик составляет три. Если один узел выходит из строя, данные всё равно доступны с других узлов, хранящих их копии.

3. Доступ к данным:
- HDFS предоставляет доступ к данным через простые API, что облегчает взаимодействие приложений с файловой системой. Хранение данных в HDFS также позволяет обеспечить параллельный доступ к данным для различных приложений.

4. Масштабируемость:
- HDFS спроектирован так, чтобы легко добавлять новые узлы и масштабировать кластер по мере роста объёмов данных. Это позволяет управлять и хранить большие объемы информации с помощью относительно недорогих серверов.

5. Производительность:
- HDFS оптимизирован для потокового чтения больших файлов и поддерживает параллельную обработку данных, что повышает производительность при выполнении задач, основанных на MapReduce.

6. особенности обработки больших файлов:
- HDFS построен для работы с крупными файлами, а не с множеством мелких. Это достигается путём оптимизации операций чтения и записи, что делает систему эффективной для хранения больших объёмов данных.

### Структура HDFS:

1. NameNode:
- Это главный управляющий элемент HDFS, который отвечает за хранение метаданных файловой системы, таких как структура каталогов и местоположение блоков данных. NameNode не хранит сами данные, а только информацию о них.

2. DataNode:
- Это узлы, которые фактически хранят блоки данных. Каждый DataNode управляет хранением блоков на локальных дисках и отвечает за выполнение операций чтения и записи по запросам от клиентов и NameNode.

### Преимущества HDFS:

HDFS предлагает ряд преимуществ:
- Отказоустойчивость: Благодаря раздельному хранению копий данных, система остаётся устойчивой к сбоям.
- Экономичность: HDFS может использовать недорогие серверы и хранить данные на обычных устройствах хранения.
- Простота в использовании: API HDFS позволяют разработчикам легко взаимодействовать с данными.
- Высокая производительность: Оптимизация для обработки больших данных и потокового чтения повышает производительность системы.