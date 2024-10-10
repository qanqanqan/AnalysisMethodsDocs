# Настройка высокой доступности в HDFS

Высокая доступность (High Availability, HA) в HDFS позволяет избежать простоев кластера в случае отказа одного из узлов NameNode. В режиме высокой доступности в HDFS используется два узла NameNode — **Active NameNode** и **Standby NameNode**, что обеспечивает автоматическое переключение на резервный узел при сбое активного узла.

## Основные компоненты HA

1. **Active NameNode** — основной узел, который обрабатывает все запросы клиентов и управляет файловой системой HDFS.
2. **Standby NameNode** — резервный узел, который синхронизируется с активным узлом и может в случае сбоя стать активным.
3. **JournalNodes** — узлы, которые используются для журналирования транзакций между Active и Standby узлами NameNode.
4. **Zookeeper** — служба координации для управления состоянием Active/Standby NameNode и автоматического переключения (failover).

## Архитектура

- **Active NameNode** выполняет все операции по управлению файловой системой.
- **Standby NameNode** постоянно синхронизируется с Active NameNode через общие журналы (Shared Edit Logs), которые хранятся в JournalNodes.
- При отказе Active NameNode происходит автоматическое переключение на Standby NameNode, что минимизирует время простоя.

## Настройка высокой доступности в HDFS

### 1. Предварительные условия

- Минимум два узла NameNode.
- Три узла JournalNode для обеспечения отказоустойчивого журнала.
- Установленный и настроенный Zookeeper для координации Active/Standby.

### 2. Настройка файлов конфигурации

#### 2.1 `hdfs-site.xml`

Добавьте следующие настройки для включения HA и указания JournalNodes:

```xml
<configuration>
    <!-- Настройка именованных сервисов NameNode для HA -->
    <property>
        <name>dfs.nameservices</name>
        <value>mycluster</value>
    </property>

    <!-- Конфигурация NameNode для кластера HA -->
    <property>
        <name>dfs.ha.namenodes.mycluster</name>
        <value>nn1,nn2</value>
    </property>

    <!-- Адрес NameNode 1 -->
    <property>
        <name>dfs.namenode.rpc-address.mycluster.nn1</name>
        <value>namenode1-host:8020</value>
    </property>

    <!-- Адрес NameNode 2 -->
    <property>
        <name>dfs.namenode.rpc-address.mycluster.nn2</name>
        <value>namenode2-host:8020</value>
    </property>

    <!-- Адрес HTTP-интерфейса NameNode 1 -->
    <property>
        <name>dfs.namenode.http-address.mycluster.nn1</name>
        <value>namenode1-host:50070</value>
    </property>

    <!-- Адрес HTTP-интерфейса NameNode 2 -->
    <property>
        <name>dfs.namenode.http-address.mycluster.nn2</name>
        <value>namenode2-host:50070</value>
    </property>

    <!-- Настройка JournalNodes -->
    <property>
        <name>dfs.namenode.shared.edits.dir</name>
        <value>qjournal://journalnode1:8485;journalnode2:8485;journalnode3:8485/mycluster</value>
    </property>

    <!-- Настройка автоматического переключения между Active и Standby -->
    <property>
        <name>dfs.ha.automatic-failover.enabled</name>
        <value>true</value>
    </property>

    <!-- Конфигурация Zookeeper для координации -->
    <property>
        <name>dfs.ha.zkfc.nn1</name>
        <value>namenode1-host</value>
    </property>
    <property>
        <name>dfs.ha.zkfc.nn2</name>
        <value>namenode2-host</value>
    </property>

    <!-- Адреса Zookeeper -->
    <property>
        <name>ha.zookeeper.quorum</name>
        <value>zookeeper1-host:2181,zookeeper2-host:2181,zookeeper3-host:2181</value>
    </property>
</configuration>
```
#### 2.2 core-site.xml

Настройте основной файл конфигурации core-site.xml для работы с высокой доступностью:

```xml
<configuration>
    <!-- Настройка NameNode с учетом HA -->
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://mycluster</value>
    </property>

    <!-- Конфигурация клиента для автоматического переключения -->
    <property>
        <name>dfs.client.failover.proxy.provider.mycluster</name>
        <value>org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider</value>
    </property>
</configuration>
```
3. Запуск JournalNodes
На всех узлах JournalNode выполните следующие шаги:
- Скопируйте файл конфигурации hdfs-site.xml на каждый узел JournalNode.
- Запустите службу JournalNode на каждом узле:
```bash
hadoop-daemon.sh start journalnode
```
4. Форматирование и синхронизация
- Форматирование JournalNode:

    - На одном из узлов NameNode выполните команду форматирования:
```bash
hdfs namenode -format
```
- Синхронизация Standby NameNode:

    - После форматирования активного узла, запустите синхронизацию Standby NameNode:
```bash
hdfs namenode -bootstrapStandby
```

# Заключение

Высокая доступность в HDFS существенно повышает отказоустойчивость кластера и минимизирует время простоя в случае отказа одного из узлов NameNode. Правильно настроенные Active/Standby узлы с использованием Zookeeper и JournalNode обеспечивают стабильную работу системы и быстрый failover при сбоях.