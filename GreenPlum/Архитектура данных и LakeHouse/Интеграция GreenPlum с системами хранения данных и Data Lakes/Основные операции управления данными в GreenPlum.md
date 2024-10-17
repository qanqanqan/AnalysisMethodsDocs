## Основные операции управления данными в GreenPlum

В Greenplum, как и в других реляционных базах данных, операции управления данными (DML — Data Manipulation Language) используются для работы с данными в таблицах. Основные операции включают добавление, изменение, удаление и выборку данных. Вот краткий обзор этих операций:

### 1. Вставка данных (INSERT)
Операция вставки используется для добавления новых строк в таблицу.
Пример:
```sql
INSERT INTO employees (id, name, position) VALUES (1, 'John Doe', 'Engineer');
```

### 2. Обновление данных (UPDATE)
Операция обновления позволяет изменять существующие данные в таблице.
Пример:
```sql
UPDATE employees SET position = 'Senior Engineer' WHERE id = 1;
```

### 3. Удаление данных (DELETE)
Операция удаления используется для удаления строк из таблицы.
Пример:
```sql
DELETE FROM employees WHERE id = 1;
```

### 4. Выборка данных (SELECT)
Операция выборки используется для извлечения данных из таблицы. Она может включать фильтрацию, сортировку и агрегацию данных.
Пример:
```sql
SELECT * FROM employees WHERE position = 'Engineer' ORDER BY name;
```

### 5. Использование условий (WHERE)
Операции вставки, обновления и удаления могут содержать условия, позволяющие выбирать конкретные строки для обработки. Это с помощью оператора WHERE.
Пример (DELETE с условием):
```sql
DELETE FROM employees WHERE position = 'Intern';
```

### 6. Транзакции
Для обеспечения целостности данных и управления несколькими операциями одновременно, Greenplum поддерживает транзакции с использованием операторов BEGIN, COMMIT и ROLLBACK.
Пример:
```sql
BEGIN;
UPDATE employees SET position = 'Manager' WHERE id = 2;
DELETE FROM employees WHERE id = 3;
COMMIT;
```

### 7. Использование агрегатных функций
Операция SELECT может использовать агрегатные функции для вычисления обобщенных значений, таких как COUNT, SUM, AVG, MAX и MIN.
Пример:
```sql
SELECT COUNT(*) AS total_employees, AVG(salary) AS average_salary FROM employees;
```

### 8. Группировка данных (GROUP BY)
Оператор GROUP BY используется для агрегации данных по определенным полям.
Пример:
```sql
SELECT position, COUNT(*) AS count FROM employees GROUP BY position;
```

### 9. Соединение таблиц (JOIN)
Greenplum поддерживает различные типы соединений для объединения данных из нескольких таблиц.
Пример:
```sql
SELECT e.name, d.department_name
FROM employees e
JOIN departments d ON e.department_id = d.id;
```
