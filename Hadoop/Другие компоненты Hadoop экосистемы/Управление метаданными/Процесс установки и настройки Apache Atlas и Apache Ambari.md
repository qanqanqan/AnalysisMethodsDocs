# Процесс установки и настройки Apache Atlas и Apache Ambari

Установка и настройка **Apache Atlas** и **Apache Ambari** являются важными шагами для эффективного управления данными и кластером Hadoop. В этом документе описаны основные этапы установки и настройки этих инструментов.

## 1. **Установка Apache Atlas**

### 1.1. **Системные требования**
- **Операционная система**: Linux (например, CentOS, Ubuntu)
- **Java**: Oracle JDK или OpenJDK 8+
- **Apache Hadoop**: установленный и настроенный Hadoop-кластер

### 1.2. **Скачивание Atlas**
1. Перейдите на [официальный сайт Apache Atlas](https://atlas.apache.org/) и загрузите последнюю версию.
2. Распакуйте архив в желаемую директорию:

   ```bash
   tar -xzf apache-atlas-<version>.tar.gz
   cd apache-atlas-<version>
   ```
### 1.3. Настройка Atlas

Откройте файл конфигурации atlas-application.properties, который находится в каталоге conf.

Настройте параметры подключения к Hadoop и базам данных (например, Apache HBase, MySQL или PostgreSQL):

```properties
atlas.graph.storage.backend=hbase
atlas.graph.storage.hostname=<HBase-hostname>
atlas.graph.storage.zookeeper.quorum=<zookeeper-host>
```

Убедитесь, что настройки безопасности, такие как Kerberos, правильно указаны (если применимо).

### 1.4. Запуск Atlas

Запустите Apache Atlas с помощью скрипта atlas_start.py:

```bash
bin/atlas_start.py
```
Проверьте статус сервиса:

```bash
curl http://localhost:21000/atlas
```
Если все работает корректно, вы увидите сообщение о статусе.

## 2. Установка Apache Ambari
### 2.1. Системные требования
- Операционная система: Linux (например, CentOS, RHEL, Ubuntu)
- Java: Oracle JDK или OpenJDK 8+
- Сетевое окружение: доступ к узлам кластера Hadoop
### 2.2. Скачивание Ambari
- Перейдите на официальный сайт Apache Ambari и загрузите последнюю версию.
- Установите Ambari через репозиторий. Для CentOS/RHEL:

```bash
sudo wget -O /etc/yum.repos.d/ambari.repo http://public-repo-1.hortonworks.com/ambari/centos7/2.x/updates/2.7.4.0-1/ambari.repo
sudo yum install ambari-server ambari-agent
```
### 2.3. Настройка Ambari
Запустите настройку Ambari Server:

```bash
ambari-server setup
```
Выберите необходимые параметры, такие как база данных, параметры Kerberos (если применимо) и другие настройки.

Настройте Ambari Agent на каждом узле кластера, отредактировав файл конфигурации /etc/ambari-agent/conf/ambari-agent.ini:

```ini
[server]
hostname=<ambari-server-hostname>
```
### 2.4. Запуск Ambari
Запустите Ambari Server:

```bash
ambari-server start
```
Запустите Ambari Agent на каждом узле:

```bash
ambari-agent start
```
### 2.5. Доступ к Ambari Web UI
- Откройте браузер и перейдите по адресу: http://<ambari-server-hostname>:8080.
- Введите учетные данные (по умолчанию: admin/admin) для доступа к интерфейсу управления.
## Заключение
Установка и настройка Apache Atlas и Apache Ambari позволяют эффективно управлять метаданными и кластером Hadoop. Следуя указанным шагам, вы сможете быстро развернуть и настроить эти инструменты для вашей организации.