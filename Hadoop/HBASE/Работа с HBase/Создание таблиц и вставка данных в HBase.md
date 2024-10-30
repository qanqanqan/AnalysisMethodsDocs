Для создания таблиц и вставки данных в HBase используются команды, которые позволяют эффективно управлять данными в распределенной базе данных. 

### Создание таблиц

Для создания таблицы в HBase используется команда `create`, которая принимает имя таблицы и список семейств колонок. Например, команда:

```bash
create 'emp', 'personal data', 'professional data'
```

создает таблицу с именем `emp`, содержащую два семейства колонок: `personal data` и `professional data`. Каждое семейство колонок может содержать неограниченное количество столбцов, что позволяет гибко управлять структурой данных.

### Вставка данных

Для вставки данных в созданную таблицу используется команда `put`, которая позволяет добавлять новые записи или обновлять существующие. Синтаксис команды выглядит следующим образом:

```bash
put 'emp', '1', 'personal data:name', 'raju'
put 'emp', '1', 'personal data:city', 'hyderabad'
put 'emp', '1', 'professional data:salary', '50000'
```

В этом примере данные вставляются для строки с ключом `1`. Первые три команды добавляют информацию о сотруднике, включая его имя, город и зарплату. Если строка с ключом `1` уже существует, то значения будут обновлены новыми данными. HBase автоматически управляет версиями значений, что позволяет хранить несколько версий для каждого столбца.

Citations:
[1] https://ir.nmu.org.ua/bitstream/handle/123456789/2087/HBASE.pdf?isAllowed=y&sequence=1
[2] https://bigdataschool.ru/wiki/hbase-wiki
[3] https://e-learning.bmstu.ru/iu6/pluginfile.php/8221/mod_resource/content/2/Hbase.pptx
[4] https://bigdataschool.ru/wiki/hbase
[5] https://docs.arenadata.io/ru/ADH/current/concept/hbase/data_model.html
[6] https://docs.arenadata.io/ru/ADH/current/how-to/hbase/built-in-mr-jobs.html
[7] https://bigdataschool.ru/blog/best-practices-to-optimize-hbase.html
[8] https://hbase.apache.org/book.html