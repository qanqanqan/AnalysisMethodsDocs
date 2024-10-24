## Какие основные функции ResourceManager в YARN?

ResourceManager (RM) — это один из ключевых компонентов YARN (Yet Another Resource Negotiator), который управляет распределением ресурсов в кластере Hadoop. Он играет центральную роль в обеспечение эффективной работы приложений, обеспечивая управление и координацию ресурсов, доступных в кластере. Вот основные функции ResourceManager:

### Основные функции ResourceManager:

1. Управление ресурсами:
- ResourceManager отслеживает общую доступность ресурсов (таких как память и вычислительная мощность) в кластере и управляет распределением этих ресурсов между различными приложениями, которые запрашивают их для выполнения.



2. Обработка заявок на ресурсы:
- RM принимает заявки на ресурсы от различных ApplicationMaster (AM), которые представляют приложения. Он определяет, какие ресурсы доступны, и решает, насколько эффективно использовать их для обслуживания этих заявок.



3. Планирование задач:
- ResourceManager применяет различные алгоритмы планирования для распределения ресурсов на основании политики, заданной в конфигурации. Эти политики могут включать такие факторы, как приоритет приложений, требования к ресурсам и текущая загрузка узлов.



4. Мониторинг состояния узлов:
- ResourceManager регулярно получает обновления о состоянии всех NodeManager (NM) в кластере. Он отслеживает, какие узлы активны и какие ресурсы доступны на каждом из них. Если NodeManager выходит из строя, RM может перераспределить задачи на другие рабочие узлы.



5. Управление жизненным циклом ApplicationMaster:
- ResourceManager управляет запусками, остановкой и отслеживанием состояния ApplicationMaster для всех активных приложений. ApplicationMaster отвечает за выполнение конкретного приложения и периодически общается с ResourceManager для запроса дополнительных ресурсов или отчётности о статусе выполнения.



6. Обеспечение отказоустойчивости:
- ResourceManager реализует механизмы для обработки сбоев как на уровне ApplicationMaster, так и на уровне NodeManager. Если ApplicationMaster выходит из строя, RM может создать новый экземпляр AM для продолжения работы приложения.



7. Сбор статистики и отчётность:
- ResourceManager собирает и хранит статистику о состоянии ресурсов, запущенных приложениях, их производительности и использовании ресурсов, что полезно для мониторинга и анализа работы кластера.



8. Поддержка различных фреймворков:
- Благодаря архитектуре YARN ResourceManager может работать с различными фреймворками обработки данных, такими как MapReduce, Apache Spark, Apache Tez и другими, обеспечивая универсальность системы для различных типов задач.