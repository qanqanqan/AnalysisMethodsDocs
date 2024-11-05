Управление зависимостями между задачами в Apache Airflow является ключевым аспектом, который позволяет определять порядок выполнения задач в рамках рабочего процесса (DAG). Это достигается с помощью различных методов и операторов, которые обеспечивают гибкость и контроль над выполнением задач.

## Основные концепции

### Upstream и Downstream Задачи
- **Upstream задачи**: Это задачи, которые должны быть выполнены до начала выполнения текущей задачи.
- **Downstream задачи**: Это задачи, которые зависят от выполнения текущей задачи.

## Установка зависимостей

### Методы установки зависимостей
В Airflow существует несколько способов установки зависимостей между задачами:

1. **Методы `set_upstream()` и `set_downstream()`**:
   Эти методы позволяют явно установить зависимости между задачами.
   ```python
   task_a.set_downstream(task_b)  # task_b будет выполняться после task_a
   task_b.set_upstream(task_a)      # эквивалентно предыдущему
   ```

2. **Операторы битового сдвига (`>>` и `<<`)**:
   Этот синтаксис более читаем и рекомендуется для использования.
   ```python
   task_a >> task_b  # task_b будет выполняться после task_a
   ```

3. **Списки и кортежи**:
   Для установки зависимостей между несколькими задачами можно использовать списки или кортежи.
   ```python
   task_a >> [task_b, task_c]  # task_b и task_c будут выполняться после task_a
   ```

### Пример установки зависимостей
```python
from airflow import DAG
from airflow.operators.dummy import DummyOperator
from datetime import datetime

with DAG('example_dag', start_date=datetime(2023, 1, 1), schedule_interval='@daily') as dag:
    task_a = DummyOperator(task_id='task_a')
    task_b = DummyOperator(task_id='task_b')
    task_c = DummyOperator(task_id='task_c')
    task_d = DummyOperator(task_id='task_d')

    # Установка зависимостей
    task_a >> [task_b, task_c]  # После выполнения task_a выполняются task_b и task_c
    [task_b, task_c] >> task_d   # После выполнения обоих выполняется task_d
```

## Правила триггера

Airflow предоставляет различные правила триггера, которые определяют, когда задача должна выполняться в зависимости от состояния upstream задач:

- **all_success**: Задача запускается только если все upstream задачи завершились успешно (значение по умолчанию).
- **all_failed**: Задача запускается, если все upstream задачи завершились с ошибкой.
- **all_done**: Задача запускается, когда все upstream задачи завершены (успех или ошибка).
- **one_success**: Задача запускается, если хотя бы одна из upstream задач завершилась успешно.
- **one_failed**: Задача запускается, если хотя бы одна из upstream задач завершилась с ошибкой.

### Пример использования правила триггера
```python
task_d = DummyOperator(
    task_id='task_d',
    trigger_rule='one_success'  # Запустится, если хотя бы одна из upstream задач успешна
)
```

## Заключение

Управление зависимостями между задачами в Apache Airflow позволяет создавать гибкие и мощные рабочие процессы. Использование методов установки зависимостей и правил триггера дает возможность точно контролировать порядок выполнения задач в зависимости от их состояния. Это делает Airflow эффективным инструментом для автоматизации процессов обработки данных.

Citations:
[1] https://stackoverflow.com/questions/55630686/how-to-set-a-downstream-task-after-finish-of-2-tasks
[2] https://www.astronomer.io/docs/learn/managing-dependencies
[3] https://airflow.apache.org/docs/apache-airflow/1.10.3/concepts.html
[4] https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/tasks.html
[5] https://github.com/apache/airflow/issues/35062
[6] https://gist.github.com/BenjaminDebeerst/fc217aa178aa648db2aa04ab6fdd71ae
[7] https://airflow.apache.org/docs/apache-airflow/stable/_api/airflow/operators/python/index.html
[8] https://www.restack.io/docs/airflow-faq-tutorial-taskflow-05