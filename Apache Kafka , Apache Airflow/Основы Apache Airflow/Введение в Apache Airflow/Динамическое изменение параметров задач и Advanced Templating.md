Динамическое изменение параметров задач и использование **Advanced Templating** в Apache Airflow позволяют создавать гибкие и масштабируемые рабочие процессы. Эти функции обеспечивают возможность адаптации выполнения задач в зависимости от различных условий и контекстов.

## Динамическое изменение параметров задач

### Использование переменных и подключений

Airflow предоставляет доступ к переменным и подключениям, что позволяет динамически настраивать параметры задач. Например, переменные можно использовать с помощью следующего синтаксиса:

- Для обычных переменных: `{{ var.value.variable_name }}`
- Для JSON-переменных: `{{ var.json.variable_name }}`
- Для подключения: `{{ conn.conn_id }}`

Эти конструкции позволяют извлекать значения из конфигурации Airflow и использовать их в задачах, что делает их более адаптивными к изменениям в окружении или данных[1].

### Jinja Templating

Airflow использует Jinja для шаблонизации, что позволяет включать динамический контент в задачи. Например, можно передать текущее время выполнения задачи:

```python
execution_date = '{{ ds }}'
```

Это позволяет задаче использовать дату выполнения в своих операциях, например, для создания директорий или обработки данных за определенный день[2][5].

### Обработка неопределенных переменных

В Airflow 2.0 и выше, если переменная не определена, задача завершится ошибкой. Это позволяет лучше управлять ошибками. Однако можно использовать фильтр `| default` для задания значений по умолчанию:

```python
{{ my_var | default('default_value') }}
```

Это полезно для обеспечения стабильности выполнения задач даже при отсутствии некоторых переменных[1].

## Advanced Templating

### Настройка полей шаблонов

Airflow позволяет настраивать поля, которые могут быть шаблонизированы через атрибут `template_fields`. Это позволяет разработчикам определять, какие параметры задачи могут использовать Jinja-шаблоны. Например, можно задать, что поле `bash_command` будет принимать шаблоны:

```python
bash_task.template_fields = ("bash_command", "env", "cwd")
```

Это дает возможность динамически изменять команды Bash в зависимости от контекста выполнения[5].

### Пользовательские макросы и фильтры

Разработчики могут создавать пользовательские макросы и фильтры для расширения возможностей шаблонизации. Это позволяет добавлять специфическую логику или форматирование, которое может быть использовано в шаблонах.

Например, можно определить макросы для сложных вычислений или преобразований данных перед их использованием в задачах[1][2].

### Рендеринг шаблонов

Airflow поддерживает рендеринг шаблонов как строк, так и объектов Python. Это позволяет более гибко управлять данными внутри задач. Например, можно настроить рендеринг так, чтобы возвращались нативные объекты Python:

```python
render_template_as_native_obj=True
```

Это особенно полезно, когда необходимо работать с результатами выполнения шаблонов как с объектами Python[5].

Таким образом, динамическое изменение параметров задач и использование продвинутой шаблонизации в Apache Airflow значительно увеличивают гибкость и мощность рабочих процессов, позволяя адаптировать их к различным условиям и требованиям.

Citations:
[1] https://www.restack.io/docs/airflow-knowledge-apache-templates-fields
[2] https://github.com/astronomer/airflow-guides/blob/main/guides/templating.md
[3] https://airflow.apache.org/docs/apache-airflow/1.10.12/tutorial.html
[4] https://www.astronomer.io/docs/learn/dags
[5] https://www.astronomer.io/docs/learn/templating
[6] https://github.com/damavis/advanced-airflow
[7] https://marclamberti.com/blog/templates-macros-apache-airflow/
[8] https://airflow.apache.org/docs/apache-airflow/2.3.2/concepts/dags.html