# Конфигурация PXF для интеграции с HDFS

Для интеграции Greenplum PXF (Platform Extension Framework) с HDFS необходимо настроить PXF так, чтобы он мог подключаться к HDFS и получать доступ к данным, хранящимся в кластере Hadoop. Ниже приведены шаги настройки конфигурации PXF для работы с HDFS.

### Шаги конфигурации PXF для интеграции с HDFS

#### 1. Убедитесь, что PXF установлен и запущен

1. **Установка PXF**: В большинстве случаев PXF устанавливается вместе с Greenplum. Если нет, установите его на всех узлах Greenplum.
2. **Включение PXF**: Убедитесь, что параметр `pxf_enable` включен в конфигурации Greenplum:
   ```bash
   gpconfig -c pxf_enable -v on
   ```
3. **Запуск PXF**: Запустите PXF на всех узлах кластера Greenplum:
   ```bash
   pxf cluster start
   ```

#### 2. Настройка конфигурации PXF для подключения к HDFS

Для подключения к HDFS необходимо предоставить PXF доступ к конфигурационным файлам Hadoop (`core-site.xml` и `hdfs-site.xml`), чтобы он мог распознавать параметры HDFS и подключаться к нему.

1. **Создание конфигурационного каталога для PXF**:
   - На каждом узле Greenplum создайте каталог `pxf/conf/servers/default`, если он еще не существует:
     ```bash
     mkdir -p $PXF_CONF/servers/default
     ```

2. **Копирование конфигурационных файлов HDFS**:
   - Скопируйте файлы `core-site.xml` и `hdfs-site.xml` из вашего кластера Hadoop в каталог `pxf/conf/servers/default` на каждом узле Greenplum:
     ```bash
     cp /path/to/hadoop/conf/core-site.xml $PXF_CONF/servers/default/
     cp /path/to/hadoop/conf/hdfs-site.xml $PXF_CONF/servers/default/
     ```
   - Эти файлы должны содержать параметры подключения к HDFS, такие как `fs.defaultFS`, порты и параметры безопасности (если HDFS настроен с Kerberos или SSL).

3. **Настройка доступа к файловой системе HDFS**:
   - Если ваш кластер HDFS требует аутентификации через Kerberos, необходимо также настроить PXF для работы с Kerberos. Это включает добавление соответствующих параметров в `pxf-site.xml` и настройку ключей Kerberos.

#### 3. Перезапуск PXF для применения настроек

После добавления конфигурационных файлов HDFS нужно перезапустить PXF, чтобы он применил изменения:
```bash
pxf cluster restart
```

#### 4. Создание внешней таблицы для доступа к данным в HDFS

Теперь, когда PXF настроен для работы с HDFS, можно создавать внешние таблицы в Greenplum для доступа к данным в HDFS.

1. **Создание внешней таблицы**:
   - Используйте команду `CREATE EXTERNAL TABLE` в Greenplum для создания таблицы, которая ссылается на данные в HDFS:
     ```sql
     CREATE EXTERNAL TABLE hdfs_table (
       id INT,
       name TEXT,
       age INT
     )
     LOCATION ('pxf://<hdfs_path>/path/to/data?PROFILE=hdfs:text')
     FORMAT 'TEXT' (DELIMITER ',');
     ```
   - Параметр `PROFILE` указывает PXF на используемый источник данных и формат (например, `hdfs:text` для текстового формата).

2. **Проверка работы внешней таблицы**:
   - Запустите запрос к созданной внешней таблице, чтобы убедиться в правильности подключения и конфигурации:
     ```sql
     SELECT * FROM hdfs_table LIMIT 10;
     ```

#### 5. Дополнительные настройки для HDFS с Kerberos

Если HDFS настроен на использование Kerberos, вам необходимо выполнить дополнительные шаги для аутентификации PXF.

1. **Настройка Kerberos в `pxf-site.xml`**:
   - В файле `$PXF_CONF/pxf-site.xml` добавьте следующие параметры:
     ```xml
     <configuration>
       <property>
         <name>pxf.service.kerberos.principal</name>
         <value>your_kerberos_principal</value>
       </property>
       <property>
         <name>pxf.service.kerberos.keytab</name>
         <value>/path/to/your_keytab.keytab</value>
       </property>
     </configuration>
     ```
   - Здесь `pxf.service.kerberos.principal` — это Kerberos-принципал для PXF, а `pxf.service.kerberos.keytab` — путь к файлу ключей Kerberos.

2. **Перезапуск PXF после настройки Kerberos**:
   - Перезапустите PXF, чтобы применить настройки Kerberos:
     ```bash
     pxf cluster restart
     ```

### Полезные советы

- **Мониторинг**: Проверяйте состояние PXF с помощью команды `pxf cluster status`, чтобы убедиться, что PXF успешно запущен на всех узлах.
- **Логи**: В случае ошибок при подключении к HDFS проверяйте логи PXF в каталоге `$PXF_CONF/logs` для диагностики.
- **Форматы данных**: PXF поддерживает различные форматы данных в HDFS (например, Parquet, Avro, Text), и для каждого формата потребуется указать соответствующий `PROFILE`.

### Пример полного рабочего процесса

1. Настройте PXF и перезапустите его на каждом узле:
   ```bash
   gpconfig -c pxf_enable -v on
   pxf cluster start
   ```
2. Скопируйте `core-site.xml` и `hdfs-site.xml` в `$PXF_CONF/servers/default/`.
3. Перезапустите PXF:
   ```bash
   pxf cluster restart
   ```
4. Создайте внешнюю таблицу в Greenplum для доступа к данным в HDFS:
   ```sql
   CREATE EXTERNAL TABLE hdfs_table (
     id INT,
     name TEXT,
     age INT
   )
   LOCATION ('pxf://<hdfs_path>/path/to/data?PROFILE=hdfs:text')
   FORMAT 'TEXT' (DELIMITER ',');
   ```
5. Проверьте доступ к данным:
   ```sql
   SELECT * FROM hdfs_table LIMIT 10;
   ```

Следуя этим шагам, PXF будет настроен для интеграции с HDFS и обеспечит доступ Greenplum к данным, хранящимся в Hadoop, через SQL-запросы.