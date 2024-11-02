Установка ClickHouse может отличаться в зависимости от используемой операционной системы. Рассмотрим процесс установки ClickHouse на популярных операционных системах: Ubuntu (Debian), CentOS (Red Hat) и macOS. Также упомянем установку в Docker.

### Установка ClickHouse на Ubuntu (Debian)

1. Обновите пакеты и установите необходимые зависимости:
```sh
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates
```

2. Добавьте репозиторий ClickHouse:
```sh
wget -qO - https://clickhouse.com/keys/GPG-KEY-clickhouse | sudo apt-key add -
echo "deb https://repo.clickhouse.com/deb/stable/main/binary/ubuntu/ bionic main" | sudo tee /etc/apt/sources.list.d/clickhouse.list
```

3. Установите ClickHouse:
```sh
sudo apt-get update
sudo apt-get install -y clickhouse-server clickhouse-client
```

4. Запустите ClickHouse:
```sh
sudo service clickhouse-server start
```

5. Проверьте, работает ли сервер:
```sh
clickhouse-client
```

### Установка ClickHouse на CentOS (Red Hat)

1. Установите необходимые зависимости:
```sh
sudo yum install -y epel-release
sudo yum install -y yum-utils
```

2. Добавьте репозиторий ClickHouse:
```sh
sudo tee /etc/yum.repos.d/clickhouse.repo <<EOF
   [clickhouse]
   name=ClickHouse
   baseurl=https://repo.clickhouse.com/rpm/stable/x86_64/
   gpgcheck=1
   enabled=1
   gpgkey=https://clickhouse.com/keys/GPG-KEY-clickhouse
   EOF
```

3. Установите ClickHouse:
```sh
sudo yum install -y clickhouse-server clickhouse-client
```

4. Запустите ClickHouse:
```sh
sudo systemctl start clickhouse-server
sudo systemctl enable clickhouse-server
```

5. Проверьте, работает ли сервер:
```sh
clickhouse-client
```

### Установка ClickHouse на macOS

1. Используйте Homebrew для установки ClickHouse:
```sh
brew install clickhouse-client
brew install clickhouse-server
```

2. Запустите ClickHouse (с использованием Homebrew Services):
```sh
brew services start clickhouse
```

3. Проверьте, работает ли сервер:
```sh
clickhouse-client
```

### Установка ClickHouse в Docker

1. Выберите образ ClickHouse и запустите контейнер:
```sh
docker run -d --name clickhouse-server --ulimit nofile=262144:262144 -p 8123:8123 -p 9000:9000 yandex/clickhouse-server
```

2. Подключитесь к ClickHouse клиенту:
```sh
docker exec -it clickhouse-server clickhouse-client
```

После завершения установки ClickHouse рекомендуется выполнить базовые настройки, такие как изменение настроек конфигурации в файле /etc/clickhouse-server/config.xml и других, в зависимости от ваших требований. Также рекомендуется ознакомиться с документацией ClickHouse для настройки системы и оптимизации производительности в зависимости от ваших сценариев использования.
