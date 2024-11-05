В Apache Airflow механизм **XCom** (Cross-Communication) используется для передачи данных между задачами в рамках одного DAG. Этот механизм позволяет задачам обмениваться небольшими фрагментами данных, что особенно полезно в сценариях ETL и других рабочих процессах. Рассмотрим подробнее, как работает XCom, его основные функции и ограничения.

## Основные функции XCom

### 1. Передача данных

XCom позволяет передавать данные между задачами с помощью методов `xcom_push` и `xcom_pull`. Задача может "пушить" данные в XCom, а другая задача может "пулить" эти данные для дальнейшего использования:

- **Пуш данных**: Если задача возвращает значение, оно автоматически записывается в XCom. Это поведение можно изменить, установив параметр `do_xcom_push=False` для оператора.
  
  Пример кода:
  ```python
  from airflow.operators.python import PythonOperator

  def return_value():
      return "Hello from task!"

  push_task = PythonOperator(
      task_id='push_task',
      python_callable=return_value,
  )
  ```

- **Пул данных**: Для получения данных из XCom используется метод `xcom_pull`, который позволяет указать идентификатор задачи и ключ для извлечения нужного значения.

  Пример кода:
  ```python
  pulled_value = task_instance.xcom_pull(task_ids='push_task', key='return_value')
  ```

### 2. Хранение данных

XCom хранит данные в базе данных метаданных Airflow. Каждая запись имеет следующие атрибуты:
- `key`: ключ записи.
- `value`: значение, хранящееся в двоичном формате.
- `timestamp`: время создания записи.
- `execution_date`: дата выполнения DAG.
- `task_id` и `dag_id`: идентификаторы задачи и DAG соответственно.

### 3. Ограничения

Несмотря на свою полезность, XCom имеет ограничения:
- **Размер данных**: XCom предназначен для передачи небольших объемов данных (например, строк или чисел). Рекомендуется избегать передачи больших объектов, таких как DataFrame из pandas или больших файлов. Для передачи больших объемов данных лучше использовать внешние хранилища (например, S3) и передавать только ссылки на них через XCom.
  
- **Производительность**: Поскольку данные хранятся в базе данных метаданных, большое количество записей XCom может повлиять на производительность системы.

## Примеры использования

### Пример передачи данных между задачами

```python
from airflow import DAG
from airflow.operators.python import PythonOperator
from datetime import datetime

def push_data(**kwargs):
    return "data to share"

def pull_data(**kwargs):
    ti = kwargs['ti']
    shared_data = ti.xcom_pull(task_ids='push_task')
    print(f"Received data: {shared_data}")

with DAG(dag_id='example_xcom',
         schedule_interval='@daily',
         start_date=datetime(2023, 1, 1),
         catchup=False) as dag:

    push_task = PythonOperator(
        task_id='push_task',
        python_callable=push_data,
        provide_context=True,
    )

    pull_task = PythonOperator(
        task_id='pull_task',
        python_callable=pull_data,
        provide_context=True,
    )

    push_task >> pull_task
```

В этом примере первая задача (`push_task`) передает строку во вторую задачу (`pull_task`), которая извлекает это значение из XCom.

## Заключение

XCom является мощным инструментом для обмена данными между задачами в Apache Airflow. Он позволяет эффективно управлять зависимостями и передавать результаты выполнения задач без необходимости использования внешних систем. Однако важно помнить о его ограничениях и избегать передачи больших объемов данных через этот механизм.

Citations:
[1] https://khashtamov.com/ru/apache-airflow-xcom/
[2] https://bigdataschool.ru/blog/airflow-xcom-under-the-hood.html
[3] https://forum.goodt.me/t/apache-airflow-osnovnye-razdely/402
[4] https://www.restack.io/docs/airflow-knowledge-xcom-airflow-guide
[5] https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/xcoms.html
[6] https://datafinder.ru/products/apache-nifi-vs-airflow-obzor-i-sravnenie
[7] https://marclamberti.com/blog/airflow-xcom/
[8] https://habr.com/ru/companies/alfa/articles/676926/