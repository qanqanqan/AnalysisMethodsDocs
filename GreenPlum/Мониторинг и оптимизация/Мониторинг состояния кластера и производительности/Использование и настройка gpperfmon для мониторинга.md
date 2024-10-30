## Использование и настройка gpperfmon для мониторинга в Greenplum

**gpperfmon** — это инструмент, предназначенный для мониторинга производительности в Greenplum. Он собирает и хранит данные о выполнении SQL-запросов и системной статистике, что позволяет администраторам отслеживать состояние кластера и выявлять узкие места в производительности.

### Установка и настройка gpperfmon

1. **Создание базы данных gpperfmon**:
   Для начала необходимо создать базу данных gpperfmon с помощью утилиты `gpperfmon_install`. Эта утилита автоматизирует процесс установки и настройки агентов сбора данных.

   ```bash
   gpperfmon_install --port <gpdb_port> --enable --password <gpmon_password>
   ```

   При этом:
   - `--port` указывает порт базы данных Greenplum.
   - `--enable` активирует агентов сбора данных.
   - `--password` задает пароль для суперпользователя `gpmon`, который будет использоваться для подключения агентов к базе данных[2][3].

2. **Настройка конфигурации**:
   После установки необходимо настроить параметры в файле конфигурации `gpperfmon.conf`, который находится в каталоге `$MASTER_DATA_DIRECTORY/gpperfmon/conf/`. Важно убедиться, что следующие параметры активированы:

   - `gp_enable_gpperfmon=on`
   - `gpperfmon_port=8888`

   Изменения в конфигурации требуют перезапуска сервера Greenplum с помощью команды:

   ```bash
   gpstop -r
   ```

3. **Мониторинг производительности**:
   После настройки gpperfmon, агенты на каждом сегменте будут собирать метрики производительности и передавать их на координатор. Данные собираются каждые 15 секунд по умолчанию, но этот интервал можно изменить в конфигурации с помощью параметра `quantum`[1][3].

### Использование Greenplum Command Center

Greenplum Command Center (GPCC) использует данные из базы данных gpperfmon для визуализации производительности кластера. Он предоставляет интерфейс для:

- Просмотра текущих и исторических метрик производительности.
- Отслеживания выполнения SQL-запросов.
- Анализа системных ресурсов, таких как использование CPU и памяти.

GPCC позволяет администраторам отменять проблемные запросы, что способствует оптимизации работы кластера[1][3].

### Заключение

Интеграция и настройка gpperfmon в Greenplum являются ключевыми шагами для эффективного мониторинга производительности. Правильная установка и конфигурация агентов сбора данных обеспечивают возможность получения актуальной информации о состоянии системы, что позволяет своевременно реагировать на проблемы и оптимизировать выполнение запросов.

Citations:
[1] https://bigdataschool.ru/blog/what-is-vmware-greenplum-command-center.html
[2] https://docs.vmware.com/en/VMware-Greenplum/5/greenplum-database/utility_guide-admin_utilities-gpperfmon_install.html
[3] https://docs.vmware.com/en/VMware-Greenplum/6/greenplum-database/ref_guide-gpperfmon-dbref.html
[4] https://habr.com/ru/companies/otus/articles/682990/
[5] https://bigdataschool.ru/blog/greenplum-hadoop-integration-with-pxf-connectors.html
[6] https://habr.com/ru/companies/arenadata/articles/564552/
[7] https://bigdataschool.ru/blog/pxf-greenplum-sql-optimization.html
[8] https://dzen.ru/a/Zo03GsA3sj6aao5a