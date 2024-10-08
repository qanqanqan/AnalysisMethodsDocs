Какие очереди существуют в YARN и как их можно настроить?
В YARN используются очереди для управления и распределения ресурсов между различными пользователями и задачами (приложениями). В зависимости от выбранного планировщика (scheduler) YARN, очередь может иметь разные параметры и методы настройки. Основные типы планировщиков в YARN, которые поддерживают очереди:

1. **Capacity Scheduler (Планировщик по емкости)**
2. **Fair Scheduler (Честный планировщик)**

Каждый из них предоставляет возможность создавать очереди с различными политиками для распределения ресурсов. Давайте рассмотрим основные типы очередей и их настройки для каждого планировщика.

### 1. **Capacity Scheduler (Планировщик по емкости)**

**Capacity Scheduler** позволяет разделить ресурсы кластера между несколькими очередями, при этом каждая очередь получает заранее заданную долю ресурсов. Это полезно в кластерах с разными группами пользователей, где нужно гарантировать определённое количество ресурсов для каждой группы.

#### Основные типы очередей в Capacity Scheduler:
- **Root Queue (Корневая очередь):** это базовая очередь, в которой могут быть созданы дочерние очереди. Все остальные очереди находятся в её поддереве.
- **Leaf Queue (Листовая очередь):** это конечные очереди, которые могут принимать задачи. В них и происходят вычисления. Листовые очереди не могут иметь дочерних очередей.

#### Настройка очередей:

Для настройки очередей в Capacity Scheduler используется файл конфигурации `capacity-scheduler.xml`.

##### Пример настройки очередей:

```xml
<property>
  <name>yarn.scheduler.capacity.root.queues</name>
  <value>queue1,queue2</value>
</property>

<!-- Настройка первой очереди -->
<property>
  <name>yarn.scheduler.capacity.root.queue1.capacity</name>
  <value>60</value> <!-- Очередь получит 60% ресурсов кластера -->
</property>

<property>
  <name>yarn.scheduler.capacity.root.queue1.maximum-capacity</name>
  <value>100</value> <!-- Максимальная емкость очереди - 100% -->
</property>

<property>
  <name>yarn.scheduler.capacity.root.queue1.state</name>
  <value>RUNNING</value> <!-- Очередь активна -->
</property>

<!-- Настройка второй очереди -->
<property>
  <name>yarn.scheduler.capacity.root.queue2.capacity</name>
  <value>40</value> <!-- Очередь получит 40% ресурсов кластера -->
</property>

<property>
  <name>yarn.scheduler.capacity.root.queue2.maximum-capacity</name>
  <value>50</value> <!-- Максимальная емкость очереди - 50% -->
</property>

<property>
  <name>yarn.scheduler.capacity.root.queue2.state</name>
  <value>RUNNING</value>
</property>
```

#### Основные параметры для настройки:
- **capacity:** Определяет процент ресурсов, выделенных для очереди. Например, `60` означает, что очередь может использовать до 60% ресурсов кластера.
- **maximum-capacity:** Максимальный процент ресурсов, который может использовать очередь, если другие очереди не используют свои ресурсы.
- **state:** Определяет текущее состояние очереди (`RUNNING` или `STOPPED`).
- **user-limit-factor:** Ограничение на количество ресурсов, которые может использовать один пользователь в пределах очереди.

#### Подочереди:
В Capacity Scheduler возможна иерархия очередей с подочередями:

```xml
<property>
  <name>yarn.scheduler.capacity.root.queues</name>
  <value>queue1,queue2</value>
</property>

<property>
  <name>yarn.scheduler.capacity.root.queue1.queues</name>
  <value>subqueue1,subqueue2</value> <!-- Подочереди для queue1 -->
</property>

<property>
  <name>yarn.scheduler.capacity.root.queue1.subqueue1.capacity</name>
  <value>30</value> <!-- 30% от ресурсов, выделенных для queue1 -->
</property>

<property>
  <name>yarn.scheduler.capacity.root.queue1.subqueue2.capacity</name>
  <value>70</value> <!-- 70% от ресурсов, выделенных для queue1 -->
</property>
```

### 2. **Fair Scheduler (Честный планировщик)**

**Fair Scheduler** распределяет ресурсы кластера поровну между всеми активными очередями и задачами, что позволяет обеспечить равномерное распределение ресурсов при отсутствии строгих ограничений. Этот планировщик может настраиваться для предоставления большего количества ресурсов задачам с более высоким приоритетом или важностью.

#### Основные типы очередей в Fair Scheduler:
- **Пул (Pool):** Аналог очереди. Каждый пул может иметь настройки, такие как минимальные и максимальные ресурсы, а также приоритеты для приложений.

#### Настройка очередей:

Для настройки очередей в Fair Scheduler используется файл `fair-scheduler.xml`.

##### Пример настройки очередей:

```xml
<allocations>
  <!-- Определение пула -->
  <pool name="pool1">
    <minResources>1024 mb, 1 vcores</minResources> <!-- Минимальное количество ресурсов -->
    <maxResources>4096 mb, 4 vcores</maxResources> <!-- Максимальное количество ресурсов -->
    <weight>1.0</weight> <!-- Вес пула для определения приоритета -->
    <schedulingPolicy>fair</schedulingPolicy> <!-- Политика планирования -->
  </pool>

  <pool name="pool2">
    <minResources>512 mb, 1 vcores</minResources>
    <maxResources>2048 mb, 2 vcores</maxResources>
    <weight>0.5</weight>
    <schedulingPolicy>fair</schedulingPolicy>
  </pool>
</allocations>
```

#### Основные параметры для настройки:
- **minResources:** Минимальное количество ресурсов, которое будет выделено пулу (очереди), даже если кластер сильно загружен.
- **maxResources:** Максимальное количество ресурсов, которое может быть выделено пулу при отсутствии конкуренции за ресурсы.
- **weight:** Определяет приоритет пула относительно других пулов. Пулы с большим весом будут получать больше ресурсов при равных условиях.
- **schedulingPolicy:** Политика распределения ресурсов, может быть `fair` (равномерное распределение) или `fifo` (очередь по принципу "первый пришел — первый обслужен").

### Заключение:

Очереди в YARN предоставляют гибкие механизмы для управления и распределения ресурсов кластера между пользователями и задачами. В зависимости от нужд можно выбрать один из двух основных типов планировщиков (Capacity или Fair), каждый из которых поддерживает настройку очередей с различными характеристиками.