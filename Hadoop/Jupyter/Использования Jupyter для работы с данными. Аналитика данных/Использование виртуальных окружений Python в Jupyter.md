## Использование виртуальных окружений Python в Jupyter

Использование виртуальных окружений Python в Jupyter Notebook — это хорошая практика, позволяющая управлять зависимостями и изолировать проекты друг от друга. С помощью виртуальных окружений вы можете создавать специальные среды, в которых будут установлены только необходимые пакеты. Давайте рассмотрим, как создать и использовать виртуальные окружения в Jupyter Notebook.

### 1. Создание виртуального окружения

Сначала необходимо создать виртуальное окружение. Для этого можно использовать менеджеры пакетов, такие как venv, virtualenv или conda. Вот как это сделать с помощью `venv`:

1. Установите Python: Убедитесь, что Python установлен на вашем компьютере. Это можно проверить, выполнив команду `python --version` или `python3 --version` в терминале.

2. Создайте виртуальное окружение:
- Перейдите в директорию, где вы хотите создать окружение:

```sh
     cd путь/к/вашему/проекту
```

- Создайте окружение:

```sh
     python -m venv myenv
```

Здесь myenv — это имя вашего окружения. Вы можете выбрать любое другое имя.

3. Активируйте виртуальное окружение:
- На Windows:

```sh
     myenv\Scripts\activate
```

- На macOS/Linux:

```sh
     source myenv/bin/activate
```

### 2. Установка необходимых пакетов

После активации вашего виртуального окружения, вы можете установить необходимые пакеты, включая Jupyter Notebook:

```sh
pip install jupyter
```

### 3. Добавление виртуального окружения в Jupyter

Теперь, чтобы сделать ваше новое виртуальное окружение доступным в Jupyter Notebook, выполните следующие команды:

1. Установите пакет `ipykernel`, который позволяет использовать ваше окружение в Jupyter:

```sh
pip install ipykernel
```

2. Зарегистрируйте виртуальное окружение в Jupyter как ядро:

```sh
python -m ipykernel install --user --name=myenv --display-name "Python (myenv)"
```

- `--name=myenv` — это имя вашего окружения.
- `--display-name "Python (myenv)"` — это имя, которое будет отображаться в интерфейсе Jupyter (вы можете задать любое другое имя).

### 4. Запуск Jupyter Notebook

Теперь вы можете запустить Jupyter Notebook:

```sh
jupyter notebook
```

В интерфейсе Jupyter Notebook вы сможете выбрать ваше виртуальное окружение в качестве ядра:

1. Создайте новый блокнот или откройте существующий.
2. Перейдите в меню `Kernel → Change kernel`.
3. Выберите `"Python (myenv)"` из списка доступных ядер.

### 5. Установка дополнительных пакетов в виртуальном окружении

Если вам нужно установить дополнительные пакеты, просто активируйте ваше виртуальное окружение и используйте `pip` для установки:

```sh
pip install имя_пакета
```

### 6. Деактивация виртуального окружения

Когда вы закончите работу, вы можете деактивировать виртуальное окружение, выполнив команду:

```sh
deactivate
```