Условное выполнение задач в Apache Airflow позволяет создавать гибкие и адаптивные рабочие процессы, где выполнение определенных задач зависит от условий, заданных ранее в DAG. В Airflow есть несколько операторов, которые помогают реализовать такую логику, включая **ShortCircuitOperator** и **BranchPythonOperator**. Рассмотрим их подробнее.

## 1. ShortCircuitOperator

**ShortCircuitOperator** используется для пропуска выполнения нижестоящих задач в зависимости от результата выполнения предыдущей задачи. Если функция, связанная с этим оператором, возвращает `False`, все нижестоящие задачи будут пропущены.

### Пример использования
```python
from airflow import DAG
from airflow.operators.python import ShortCircuitOperator
from airflow.operators.dummy import DummyOperator
from datetime import datetime

def condition():
    # Условие для выполнения
    return True  # или False

with DAG('short_circuit_example',
         start_date=datetime(2023, 1, 1),
         schedule_interval='@daily',
         catchup=False) as dag:

    short_circuit = ShortCircuitOperator(
        task_id='short_circuit_task',
        python_callable=condition,
    )

    task_a = DummyOperator(task_id='task_a')
    task_b = DummyOperator(task_id='task_b')

    short_circuit >> [task_a, task_b]
```

В этом примере, если `condition()` возвращает `False`, задачи `task_a` и `task_b` будут пропущены.

## 2. BranchPythonOperator

**BranchPythonOperator** позволяет реализовать ветвление в DAG на основе логики, определенной в функции Python. Этот оператор возвращает идентификатор задачи, которая должна быть выполнена. Все остальные задачи в ветвлении будут помечены как пропущенные.

### Пример использования
```python
from airflow import DAG
from airflow.operators.python import BranchPythonOperator
from airflow.operators.dummy import DummyOperator
from datetime import datetime

def choose_branch():
    # Логика выбора ветви
    return 'branch_a'  # Или 'branch_b'

with DAG('branch_example',
         start_date=datetime(2023, 1, 1),
         schedule_interval='@daily',
         catchup=False) as dag:

    branching = BranchPythonOperator(
        task_id='branching_task',
        python_callable=choose_branch,
    )

    branch_a = DummyOperator(task_id='branch_a')
    branch_b = DummyOperator(task_id='branch_b')

    branching >> [branch_a, branch_b]
```

В этом примере функция `choose_branch()` определяет, какая из веток будет выполнена.

## 3. Условия выполнения

Условное выполнение задач может также зависеть от других факторов, таких как день недели или время суток. Airflow предоставляет специальные операторы для реализации таких условий:

- **BranchDayOfWeekOperator**: Выполняет задачу в зависимости от текущего дня недели.
- **BranchDateTimeOperator**: Выполняет задачу в зависимости от текущего времени.

### Пример использования BranchDayOfWeekOperator
```python
from airflow import DAG
from airflow.operators.dummy import DummyOperator
from airflow.operators.branch import BranchDayOfWeekOperator
from datetime import datetime

with DAG('day_of_week_example',
         start_date=datetime(2023, 1, 1),
         schedule_interval='@daily',
         catchup=False) as dag:

    branching = BranchDayOfWeekOperator(
        task_id='branching_task',
        week_day=6,  # Воскресенье (0 - понедельник, 6 - воскресенье)
    )

    task_weekend = DummyOperator(task_id='weekend_task')
    task_weekday = DummyOperator(task_id='weekday_task')

    branching >> [task_weekend, task_weekday]
```

## Заключение

Условное выполнение задач в Apache Airflow позволяет создавать более сложные и адаптивные рабочие процессы. Использование операторов **ShortCircuitOperator** и **BranchPythonOperator** дает возможность управлять выполнением задач на основе различных условий и логики. Это делает Airflow мощным инструментом для автоматизации процессов обработки данных и управления рабочими потоками.

Citations:
[1] https://bigdataschool.ru/blog/shortcircuitoperator-in-airflow-dag.html
[2] https://bigdataschool.ru/blog/branching-in-dag-airflow-with-operators.html
[3] https://slavlotski.com/all/apache-airflow/
[4] https://bigdataschool.ru/blog/airflow-key-concepts-from-dag-to-hook.html
[5] https://datafinder.ru/products/vse-chto-vam-nuzhno-znat-ob-airflow-dags-ch1-osnovy-i-raspisaniya
[6] https://habr.com/ru/articles/682384/
[7] https://practicum.yandex.ru/blog/apache-airflow/
[8] https://cloud.vk.com/blog/airflow-what-it-is-how-it-works