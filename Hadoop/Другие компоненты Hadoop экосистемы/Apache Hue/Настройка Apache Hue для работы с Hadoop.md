# Настройка Apache Hue для работы с Hadoop

Настройка Apache Hue для работы с Hadoop включает несколько ключевых этапов, от установки до конфигурации различных компонентов.

## Шаги по настройке Apache Hue

### 1.  **Установка Hue**

#### a. Зависимости

Убедитесь, что у вас установлены следующие компоненты:

-   Apache Hadoop
-   Apache Hive (если планируете работать с Hive)
-   Apache HBase (если планируете работать с HBase)
-   Apache Impala (по желанию)

#### b. Установка Hue

Существует несколько способов установки Hue:

-   **Скачивание и сборка из исходников**:  `bash git clone https://github.com/cloudera/hue.git cd hue make apps`
    
-   **Использование пакетов**  (например, для Ubuntu):  `bash sudo apt-get install hue`

**На кластерах HDInsight**:

-   Для установки Hue на кластере HDInsight рекомендуется использовать скрипт. Например, можно выполнить следующую команду: `https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh` 
    
-   Убедитесь, что головной узел имеет не менее 8 ядер и 14 ГБ памяти

-   **На локальном кластере**:
    -   Скачайте Hue с официального репозитория и выполните установку. Убедитесь, что все зависимости, такие как Python и необходимые библиотеки, установлены.

### 2.  **Настройка конфигураций**

-   **Создание пользователя**:
    
    -   Создайте пользователя `hue` в системе и настройте необходимые права доступа к HDFS:  `sudo  useradd -u 54336 hue sudo  chown -R hue:hue /usr/local/hue sudo  su - hdfs -c "hdfs dfs -mkdir /user/hue" sudo  su - hdfs -c "hdfs dfs -chown hue:hadoop /user/hue"` 

-   **Настройка базы данных**:
    -   Hue требует базу данных для хранения метаданных. Например, можно использовать MySQL: `CREATE  DATABASE desktop; GRANT  ALL  PRIVILEGES  ON  *.*  TO  'hue'@'localhost'; ALTER  USER  'hue'@'localhost' IDENTIFIED BY  'secretpassword'; FLUSH PRIVILEGES;`

#### a.  **Редактирование конфигурационного файла**: `hue.ini`

После установки Hue вам нужно настроить файл конфигурации  `hue.ini`. Обычно он находится в директории  `/etc/hue/`  или в папке установки Hue.

-   Откройте файл `hue.ini` и настройте параметры подключения к HDFS, Hive и другим компонентам:`[[database]] engine=mysql host=localhost port=3306 user=hue password=secretpassword name=desktop`

-   Укажите информацию о вашем кластере Hadoop:  `ini [desktop] secret_key=your_secret_key`
    
-   Настройка подключения к Hive:  `ini [hive] interface=beeswax beeswax_server_host=your_hive_server_host beeswax_server_port=10000`
    
-   Если вы используете Impala, добавьте:  `ini [impala] interface=impala impala_server_host=your_impala_server_host impala_server_port=21000`

#### b. Настройка компонентов Hadoop**

-   **Интеграция с HDFS**:

    -   Убедитесь, что Hue имеет доступ и может подключаться к HDFS: `namenode_host=your_namenode_host namenode_port=8020`. Это может потребовать настройки прав доступа и конфигурации `core-site.xml`.
    
-   **Настройка YARN**:

    -   Добавьте библиотеку MapReduce в классpath YARN для выполнения приложений: `$HADOOP_CONF_DIR,/usr/lib/hadoop-mapreduce/*,/usr/lib/hadoop/*,...`

### 3.  **Запуск Hue и тестирование подключения**

#### a. Запуск сервера

Вы можете запустить Hue с помощью следующей команды:  `bash cd /path/to/hue build/env/bin/hue runserver`. 

-   После завершения всех настроек запустите Hue с помощью команды: `cd /usr/local/hue/build/env/bin/ nohup ./hue runserver` 

#### b. Тестирование

По умолчанию Hue будет доступен по адресу `http://<your-hue-server>:8888`.

- После всех настроек, откройте веб-браузер и чтобы проверьте доступность интерфейса перейдите по адресу  `http://localhost:8888`. Вы должны увидеть интерфейс Hue. Попробуйте выполнить простые SQL-запросы к Hive или Impala, чтобы убедиться, что все работает корректно.

### 4.  **Настройка базы данных**

Hue требует базу данных для хранения метаданных. Обычно используется SQLite по умолчанию, но рекомендуется использовать более производительную СУБД, такую как MySQL или PostgreSQL.

-   Настройте  `hue.ini`  для работы с выбранной СУБД:  `ini [desktop] # Пример для MySQL sqlalchemy_connection=mysql://username:password@localhost/hue`

### 5.  **Настройка аутентификации**

Hue поддерживает различные механизмы аутентификации (LDAP, PAM и т.д.). Настройте аутентификацию в  `hue.ini`: `auth_backend=desktop.auth.backend.ldap`

### 6.  **Безопасность**

Если ваш кластер защищен Kerberos, настройте аутентификацию в Hue, добавив необходимые параметры в конфигурацию и перезапустив сервисы.

## Заключение

Настройка Apache Hue для работы с Hadoop требует внимательного подхода к установке и конфигурации различных компонентов и может показаться сложной, но следуя каждому вышеуказанному шагу, вы сможете успешно интегрировать Hue в свою экосистему Hadoop и начать эффективно работать с данными.