Что такое HDFS  и какие основные компоненты и его структура?
**HDFS (Hadoop Distributed File System)** — это распределенная файловая система, являющаяся основной частью экосистемы Hadoop. HDFS позволяет хранить большие объемы данных и обрабатывать их параллельно на нескольких узлах (серверах) в кластере. Система создана для работы с очень большими файлами, обеспечивая высокую доступность и отказоустойчивость за счет репликации данных.

### Основные компоненты HDFS:

1. **NameNode (Главный узел):**
   - Хранит метаданные о файловой системе, такие как структура каталогов и файлов, а также информацию о том, где блоки данных хранятся на DataNode.
   - NameNode не хранит сами данные, а только информацию о том, где они находятся.
   - Этот узел является критическим, и его потеря может привести к недоступности файловой системы.

2. **DataNode (Узел данных):**
   - На этих узлах хранятся реальные данные. Каждый файл в HDFS делится на блоки, которые распределяются по разным DataNode.
   - DataNode также отвечает за чтение и запись данных по запросам от клиентов или NameNode.
   - Эти узлы могут быть реплицированы для обеспечения отказоустойчивости.

3. **Secondary NameNode (Вторичный NameNode):**
   - Часто возникает путаница, думая, что Secondary NameNode является резервом для NameNode, но это не так. Он используется для создания снимков метаданных (контрольных точек) NameNode, чтобы уменьшить нагрузку на NameNode и помочь восстановить его в случае сбоя.
   - Secondary NameNode помогает агрегировать журнал изменений (edit log) и пересчитывать метаданные, но не заменяет NameNode.

4. **Block (Блок данных):**
   - Данные в HDFS разбиваются на блоки, по умолчанию размер одного блока составляет 128 МБ (может быть изменен).
   - Блоки распределяются по нескольким DataNode, и каждый блок реплицируется для обеспечения устойчивости к отказам. Обычно используются три копии каждого блока, что означает тройную репликацию.

5. **Replication (Репликация):**
   - Для повышения надежности данные в HDFS реплицируются на несколько DataNode. Например, если один узел выйдет из строя, данные могут быть восстановлены с другого узла, который содержит копию этого блока.
   - По умолчанию репликация установлена на уровень 3, то есть каждый блок имеет три копии.

### Структура HDFS:

- **Файловая система:** HDFS предоставляет иерархическую файловую систему, аналогичную традиционным файловым системам UNIX или Windows.
- **Директории и файлы:** Пользователи могут создавать директории и файлы. Файл делится на блоки, которые распределяются по DataNode.
- **Метаданные:** NameNode управляет метаданными файловой системы, такими как структура каталогов, информация о файлах и размещении блоков на DataNode.
- **Распределенные блоки:** Блоки файлов хранятся на различных узлах (DataNode), что позволяет обрабатывать данные параллельно и более эффективно.

### Основные характеристики HDFS:
- **Отказоустойчивость:** HDFS спроектирована для работы на недорогих, ненадежных серверах, поэтому предусмотрены механизмы репликации данных для их защиты.
- **Масштабируемость:** Поддержка горизонтального масштабирования, что позволяет легко увеличивать кластер.
- **Параллелизм:** Позволяет обрабатывать большие объемы данных параллельно на многих узлах.
- **Высокая пропускная способность:** Оптимизирована для высокой пропускной способности данных, а не для минимизации задержек при чтении и записи.

Таким образом, HDFS является ключевым элементом Hadoop, обеспечивающим хранение и управление большими объемами данных в распределенной среде.