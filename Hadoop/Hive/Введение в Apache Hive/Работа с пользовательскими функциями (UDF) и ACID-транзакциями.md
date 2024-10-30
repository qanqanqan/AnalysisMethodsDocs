## Работа с пользовательскими функциями (UDF) и ACID-транзакциями

Apache Hive предоставляет возможность использования пользовательских функций (User Defined Functions, UDF) для расширения функциональности и выполнения специфических задач по обработке данных. Кроме того, Hive поддерживает ACID-транзакции, которые обеспечивают надежное управление изменениями в базах данных. Рассмотрим оба этих аспекта более подробно.

### 1. Работа с пользовательскими функциями (UDF)

Пользовательские функции в Hive позволяют разработчикам создавать индивидуальные функции для выполнения операций, которые не предусмотрены стандартными функциями Hive. Это полезно, когда необходимо выполнить специфические вычисления или преобразования данных.

#### Создание UDF

Чтобы создать пользовательскую функцию в Hive, необходимо выполнить следующие шаги:

1. Написать код на Java: Вам нужно создать класс, который расширяет UDF и переопределяет метод evaluate. Пример:

```java
   import org.apache.hadoop.hive.ql.exec.Description;
   import org.apache.hadoop.hive.ql.exec.UDF;

   @Description(name = "my_custom_function",
                value = "_FUNC_(input) - Returns custom output")
   public class MyCustomFunction extends UDF {
       public String evaluate(String input) {
           // Логика обработки данных
           return input.toUpperCase(); // Пример: преобразование строки в верхний регистр
       }
   }
```

2. Скомпилировать JAR-файл: После написания Java-кода необходимо скомпилировать его в JAR-файл.

3. Загрузить JAR в Hive: Используйте команду `ADD JAR` для того, чтобы загрузить скомпилированный JAR-файл в среду Hive.

```sql
    ADD JAR /path/to/my_custom_function.jar;
```

4. Зарегистрировать функцию: Используйте команду `CREATE TEMPORARY FUNCTION` для регистрации функции в Hive.

```sql
   CREATE TEMPORARY FUNCTION my_custom_function AS 'com.example.MyCustomFunction';
```

5. Использование UDF в запросах: Теперь вы можете использовать вашу пользовательскую функцию в запросах, например:

```sql
   SELECT my_custom_function(name) FROM employees;
```

### 2. ACID-транзакции в Hive

Hive начиная с версии 0.14 поддерживает ACID-транзакции. Это позволяет пользователям выполнять операции вставки, обновления и удаления с гарантией целостности данных.

#### Основные возможности ACID

- Транзакционность: Все операции над данными (вставка, обновление, удаление) обрабатываются как единое целое. Если одна из операций не удается, вся транзакция откатывается.
- Изоляция: Позволяет нескольким пользователям одновременно вносить изменения в базу данных, сохраняя при этом целостность данных.
- Устойчивость: После завершения транзакции все изменения сохраняются, даже в случае сбоя системы.

#### Как включить ACID-транзакции в Hive

Чтобы включить ACID-транзакции в настройках Hive, необходимо выполнить следующие шаги:

1. Конфигурация Hive: В файле `hive-site.xml` необходимо указать следующие параметры:

```xml
   <property>
       <name>hive.support.concurrency</name>
       <value>true</value>
   </property>
   <property>
       <name>hive.exec.dynamic.partition.mode</name>
       <value>nonstrict</value>
   </property>
   <property>
       <name>hive.txn.manager</name>
       <value>org.apache.hadoop.hive.ql.lockmgr.DbTxnManager</value>
   </property>
   <property>
       <name>hive.compactor.initiator.on</name>
       <value>true</value>
   </property>
   <property>
       <name>hive.compactor.worker.threads</name>
       <value>1</value>
   </property>
   <property>
       <name>hive.metastore.transactional</name>
       <value>true</value>
   </property>
```

2. Создание таблиц с поддержкой ACID: При создании таблицы необходимо указать параметр TBLPROPERTIES для активации транзакционной поддержки:

```sql
   CREATE TABLE employees (
       id INT,
       name STRING
   ) CLUSTERED BY (id) INTO 10 BUCKETS
   TBLPROPERTIES ('transactional'='true');
```

3. Выполнение транзакций: После настройки можно выполнять транзакции:

- Вставка (необходимо использовать INSERT INTO для дополнительных строк):

```sql
     INSERT INTO employees VALUES (1, 'Alice'), (2, 'Bob');
```

- Обновление:

```sql
     UPDATE employees SET name = 'Charlie' WHERE id = 1;
```

- Удаление:

```sql
    DELETE FROM employees WHERE id = 2;
```