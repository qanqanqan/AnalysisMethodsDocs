Функция `runningAccumulate` в ClickHouse используется для вычисления накопительных итогов, позволяя суммировать значения по порядку в пределах набора данных. Эта функция может быть полезна для анализа временных рядов и других последовательных данных.

## Основные характеристики `runningAccumulate`

- **Накопительный итог**: `runningAccumulate` суммирует значения в указанном столбце, начиная от первой строки и до текущей. Это позволяет отслеживать, как сумма изменяется с добавлением новых данных.
  
- **Не является агрегатной функцией**: Важно отметить, что `runningAccumulate` не является агрегатной функцией и не может использоваться с `GROUP BY`. Она предназначена для работы с упорядоченными данными.

### Пример использования

Рассмотрим простой пример, где у нас есть таблица с числовыми значениями:

```sql
SELECT 
    x,
    runningAccumulate(x) AS cumulative_sum
FROM 
    (SELECT number AS x FROM numbers(10))
```

Результат будет выглядеть следующим образом:

```
┌─x─┬─cumulative_sum─┐
│ 0 │ 0              │
│ 1 │ 1              │
│ 2 │ 3              │
│ 3 │ 6              │
│ 4 │ 10             │
│ 5 │ 15             │
│ 6 │ 21             │
│ 7 │ 28             │
│ 8 │ 36             │
│ 9 │ 45             │
└───┴────────────────┘
```

### Ограничения

- **Проблемы с блоками**: При использовании `runningAccumulate` могут возникать ошибки, если данные разбиваются на блоки. Например, если данные сортируются или группируются, результаты могут быть некорректными из-за того, что функция не учитывает порядок строк между блоками[2][3].

- **Необходимость в сортировке**: Чтобы получить правильные результаты, важно правильно упорядочить данные перед применением функции. Например, если вы хотите получить накопительные итоги по дате, убедитесь, что данные отсортированы по дате[3].

### Альтернативы

Для более сложных случаев накопления последних N строк можно рассмотреть использование оконных функций (начиная с версии ClickHouse 21.3). Например:

```sql
SELECT 
    x,
    sum(x) OVER (ORDER BY x ASC ROWS BETWEEN 1 PRECEDING AND CURRENT ROW) AS rolling_sum
FROM 
    (SELECT number AS x FROM numbers(10))
```

Этот запрос будет суммировать текущее значение и предыдущее значение, что позволяет контролировать количество строк для накопления.

## Заключение

Функция `runningAccumulate` в ClickHouse является мощным инструментом для вычисления накопительных итогов, однако требует внимательного подхода к порядку данных и может быть ограничена в случаях с разбивкой на блоки. Для более сложных сценариев рекомендуется использовать оконные функции.

Citations:
[1] https://github.com/ClickHouse/ClickHouse/issues/27798
[2] https://kb.altinity.com/altinity-kb-queries-and-syntax/cumulative-unique/
[3] https://stackoverflow.com/questions/65953013/clickhouse-runningaccumulate-does-not-work-as-i-expect
[4] https://clickhouse.com/docs/en/sql-reference/data-types/aggregatefunction
[5] https://clickhouse.com/docs/en/operations/settings/query-complexity
[6] https://clickhouse.com/blog/aggregate-functions-combinators-in-clickhouse-for-arrays-maps-and-states
[7] https://altinity.com/blog/clickhouse-aggregation-fun-part-2-exploring-and-fixing-performance
[8] https://clickhouse.com/docs/en/sql-reference/aggregate-functions