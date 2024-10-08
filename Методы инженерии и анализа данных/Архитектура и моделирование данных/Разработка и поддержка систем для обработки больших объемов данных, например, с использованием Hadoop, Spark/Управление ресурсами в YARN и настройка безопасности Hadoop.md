### Управление ресурсами в YARN

**YARN (Yet Another Resource Negotiator)** — это система управления ресурсами в экосистеме Hadoop, которая отделяет управление ресурсами от выполнения вычислений. Основная задача YARN — распределение вычислительных ресурсов (CPU, память) между приложениями, работающими на кластере.

#### Основные компоненты YARN

1. **ResourceManager (Менеджер ресурсов)**  
   Это центральный компонент, который управляет ресурсами кластера и координирует их распределение между приложениями. Он отвечает за глобальное планирование и выделение ресурсов для каждого приложения.

2. **NodeManager (Менеджер узла)**  
   NodeManager работает на каждом узле кластера и отвечает за управление ресурсами на этом узле. Он контролирует контейнеры (containers), в которых выполняются задачи, и периодически отправляет отчёты о состоянии ресурсов в ResourceManager.

3. **ApplicationMaster**  
   Для каждого приложения создаётся собственный ApplicationMaster, который управляет выполнением конкретного приложения и координирует выделенные для него ресурсы. Он взаимодействует с ResourceManager для запроса дополнительных ресурсов и отслеживает выполнение задач приложения.

4. **Контейнеры (Containers)**  
   Контейнеры — это основные единицы выполнения задач в YARN, которым выделяются определённые ресурсы (память и CPU). Каждый контейнер выполняет одну или несколько задач, запущенных ApplicationMaster.

#### Как происходит распределение ресурсов в YARN

1. **Запрос ресурсов**  
   Когда приложение (например, Spark или MapReduce) запускается, оно отправляет запрос на ресурсы в ResourceManager. Этот запрос включает требования к памяти и процессорам для выполнения задач.

2. **Выделение ресурсов**  
   ResourceManager принимает решение о выделении ресурсов, основываясь на текущем состоянии кластера и доступных ресурсах. После этого ресурсы выделяются контейнерам на узлах кластера.

3. **Запуск контейнеров**  
   Контейнеры запускаются на узлах, управляемых NodeManager. NodeManager следит за тем, чтобы контейнеры не использовали больше ресурсов, чем было выделено, и отправляет отчёты о состоянии контейнеров ResourceManager.

4. **Мониторинг и завершение**  
   ResourceManager и NodeManager совместно следят за состоянием ресурсов и выполнения задач. Если контейнер завершает выполнение задачи, ресурсы освобождаются и могут быть распределены для других приложений.

#### Настройка управления ресурсами в YARN

1. **Параметры памяти и CPU**  
   Для настройки распределения памяти и процессоров используются параметры:
   - **yarn.nodemanager.resource.memory-mb** — общий объём памяти на узле, доступный для контейнеров.
   - **yarn.nodemanager.resource.cpu-vcores** — общее количество процессоров (ядра CPU), доступных на узле.
   - **yarn.scheduler.maximum-allocation-mb** — максимальный объём памяти, который может быть выделен для одного контейнера.
   - **yarn.scheduler.maximum-allocation-vcores** — максимальное количество процессоров для одного контейнера.

2. **Алгоритмы распределения ресурсов**  
   YARN поддерживает несколько планировщиков ресурсов:
   - **CapacityScheduler**: Позволяет резервировать ресурсы для разных пользователей и очередей. Подходит для многопользовательской среды, где необходимо гарантировать ресурсы для определённых групп.
   - **FairScheduler**: Обеспечивает справедливое распределение ресурсов между приложениями, не позволяя одному приложению монополизировать кластер.

3. **Динамическое выделение ресурсов (Dynamic Resource Allocation)**  
   Spark и другие приложения могут использовать динамическое выделение ресурсов, где количество контейнеров (и, соответственно, ресурсов) изменяется в зависимости от нагрузки.

### Настройка безопасности в Hadoop

Безопасность в Hadoop — это важный аспект, так как Hadoop часто используется для хранения и обработки конфиденциальных данных. Для обеспечения безопасности в Hadoop используется несколько уровней защиты: аутентификация, авторизация, шифрование и аудит.

#### 1. **Аутентификация**

Аутентификация подтверждает личность пользователей и сервисов, которые пытаются получить доступ к кластеру Hadoop. Основной механизм аутентификации в Hadoop — это интеграция с **Kerberos**.

- **Kerberos** — это протокол аутентификации, который использует тикеты для подтверждения подлинности пользователей и сервисов. Когда пользователь или сервис пытается получить доступ к ресурсу Hadoop, он должен пройти аутентификацию через Kerberos.
  
   Настройка Kerberos включает:
   - Конфигурацию **KDC (Key Distribution Center)**, который выдает тикеты.
   - Настройку **krb5.conf** на всех узлах кластера, чтобы использовать Kerberos для аутентификации.

Пример конфигурации в `core-site.xml`:
```xml
<property>
  <name>hadoop.security.authentication</name>
  <value>kerberos</value>
</property>
```

#### 2. **Авторизация**

Авторизация контролирует, какие действия пользователи могут выполнять в кластере после аутентификации. В Hadoop существует несколько уровней авторизации:
- **HDFS-права доступа**: Пользователи и группы могут получать доступ к файлам и директориям в HDFS на основе стандартных Unix-прав доступа (чтение, запись, выполнение).
  
Пример конфигурации прав доступа:
```bash
hdfs dfs -chmod 750 /secure-directory
hdfs dfs -chown user1:group1 /secure-directory
```

- **YARN-контроль доступа**: Можно настроить доступ к ресурсам YARN с помощью политик, которые ограничивают выполнение приложений определёнными пользователями или группами.

#### 3. **Шифрование данных**

- **Шифрование данных на месте (at-rest encryption)**: Данные, хранящиеся в HDFS, могут быть зашифрованы с помощью механизма **Encryption Zones**. В каждой зоне шифрования файлы автоматически шифруются при записи и расшифровываются при чтении. Шифрование осуществляется с использованием ключей, управляемых системой **KMS (Key Management Server)**.

Пример создания зоны шифрования:
```bash
hdfs crypto -createZone -keyName myKey -path /encrypted-data
```

- **Шифрование данных "на лету" (in-transit encryption)**: Данные, передаваемые между узлами или клиентами и HDFS, могут быть зашифрованы с использованием **TLS/SSL** для защиты от перехвата.

Пример настройки шифрования:
```xml
<property>
  <name>dfs.encrypt.data.transfer</name>
  <value>true</value>
</property>
```

#### 4. **Аудит (Audit Logging)**

Hadoop поддерживает аудит, что позволяет отслеживать все действия пользователей и сервисов в кластере. Аудит позволяет записывать информацию о доступе к файлам, запуске приложений и других действиях в журнал, что помогает обеспечивать прозрачность и безопасность.

Для настройки аудита можно использовать такие инструменты, как **Apache Ranger** и **Apache Sentry**, которые обеспечивают централизованное управление политиками безопасности и запись событий аудита.

#### Заключение

Управление ресурсами в YARN позволяет эффективно распределять вычислительные ресурсы между приложениями в кластере Hadoop, обеспечивая гибкость и масштабируемость. Важную роль также играет настройка безопасности в Hadoop, которая включает аутентификацию через Kerberos, контроль доступа с помощью прав в HDFS и YARN, шифрование данных как на уровне хранения, так и при передаче, а также ведение журнала аудита для отслеживания действий пользователей.