В Hadoop важную роль играют конфигурационные файлы, которые настраивают работу кластера и определяют параметры различных компонентов. Два из таких файлов — hdfs-site.xml и core-site.xml — содержат параметры конфигурации для HDFS и основных служб Hadoop.

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
# core-site.xml

Файл core-site.xml содержит основные настройки Hadoop, которые влияют на работу всей системы. Он определяет такие параметры, как используемая файловая система (HDFS, S3 и др.) и настройки для взаимодействия с ней.

## Ключевые параметры core-site.xml:
### fs.defaultFS:

- Описание: Определяет используемую файловую систему по умолчанию (обычно это HDFS).
- Пример:
```xml
<property>
  <name>fs.defaultFS</name>
  <value>hdfs://namenode-hostname:8020</value>
</property>
```
- Этот параметр задает адрес NameNode, который будет использоваться для работы с файловой системой HDFS.
### hadoop.tmp.dir:

- Описание: Временная директория для хранения файлов Hadoop. Используется для временного хранения данных, когда те еще не загружены в HDFS.
- Пример:
```xml
<property>
  <name>hadoop.tmp.dir</name>
  <value>/tmp/hadoop-${user.name}</value>
</property>
```

### io.file.buffer.size:

- Описание: Определяет размер буфера для операций ввода-вывода (I/O)
- Пример:
```xml
Копировать код
<property>
  <name>io.file.buffer.size</name>
  <value>131072</value> <!-- 128 KB -->
</property>
```

### hadoop.proxyuser.[username].hosts и hadoop.proxyuser.[username].groups

- Описание: Эти параметры используются для настройки прав проксирования. Они позволяют одному пользователю Hadoop выполнять задачи от имени другого пользователя.
- Пример:
```xml
Копировать код
<property>
  <name>hadoop.proxyuser.hdfs.hosts</name>
  <value>*</value>
</property>
<property>
  <name>hadoop.proxyuser.hdfs.groups</name>
  <value>users</value>
</property>
```
### fs.trash.interval:

- Описание: Определяет время в минутах, в течение которого удаленные файлы будут храниться в корзине (Trash) перед окончательным удалением.
- Пример:
```xml
<property>
  <name>fs.trash.interval</name>
  <value>60</value> <!-- 60 минут -->
</property>
```
### ipc.client.connect.max.retries:

- Описание: Задает максимальное количество попыток подключения клиента к серверу (например, к NameNode).
- Пример:
```xml
<property>
  <name>ipc.client.connect.max.retries</name>
  <value>10</value>
</property>
```

# Итоги:
- hdfs-site.xml — содержит настройки, относящиеся к HDFS, такие как количество реплик, размер блоков, пути к директориям хранения данных на узлах NameNode и DataNode.
- core-site.xml — содержит общие настройки Hadoop, такие как файловая система по умолчанию, временные директории, параметры ввода-вывода и настройки безопасности.

Эти два файла необходимо настроить правильно для оптимальной работы кластера Hadoop, так как они задают ключевые параметры, влияющие на производительность и отказоустойчивость системы.