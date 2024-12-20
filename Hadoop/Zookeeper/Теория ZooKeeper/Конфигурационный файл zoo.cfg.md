## Конфигурационный файл zoo.cfg

Конфигурационный файл `zoo.cfg` является ключевым компонентом Apache ZooKeeper, который определяет настройки сервера ZooKeeper. Этот файл содержит параметры конфигурации, необходимые для запуска и работы ZooKeeper. Вот основные параметры, которые обычно включаются в `zoo.cfg`:

### Основные параметры конфигурации

1. **`tickTime`**:
   - **Описание**: Определяет базовый временной интервал в миллисекундах, используемый для синхронизации между серверами ZooKeeper и клиентами. Например, heartbeat между сервером и клиентом.
   - **Пример**: `tickTime=2000`

2. **`dataDir`**:
   - **Описание**: Указывает путь к каталогу, где ZooKeeper будет хранить свои данные и логи транзакций. Этот каталог должен быть доступен для записи.
   - **Пример**: `dataDir=/var/lib/zookeeper`

3. **`clientPort`**:
   - **Описание**: Порт, на котором ZooKeeper будет слушать клиентские подключения. Клиенты подключаются к этому порту для взаимодействия с сервером ZooKeeper.
   - **Пример**: `clientPort=2181`

4. **`initLimit`**:
   - **Описание**: Максимальное количество `tickTime`, которое серверы ZooKeeper могут использовать для инициализации соединения с лидером.
   - **Пример**: `initLimit=10`

5. **`syncLimit`**:
   - **Описание**: Максимальное количество `tickTime`, которое серверы ZooKeeper могут использовать для синхронизации с лидером.
   - **Пример**: `syncLimit=5`

6. **`server.X`**:
   - **Описание**: Определяет конфигурацию для каждого сервера в кластере ZooKeeper, где `X` — это уникальный идентификатор сервера. Формат: `server.X=hostname:peerPort:leaderElectionPort`.
   - **Пример**: `server.1=zookeeper1:2888:3888`

### Дополнительные параметры

- **`dataLogDir`**:
  - **Описание**: Указывает отдельный каталог для хранения логов транзакций. Это может улучшить производительность, если логи и данные хранятся на разных дисках.
  - **Пример**: `dataLogDir=/var/log/zookeeper`

- **`maxClientCnxns`**:
  - **Описание**: Максимальное количество клиентских соединений, которые может установить один клиентский IP-адрес.
  - **Пример**: `maxClientCnxns=60`

### Пример файла `zoo.cfg`

```plaintext
tickTime=2000
dataDir=/var/lib/zookeeper
clientPort=2181
initLimit=10
syncLimit=5
server.1=zookeeper1:2888:3888
server.2=zookeeper2:2888:3888
server.3=zookeeper3:2888:3888
```

Этот файл конфигурации должен быть одинаковым для всех серверов в кластере ZooKeeper, за исключением строки `server.X`, которая должна быть уникальной для каждого сервера. `zoo.cfg` играет важную роль в определении поведения и структуры кластера ZooKeeper.