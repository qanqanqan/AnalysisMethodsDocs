## Теория YARN и MapReduce

YARN (Yet Another Resource Negotiator) и MapReduce — это два ключевых компонента экосистемы Apache Hadoop, которые обеспечивают распределённую обработку и управление ресурсами в больших данных.

### YARN (Yet Another Resource Negotiator)

YARN — это архитектурный компонент, введённый в Hadoop 2.0, который отделяет управление ресурсами от обработки данных. Он отвечает за распределение ресурсов среди различных приложений и за управление рабочими процессами. Основные элементы YARN:

1. ResourceManager: Это главный управляющий элемент в YARN, который отвечает за распределение ресурсов на кластере между различными приложениями. Он управляет ресурсами и следит за состоянием работы узлов.

2. NodeManager: Это агент, работающий на каждом узле кластера, который отвечает за мониторинг работы контейнеров, управления ресурсами и обеспечения выполнения приложений.

3. ApplicationMaster (AM): Это процесс, который управляет жизненным циклом конкретного приложения (например, задания MapReduce). Он запрашивает ресурсы у ResourceManager и управляет выполнением задач.

### MapReduce

MapReduce — это модель программирования и система, используемая для обработки и генерации больших наборов данных с помощью простых операций "Map" и "Reduce". Основные этапы работы:

1. Map: На этом этапе комбинируются входные данные и преобразуются в пару ключ-значение. Каждая задача Map обрабатывает часть данных и передаёт её на следующий этап.

2. Shuffle and Sort: После завершения этапа Map данные передаются на этап Shuffle, где они сортируются и объединяются, чтобы подготовить их для обработки.

3. Reduce: На этом этапе система обрабатывает выходные пары ключ-значение, выполняя операции агрегации или суммирования и создавая итоговый вывод.

### Взаимосвязь между YARN и MapReduce

С переходом к YARN, архитектура выполнения MapReduce была улучшена. Вместо того, чтобы MapReduce контролировал выполнение своих задач и распределение ресурсов, YARN берёт на себя управление ресурсами, оставляя самому MapReduce фокусироваться на обработке данных. Это позволяет использовать YARN для управления различными типами приложений, не ограничиваясь только MapReduce.

Таким образом, YARN обеспечивает более гибкую и масштабируемую платформу для обработки больших данных, поддерживая различные парадигмы обработки помимо MapReduce, такие как Spark, Tez и другие.