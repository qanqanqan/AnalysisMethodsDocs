В Apache Airflow исполнители (Executors) играют ключевую роль в выполнении задач в рабочих процессах (DAG). Они определяют, как и где будут исполняться задачи, и могут быть настроены для различных сценариев использования. Рассмотрим основные типы исполнителей в Airflow.

## Типы Исполнителей

### Локальные Исполнители

#### Секвенциальный Исполнитель
- **Функционал**: Выполняет задачи последовательно, одна за другой. Это самый простой исполнитель, который подходит для разработки и тестирования.
- **Применение**: Используется по умолчанию, но не рекомендуется для производственных сред из-за ограниченной параллельности.

#### Локальный Исполнитель
- **Функционал**: Позволяет параллельное выполнение нескольких задач на одной машине.
- **Применение**: Подходит для небольших установок в производственной среде, где есть необходимость в многозадачности на одной машине.

### Ремотные Исполнители

#### Келлерский Исполнитель
- **Функционал**: Распределяет задачи между несколькими машинами с использованием очереди сообщений (например, RabbitMQ).
- **Применение**: Подходит для горизонтального масштабирования на кластере машин.

#### Кластерный Келлерский Исполнитель
- **Функционал**: Сочетает возможности CeleryExecutor и KubernetesExecutor, позволяя горизонтальное масштабирование через кластер рабочих узлов с использованием очереди сообщений и выполнения задач в кластере Kubernetes.

#### Кластерный Контейнерный Исполнитель
- **Функционал**: Запускает задачи в виде отдельных контейнеров в кластере Kubernetes.
- **Применение**: Обеспечивает отличную масштабируемость и надежность, особенно для облачных решений.

### Другие Опции

- **AWS ECS Экскьютор**: Экспериментальный исполнитель, предлагаемый для использования в облаке Amazon Web Services.
- **AWS Бэтч Экскьютор**: Также экспериментальный вариант для выполнения задач в AWS.

## Конфигурирование и Переключение Исполнителя

- **Установка Исполнителя**: Можно управлять исполнителем, изменяя соответствующий класс экскьютора в секции `[core]` конфигурационного файла Airflow (`airflow.cfg`). Например:
    ```ini
    [core]
    executor = KubernetesExecutor 
    ```
  
- **Переключение Исполнителя**: Чтобы сменить исполнитель, достаточно изменить значение `executor` в конфигурационном файле.

## Технологическая Инфраструктура

- **Decoupling Executors (АИП-51)**: Новое решение, позволяющее сторонним разработчикам создавать свои собственные исполнители без необходимости изменения исходного кода Airflow. Это улучшает модульность и гибкость системы.

Таким образом, выбор правильного типа исполнителя является критически важным для оптимального функционирования и масштабирования рабочих процессов в Apache Airflow.

Citations:
[1] https://www.linkedin.com/pulse/apache-airflow-executors-101-shahab-nasir
[2] https://airflow.apache.org/docs/apache-airflow/2.3.2/executor/index.html
[3] https://www.restack.io/docs/airflow-knowledge-executor-worker-local-apache
[4] https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/executor/index.html
[5] https://www.astronomer.io/docs/learn/airflow-executors-explained
[6] https://www.youtube.com/watch?v=VFC0E6Oyj7A
[7] https://www.youtube.com/watch?v=TQIInLmKM4k
[8] https://habr.com/ru/companies/alfa/articles/676926/