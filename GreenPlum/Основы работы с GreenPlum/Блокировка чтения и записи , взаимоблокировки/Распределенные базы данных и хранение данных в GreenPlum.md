# Распределённые базы данных и хранение данных в Greenplum

## Распределённые базы данных

**Распределённая база данных** — это система баз данных, в которой данные хранятся на нескольких узлах (серверах), которые могут находиться как в одной физической локации, так и распределены по разным географическим регионам. Основной целью распределённых баз данных является улучшение производительности и масштабируемости за счёт параллельной обработки запросов и распределённого хранения данных.

### Ключевые характеристики распределённых баз данных:

1. **Данные распределены по нескольким узлам**: Вместо хранения всех данных в одном месте, они делятся на части и распределяются между узлами системы.
   
2. **Параллельная обработка**: Каждый узел может обрабатывать свою часть данных одновременно с другими узлами, что сокращает время обработки запросов.
   
3. **Надёжность и отказоустойчивость**: Если один из узлов выходит из строя, другие узлы могут продолжить обработку данных, обеспечивая высокую доступность системы.

4. **Горизонтальная масштабируемость**: При увеличении объёма данных можно добавлять новые узлы, что позволяет системе легко расти без значительных потерь в производительности.

## Хранение данных в Greenplum

Greenplum — это распределённая система баз данных, которая использует архитектуру MPP (Massively Parallel Processing) для работы с данными. В Greenplum данные хранятся в виде распределённых сегментов на разных узлах кластера, что обеспечивает высокую производительность при обработке запросов.

### Основные концепции хранения данных в Greenplum:

1. **Сегменты (Segments)**:
   - В Greenplum данные распределяются по множеству сегментов (узлов) кластера. Каждый сегмент содержит подмножество всей базы данных, и запросы SQL, выполняемые на данных, разделяются на подзапросы, которые обрабатываются параллельно каждым сегментом.
   - Сегменты содержат копии таблиц и выполняют большую часть вычислений по запросам.

2. **Мастер-узел (Master Node)**:
   - Мастер-узел управляет координацией запросов и распределением их между сегментами. Он принимает SQL-запросы от пользователей, разбивает их на подзапросы и отправляет на соответствующие сегменты для обработки.
   - Мастер-узел не хранит данные пользователя; он лишь содержит метаданные (информацию о структуре базы данных, схемах и таблицах).

3. **Методы распределения данных**:
   Greenplum поддерживает два основных метода распределения данных по сегментам:
   
   - **Хэш-распределение (Hash Distribution)**:
     - Данные распределяются по сегментам на основе хэш-функции, которая применяется к значению одной или нескольких колонок. Это помогает равномерно распределить строки таблицы между сегментами и обеспечить эффективную параллельную обработку запросов.
   
   - **Распределение по репликам (Replicated Distribution)**:
     - Некоторые таблицы могут быть полностью дублированы на каждом сегменте для ускорения выполнения запросов, которые требуют доступа к большому объёму данных (например, небольшие таблицы-справочники).
   
4. **Таблицы распределяются по сегментам**:
   - Таблицы могут быть распределены между сегментами по различным схемам в зависимости от потребностей приложения. При хэш-распределении строки таблиц сохраняются на разных сегментах, чтобы обеспечить равномерную нагрузку.

5. **Фрагментация данных (Data Partitioning)**:
   - Greenplum поддерживает механизм разделения таблиц на фрагменты (партиции), что позволяет улучшить производительность запросов за счёт доступа только к необходимым частям данных. Это особенно полезно для временных данных, таких как лог-файлы, где можно выполнять запросы только на определённые временные интервалы.

### Процесс выполнения запроса:

1. **Планирование запроса**:
   - Мастер-узел получает SQL-запрос и определяет оптимальный план его выполнения. План включает распределение подзапросов по сегментам и метод их выполнения.

2. **Распределение подзапросов**:
   - Мастер-узел отправляет каждый подзапрос на соответствующие сегменты, где данные хранятся, для параллельной обработки.

3. **Параллельное выполнение**:
   - Сегменты выполняют свои подзапросы параллельно и возвращают результаты мастер-узлу.

4. **Агрегация результатов**:
   - Мастер-узел собирает результаты от всех сегментов и отправляет итоговый результат пользователю.

## Преимущества распределённого хранения в Greenplum:

- **Высокая производительность** за счёт параллельной обработки запросов.
- **Горизонтальная масштабируемость**: можно легко добавлять новые узлы для увеличения ёмкости и производительности системы.
- **Эффективное управление большими объёмами данных**, благодаря распределению данных по сегментам.
- **Отказоустойчивость**: сбой одного сегмента не приводит к полной недоступности системы, так как другие сегменты продолжают работать.

## Заключение:

Greenplum эффективно использует архитектуру распределённых баз данных для параллельной обработки больших объёмов данных. Благодаря механизму сегментации и параллельного выполнения запросов, Greenplum обеспечивает высокую производительность, масштабируемость и надёжность, что делает её популярным выбором для аналитических и бизнес-приложений, работающих с большими данными.
