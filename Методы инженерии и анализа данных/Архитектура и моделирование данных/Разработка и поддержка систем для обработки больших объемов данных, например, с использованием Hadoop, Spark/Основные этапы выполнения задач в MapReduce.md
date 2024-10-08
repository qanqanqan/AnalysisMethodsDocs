### Основные этапы выполнения задач в MapReduce

MapReduce — это программная модель для обработки больших объёмов данных в распределённой среде. Основная концепция заключается в разделении задачи на два ключевых этапа: **Map** и **Reduce**, которые выполняются параллельно на множестве узлов кластера. Однако весь процесс выполнения задачи в MapReduce включает несколько шагов, каждый из которых играет важную роль. Рассмотрим основные этапы выполнения задач в MapReduce.

#### 1. **Инициализация задания**

Процесс выполнения задачи MapReduce начинается с того, что клиентское приложение (например, Java-программа) отправляет запрос на выполнение задания (job) в кластер Hadoop. Для этого клиент вызывает **JobTracker** (в старой версии Hadoop) или **ResourceManager** в YARN (в новой версии Hadoop).

**Шаги:**
- Клиент определяет задание (job), включая файлы ввода, выходные директории, классы Mapper и Reducer, а также другие параметры.
- Клиент отправляет запрос на выполнение задания в кластер.

#### 2. **Разбиение данных (Input Splits)**

Когда задание принято, данные, которые нужно обработать, разбиваются на **сплиты** (части) с целью разделения входного файла на более мелкие части, каждая из которых может быть обработана отдельным узлом параллельно. Обычно размер одного сплита равен размеру блока HDFS (обычно 128 или 256 МБ).

**Шаги:**
- Входные данные разбиваются на части (input splits), каждая из которых будет обработана отдельной задачей Map.
- Разделение зависит от формата данных (например, текстовые или бинарные файлы).

#### 3. **Этап Map (Отображение)**

На этапе Map для каждой части входных данных запускается функция **Mapper**. Она принимает входные данные в виде ключей и значений, а затем выполняет обработку, возвращая набор промежуточных данных, представленных также в виде пар ключ-значение.

**Шаги:**
- Для каждой части данных запускается отдельная задача **Map Task**, которая выполняет обработку входных данных.
- Mapper преобразует входные ключи и значения во множество промежуточных ключей и значений.
- Пример: если задание подсчёта слов, на этапе Map каждое слово в тексте преобразуется в пару («слово», 1).

#### 4. **Комбинирование (Optional: Combiner)**

**Combiner** — это необязательный этап, который выполняется для минимизации объема данных перед передачей на следующий этап. Он используется для предварительной агрегации данных на локальном уровне (на каждом узле). Комбайнер работает как мини-Reducer, сокращая количество данных, отправляемых по сети.

**Шаги:**
- Combiner агрегирует данные на уровне каждого узла перед отправкой данных на этап Shuffle и Reduce.
- Пример: для подсчёта слов Combiner может суммировать промежуточные значения для каждого слова на каждом узле, чтобы сократить объем данных, передаваемых в сеть.

#### 5. **Перемешивание и сортировка (Shuffle and Sort)**

После того как этап Map завершён, промежуточные результаты сортируются и передаются на узлы для этапа Reduce. Этот процесс называется **Shuffle and Sort**. Он включает перемещение данных по сети так, чтобы все значения с одинаковыми ключами попали на один узел.

**Шаги:**
- Промежуточные данные сортируются по ключам.
- Данные передаются (перемешиваются) на узлы Reducer таким образом, чтобы данные с одинаковыми ключами попали на один узел для дальнейшей обработки.

#### 6. **Этап Reduce (Свертка)**

На этапе Reduce функция **Reducer** принимает сгруппированные промежуточные данные (с одинаковыми ключами) и выполняет финальную обработку, агрегируя значения для каждого ключа. Результаты затем записываются в выходные файлы.

**Шаги:**
- Reducer принимает промежуточные ключи и соответствующие им значения, сгенерированные на этапе Map и отсортированные на этапе Shuffle.
- Reducer сводит (агрегирует) значения для каждого ключа, производя окончательные результаты.
- Пример: в задаче подсчёта слов Reducer суммирует все вхождения для каждого слова.

#### 7. **Запись результатов (Output)**

После завершения этапа Reduce результаты записываются в выходную директорию, обычно на HDFS или другую распределённую файловую систему. Эти данные могут быть использованы для дальнейшей обработки или анализа.

**Шаги:**
- Результаты работы Reducer записываются в выходные файлы на HDFS.
- Каждый Reducer пишет свой результат в отдельный файл, и эти файлы можно собрать в один полный результат выполнения задачи.

#### 8. **Завершение задания (Job Completion)**

Когда все этапы Map и Reduce завершены, система отправляет уведомление клиенту о том, что задача успешно завершена. Клиент может получить доступ к выходным данным для дальнейшей обработки или анализа.

### Пример работы MapReduce

Для лучшего понимания рассмотрим классический пример **подсчёта слов (Word Count)**.

1. **Инициализация задания**: клиентская программа отправляет запрос на выполнение задачи подсчёта слов.
2. **Разбиение данных**: входной файл (например, текстовый документ) разбивается на части по 128 МБ.
3. **Этап Map**: каждая строка обрабатывается и для каждого слова генерируется пара («слово», 1).
4. **Combiner (необязательно)**: на каждом узле Combiner предварительно суммирует количество слов, чтобы минимизировать данные, передаваемые по сети.
5. **Перемешивание и сортировка**: данные с одинаковыми словами группируются и перемещаются на узлы Reduce.
6. **Этап Reduce**: для каждого слова данные суммируются, и конечный результат — количество вхождений каждого слова — записывается в файл.
7. **Запись результата**: результаты записываются в выходную директорию на HDFS.
8. **Завершение задания**: клиент получает уведомление о завершении задания и может получить доступ к результатам.

### Заключение

Процесс выполнения задач в MapReduce включает несколько этапов, начиная с инициализации задания и заканчивая записью результатов. Ключевыми этапами являются Map и Reduce, которые позволяют обрабатывать данные параллельно на множестве узлов. Благодаря распределённой архитектуре и гибкости MapReduce стал важным инструментом для обработки больших данных в таких системах, как Hadoop.