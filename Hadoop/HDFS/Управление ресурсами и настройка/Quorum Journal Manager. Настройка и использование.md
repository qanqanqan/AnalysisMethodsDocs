# Quorum Journal Manager (QJM) в HDFS: Настройка и использование

**Quorum Journal Manager (QJM)** — это механизм в HDFS, предназначенный для обеспечения высокой доступности (HA) через ведение журналов изменений для NameNode. Он позволяет нескольким экземплярам JournalNode сохранять журналы, что обеспечивает отказоустойчивость и консистентность данных.

## Основные концепции

- **JournalNode**: Узел, который хранит журналы изменений, поступающие от Active NameNode.
- **Quorum**: Минимальное число узлов, необходимых для достижения согласия (обычно это большинство узлов) для подтверждения операции.
- **Shared Edit Log**: Общий журнал изменений, который используется для синхронизации состояния NameNode.

## Преимущества использования QJM

1. **Отказоустойчивость**: Если один из узлов выходит из строя, остальные продолжают работу.
2. **Консистентность данных**: Использование кворума обеспечивает согласие между узлами.
3. **Масштабируемость**: Легко добавлять новые JournalNode в кластер.

## Настройка Quorum Journal Manager

### 1. Предварительные условия

- Установленный Hadoop (версия 2.x и выше).
- Минимум три узла JournalNode для обеспечения отказоустойчивости.
- Подготовленные узлы NameNode для работы в режиме высокой доступности.

### 2. Настройка конфигурационных файлов

#### 2.1 `hdfs-site.xml`

В файле конфигурации `hdfs-site.xml` добавьте следующие параметры для настройки QJM:

```xml
<configuration>
    <!-- Настройка HA для HDFS -->
    <property>
        <name>dfs.nameservices</name>
        <value>mycluster</value>
    </property>

    <property>
        <name>dfs.ha.namenodes.mycluster</name>
        <value>nn1,nn2</value>
    </property>

    <!-- Конфигурация QJM -->
    <property>
        <name>dfs.namenode.shared.edits.dir</name>
        <value>qjournal://journalnode1:8485;journalnode2:8485;journalnode3:8485/mycluster</value>
    </property>

    <!-- Установка максимального количества записей в журнале -->
    <property>
        <name>dfs.journalnode.edits.dir</name>
        <value>/path/to/journal/directory</value>
    </property>

    <!-- Настройка минимального кворума -->
    <property>
        <name>dfs.journalnode.quorum.size</name>
        <value>2</value> <!-- Установите на 2 для достижения кворума из 3 узлов -->
    </property>

    <!-- Автоматическое переключение -->
    <property>
        <name>dfs.ha.automatic-failover.enabled</name>
        <value>true</value>
    </property>

    <property>
        <name>ha.zookeeper.quorum</name>
        <value>zk1:2181,zk2:2181,zk3:2181</value>
    </property>
</configuration>
```
### 3. Запуск JournalNode
1. Копирование конфигурации: Убедитесь, что файл hdfs-site.xml скопирован на каждый узел JournalNode.

2. Создание директории для журнала: На каждом узле JournalNode создайте директорию для хранения журналов:

```bash
mkdir -p /path/to/journal/directory
```
3. Запуск JournalNode: На каждом узле выполните команду для запуска JournalNode:

```bash
hadoop-daemon.sh start journalnode
```
### 4. Настройка NameNode
После настройки JournalNode необходимо настроить NameNode для использования QJM.

1. Форматирование NameNode (если это первый запуск):

```bash
hdfs namenode -format
```
2. Синхронизация Standby NameNode:

```bash
hdfs namenode -bootstrapStandby
```
### 5. Проверка работы
1. Убедитесь, что JournalNode запущены и работают:

```bash
jps
```
Должен отображаться процесс JournalNode на каждом узле.

2. Проверка состояния NameNode и его синхронизации:

```bash
hdfs haadmin -getServiceState nn1
hdfs haadmin -getServiceState nn2
```
### 6. Тестирование отказоустойчивости
1. Отключите один из узлов JournalNode, чтобы проверить, как система реагирует на отказ:

```bash
hadoop-daemon.sh stop journalnode
```
2. Проверьте, что система продолжает работать с оставшимися узлами:

```bash
hdfs haadmin -failover nn1 nn2
```
3. Восстановите узел JournalNode и запустите его снова:

```bash
hadoop-daemon.sh start journalnode
```

# Заключение

Quorum Journal Manager (QJM) обеспечивает высокую доступность и отказоустойчивость в HDFS, позволяя нескольким узлам JournalNode вести журналы изменений. Настройка QJM включает в себя создание и настройку JournalNode, настройку файлов конфигурации и тестирование отказоустойчивости. Используя QJM, вы можете быть уверены в консистентности и доступности ваших данных в HDFS.