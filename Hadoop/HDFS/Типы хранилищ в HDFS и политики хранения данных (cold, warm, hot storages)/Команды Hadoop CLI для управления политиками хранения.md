# Команды Hadoop CLI для управления политиками хранения

Hadoop CLI предоставляет ряд команд, которые позволяют настраивать и изменять параметры управления политиками хранения данных в HDFS.

## Основные команды Hadoop CLI для управления политиками хранения

### 1. Изменение уровня репликации

Для изменения уровня репликации файлов в HDFS используйте команду  `setrep`:

```bash
hdfs dfs -setrep -w <replication_factor> <path>
```

**Пример**:  `bash hdfs dfs -setrep -w 3 /user/hadoop/myfile.txt`  Эта команда изменит уровень репликации файла на 3.

### 2. Просмотр текущего уровня репликации

Чтобы просмотреть текущий уровень репликации файла, используйте команду  `-ls`:

```bash
hdfs dfs -ls <path>
```

**Пример**:  `bash hdfs dfs -ls /user/hadoop/`

### 3. Перемещение или копирование данных

Вы можете перемещать или копировать данные между различными хранилищами с помощью команд  `-mv`  и  `-cp`:

**Перемещение**:  `bash hdfs dfs -mv <source_path> <destination_path>`

**Копирование**:  `bash hdfs dfs -cp <source_path> <destination_path>`

**Пример**:  `bash hdfs dfs -mv /user/hadoop/myfile.txt /user/hadoop/ssd_storage/myfile.txt`

### 4. Создание директории

Для создания новой директории, которая может использоваться для хранения данных с новым уровнем репликации или на новом типе хранилища, используйте команду  `-mkdir`:

```bash
hdfs dfs -mkdir <path>
```

**Пример**:  `bash hdfs dfs -mkdir /user/hadoop/ssd_storage`

### 5. Удаление данных

Чтобы удалить файлы или директории в HDFS, используйте команду  `-rm`  или  `-rm -r`  для удаления рекурсивно:

```bash
hdfs dfs -rm <path>
hdfs dfs -rm -r <directory_path>
```

**Пример**:  `bash hdfs dfs -rm /user/hadoop/old_file.txt hdfs dfs -rm -r /user/hadoop/old_directory`

### 6. Изменение политики хранения

Чтобы изменить политику хранения для файлов, вы можете использовать команды для установки атрибутов, если у вас установлены настройки для разных классов хранилищ. Например, Для установки политики хранения для определенного пути в HDFS используется одна из следующих команд:

```bash
hdfs dfs -setStoragePolicy <storage_policy> <path>
или
hdfs storagepolicies -setStoragePolicy -path <path> -policy <policy>
```

Где `<path>` — это путь к файлу или директории, а `<storage_policy> или <policy>` — имя политики хранения, которую вы хотите применить

**Пример**:  `bash hdfs dfs -setStoragePolicy -path /user/hadoop/mydata -policy ALL_SSD`

### 7. Просмотр политик хранения

Чтобы просмотреть какая политика хранения применяется для конкретного файла или директории, используется команда `-getStoragePolicy`:

```bash
hdfs dfs -getStoragePolicy <path>
или
hdfs storagepolicies -getStoragePolicy -path <path>
```

Эта команда вернет текущую политику для указанного пути.

**Пример**:  `bash hdfs dfs -getStoragePolicy /user/hadoop/mydata`

### 8. **Удаление политики хранения**  

Если необходимо удалить политику хранения с файла или директории, используется команда:

- `hdfs storagepolicies -unsetStoragePolicy -path <path>` 

Эта команда уберет политику и вернет файл или директорию к состоянию по умолчанию.

### 9. **Синхронизация блоков с новой политикой**  

После изменения политики хранения можно выполнить команду для перемещения блоков данных в соответствии с новой политикой:

- `hdfs storagepolicies -satisfyStoragePolicy -path <path>` 

Эта команда инициирует процесс перемещения данных в соответствии с установленной политикой.

### 10. **Просмотр доступных политик хранения**  

Чтобы получить список всех доступных политик хранения, используется команда:

`hdfs storagepolicies -listPolicies` 

Эта команда выведет все предустановленные политики, такие как HOT, WARM и COLD.

## Примеры предустановленных политик

HDFS предлагает несколько предустановленных политик хранения:

-   **HOT**: Все реплики хранятся на DISK.
-   **WARM**: Первая реплика хранится на DISK, остальные — на ARCHIVE.
-   **COLD**: Все реплики хранятся на ARCHIVE.

## Заключение

Эти команды и политики Hadoop CLI являются основными инструментами для эффективного управления данными в HDFS и обеспечивают их правильное хранение в зависимости от потребностей бизнеса. Они позволяют контролировать уровень репликации, перемещать данные между различными хранилищами и устанавливать политики хранения в зависимости от ваших нужд.