# hdfs-site.xml
Файл hdfs-site.xml используется для настройки параметров, которые непосредственно касаются HDFS (Hadoop Distributed File System). Этот файл задает конфигурации NameNode, DataNode и других параметров, связанных с хранением данных в HDFS.

## Ключевые параметры hdfs-site.xml:
### dfs.replication

- Описание: Задает количество реплик для каждого блока данных в HDFS.

- Пример:
```xml
<property>
  <name>dfs.replication</name>
  <value>3</value>
</property>
```
- По умолчанию: 3 реплики, то есть каждый блок данных хранится на трех узлах DataNode для обеспечения отказоустойчивости.
### dfs.blocksize:

- Описание: Определяет размер блоков данных в HDFS.
- Пример:
```xml
<property>
  <name>dfs.blocksize</name>
  <value>134217728</value> <!-- 128 MB --, 134217728 - количество байт>
</property>
```
- По умолчанию: Обычно 128 МБ или 256 МБ. Большие блоки используются для оптимизации работы с большими объемами данных.

### dfs.namenode.name.dir:

- Описание: Указывает локальные директории на NameNode, где хранятся метаданные файловой системы.
- Пример:
```xml
<property>
  <name>dfs.namenode.name.dir</name>
  <value>file:///data/hadoop/dfs/namenode</value>
</property>
```
### dfs.datanode.data.dir:

- Описание: Указывает локальные директории на DataNode, где будут храниться блоки данных.
- Пример:
```xml
<property>
  <name>dfs.datanode.data.dir</name>
  <value>file:///data/hadoop/dfs/datanode</value>
</property>
```
### dfs.permissions:

- Описание: Включает или отключает проверку прав доступа в HDFS.
- Пример:
```xml 
<property>
  <name>dfs.permissions</name>
  <value>true</value>
</property>
```
### dfs.namenode.handler.count:

- Описание: Определяет количество потоков (threads), обрабатывающих запросы на NameNode.
- Пример:
```xml
<property>
  <name>dfs.namenode.handler.count</name>
  <value>100</value>
</property>
```