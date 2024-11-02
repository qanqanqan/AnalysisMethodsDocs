ClickHouse предлагает широкий спектр агрегационных функций, которые позволяют эффективно обрабатывать и анализировать данные. Вот основные виды агрегационных функций, доступных в ClickHouse:

## Стандартные агрегационные функции

1. **Счет**:
   - `count()`: Подсчитывает количество строк.
   - `count(column)`: Подсчитывает количество ненулевых значений в указанном столбце.

2. **Минимум и максимум**:
   - `min(column)`: Возвращает минимальное значение в столбце.
   - `max(column)`: Возвращает максимальное значение в столбце.

3. **Сумма и среднее**:
   - `sum(column)`: Суммирует значения в столбце.
   - `avg(column)`: Вычисляет среднее значение.

4. **Статистические функции**:
   - `stddevPop(column)`: Вычисляет стандартное отклонение для всей популяции.
   - `varPop(column)`: Вычисляет дисперсию для всей популяции.

## Специальные агрегационные функции

1. **Уникальные значения**:
   - `uniq(column)`: Возвращает оценочное количество уникальных значений.
   - `uniqExact(column)`: Возвращает точное количество уникальных значений.

2. **Функции для работы с массивами**:
   - `groupArray(column)`: Создает массив из значений указанного столбца.
   - `arrayJoin(array)`: Разворачивает массив в строки.

3. **Первые и последние значения**:
   - `first_value(column)`: Возвращает первое значение в группе.
   - `last_value(column)`: Возвращает последнее значение в группе.

4. **Комбинаторы**:
   - Комбинаторы, такие как `sumIf()`, позволяют выполнять агрегации с условиями. Например, `sumIf(total_amount, status = 'confirmed')` суммирует только те значения, которые соответствуют определенному условию.

## Параметрические функции

ClickHouse также поддерживает параметрические агрегатные функции, которые принимают дополнительные параметры помимо столбцов. Например, можно использовать функцию для вычисления квантилей:

```sql
quantiles(0.5, 0.9)(column)
```

## Обработка NULL

При агрегации значения NULL игнорируются, если не указано иное. Например, функции `first_value` и `last_value` могут быть использованы с модификатором `RESPECT NULLS`, чтобы учитывать значения NULL:

```sql
first_value(column) RESPECT NULLS
```

## Примеры использования

### Пример агрегации с группировкой

```sql
SELECT id, sum(amount), avg(amount)
FROM transactions
GROUP BY id;
```

### Пример использования комбинатора

```sql
SELECT sumIf(amount, status = 'confirmed') AS confirmed_sum
FROM payments;
```

ClickHouse предлагает мощные инструменты для агрегации данных, что делает его подходящим для аналитических задач и обработки больших объемов информации [1][2][4].

Citations:
[1] https://clickhouse.com/docs/en/sql-reference/aggregate-functions
[2] https://clickhouse.com/docs/en/sql-reference/data-types/aggregatefunction
[3] https://clickhouse.com/blog/aggregate-functions-combinators-in-clickhouse-for-arrays-maps-and-states
[4] https://clickhouse.com/docs/en/sql-reference/aggregate-functions/reference
[5] https://altinity.com/blog/clickhouse-aggregation-fun-part-2-exploring-and-fixing-performance
[6] https://altinity.com/blog/harnessing-the-power-of-clickhouse-arrays-part-2
[7] https://clickhouse.com/docs/en/operations/settings/query-complexity
[8] https://clickhouse.com/docs/en/sql-reference/aggregate-functions/combinators