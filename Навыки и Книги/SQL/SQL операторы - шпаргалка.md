# SQL операторы - шпаргалка

> Шпаргалка по основным SQL-операторам и конструкциям. Набор операторов может отличаться между PostgreSQL, MySQL, MS SQL Server, Oracle и SQLite, поэтому в таблице есть колонка с примечаниями.

## Арифметические операторы

| Оператор | Назначение | Пример | Примечание |
| --- | --- | --- | --- |
| `+` | Сложение | `price + tax` | В некоторых СУБД также используется для конкатенации строк, например в MS SQL Server |
| `-` | Вычитание | `amount - discount` | Также унарный минус: `-amount` |
| `*` | Умножение | `price * quantity` |  |
| `/` | Деление | `total / count` | Поведение целочисленного деления зависит от СУБД |
| `%` | Остаток от деления | `id % 2` | PostgreSQL, MySQL, MS SQL Server, SQLite |
| `DIV` | Целочисленное деление | `10 DIV 3` | MySQL |
| `^` | Возведение в степень или XOR | `2 ^ 3` | Зависит от СУБД; в PostgreSQL - степень, в MySQL - bitwise XOR |

## Операторы сравнения

| Оператор | Назначение | Пример | Примечание |
| --- | --- | --- | --- |
| `=` | Равно | `status = 'ACTIVE'` | Не использовать для сравнения с `NULL` |
| `<>` | Не равно | `status <> 'DELETED'` | Стандартный SQL |
| `!=` | Не равно | `status != 'DELETED'` | Поддерживается большинством СУБД, но не базовый стандартный вариант |
| `>` | Больше | `amount > 1000` |  |
| `<` | Меньше | `amount < 1000` |  |
| `>=` | Больше или равно | `created_at >= '2026-01-01'` |  |
| `<=` | Меньше или равно | `created_at <= '2026-01-31'` |  |
| `BETWEEN ... AND ...` | Значение в диапазоне | `amount BETWEEN 100 AND 500` | Границы включаются |
| `NOT BETWEEN ... AND ...` | Значение вне диапазона | `amount NOT BETWEEN 100 AND 500` |  |
| `IN (...)` | Значение входит в список | `status IN ('NEW', 'DONE')` | Удобно вместо набора `OR` |
| `NOT IN (...)` | Значение не входит в список | `status NOT IN ('DELETED')` | Осторожно с `NULL` внутри списка |
| `LIKE` | Поиск по шаблону | `name LIKE 'Ann%'` | `%` - любая последовательность, `_` - один символ |
| `NOT LIKE` | Не соответствует шаблону | `name NOT LIKE 'test%'` |  |
| `ILIKE` | Регистронезависимый `LIKE` | `name ILIKE 'ann%'` | PostgreSQL |
| `SIMILAR TO` | SQL regex-подобный шаблон | `code SIMILAR TO '(A|B)%'` | PostgreSQL, не везде |
| `REGEXP`, `RLIKE` | Регулярное выражение | `name REGEXP '^A'` | MySQL, MariaDB; синтаксис зависит от СУБД |
| `~` | Соответствует regex | `name ~ '^A'` | PostgreSQL |
| `~*` | Соответствует regex без учета регистра | `name ~* '^a'` | PostgreSQL |
| `!~` | Не соответствует regex | `name !~ '^A'` | PostgreSQL |
| `!~*` | Не соответствует regex без учета регистра | `name !~* '^a'` | PostgreSQL |

## Работа с NULL и неизвестными значениями

| Оператор | Назначение | Пример | Примечание |
| --- | --- | --- | --- |
| `IS NULL` | Проверка на `NULL` | `deleted_at IS NULL` | Правильный способ сравнения с `NULL` |
| `IS NOT NULL` | Проверка, что значение не `NULL` | `email IS NOT NULL` |  |
| `IS TRUE` | Проверка на true | `is_active IS TRUE` | Удобно для boolean-полей |
| `IS FALSE` | Проверка на false | `is_active IS FALSE` |  |
| `IS UNKNOWN` | Проверка на unknown | `condition IS UNKNOWN` | Связано с трехзначной логикой SQL |
| `IS DISTINCT FROM` | Безопасное сравнение с учетом `NULL` | `a IS DISTINCT FROM b` | PostgreSQL, SQL standard; `NULL` считается сравнимым значением |
| `IS NOT DISTINCT FROM` | Безопасное равенство с учетом `NULL` | `a IS NOT DISTINCT FROM b` | PostgreSQL, SQL standard |
| `<=>` | NULL-safe equal | `a <=> b` | MySQL |
| `COALESCE()` | Первое не-NULL значение | `COALESCE(phone, email, 'n/a')` | Функция, но часто используется как операторная подсказка |
| `NULLIF()` | Вернуть `NULL`, если значения равны | `NULLIF(value, 0)` | Часто используют против деления на ноль |

## Логические операторы

| Оператор | Назначение | Пример | Примечание |
| --- | --- | --- | --- |
| `AND` | Логическое И | `status = 'NEW' AND amount > 100` | Приоритет выше, чем у `OR` |
| `OR` | Логическое ИЛИ | `status = 'NEW' OR status = 'DONE'` | Скобки улучшают читаемость |
| `NOT` | Логическое НЕ | `NOT is_blocked` |  |
| `ANY` | Сравнение с любым значением подзапроса/массива | `amount > ANY (SELECT limit_value FROM limits)` | Синтаксис зависит от СУБД |
| `SOME` | Синоним `ANY` | `amount > SOME (...)` | SQL standard |
| `ALL` | Сравнение со всеми значениями | `amount > ALL (SELECT limit_value FROM limits)` |  |
| `EXISTS` | Проверка существования строк | `EXISTS (SELECT 1 FROM orders o WHERE o.user_id = u.id)` | Часто быстрее и выразительнее, чем `IN` для correlated subquery |
| `NOT EXISTS` | Проверка отсутствия строк | `NOT EXISTS (SELECT 1 FROM orders o WHERE o.user_id = u.id)` | Хороший вариант вместо `NOT IN` при возможных `NULL` |

## Операторы объединения результатов запросов

| Оператор | Назначение | Пример | Примечание |
| --- | --- | --- | --- |
| `UNION` | Объединить результаты и убрать дубли | `SELECT id FROM a UNION SELECT id FROM b` | Требует совместимые колонки |
| `UNION ALL` | Объединить результаты без удаления дублей | `SELECT id FROM a UNION ALL SELECT id FROM b` | Обычно быстрее `UNION` |
| `INTERSECT` | Пересечение результатов | `SELECT id FROM a INTERSECT SELECT id FROM b` | Не во всех СУБД |
| `INTERSECT ALL` | Пересечение с учетом дублей | `SELECT id FROM a INTERSECT ALL SELECT id FROM b` | Поддержка зависит от СУБД |
| `EXCEPT` | Разность результатов | `SELECT id FROM a EXCEPT SELECT id FROM b` | PostgreSQL, SQL Server; в Oracle аналог - `MINUS` |
| `EXCEPT ALL` | Разность с учетом дублей | `SELECT id FROM a EXCEPT ALL SELECT id FROM b` | Поддержка зависит от СУБД |
| `MINUS` | Разность результатов | `SELECT id FROM a MINUS SELECT id FROM b` | Oracle |

## JOIN-операторы

| Оператор | Назначение | Пример | Примечание |
| --- | --- | --- | --- |
| `INNER JOIN` | Только совпавшие строки из обеих таблиц | `users u INNER JOIN orders o ON o.user_id = u.id` | `JOIN` без типа обычно означает `INNER JOIN` |
| `LEFT JOIN` | Все строки слева + совпадения справа | `users u LEFT JOIN orders o ON o.user_id = u.id` | То же, что `LEFT OUTER JOIN` |
| `RIGHT JOIN` | Все строки справа + совпадения слева | `orders o RIGHT JOIN users u ON o.user_id = u.id` | Часто можно заменить на `LEFT JOIN` перестановкой таблиц |
| `FULL JOIN` | Все строки с обеих сторон | `a FULL JOIN b ON a.id = b.id` | То же, что `FULL OUTER JOIN`; не везде поддерживается |
| `CROSS JOIN` | Декартово произведение | `sizes CROSS JOIN colors` | Все комбинации строк |
| `SELF JOIN` | Join таблицы с самой собой | `employees e JOIN employees m ON e.manager_id = m.id` | Не отдельный оператор, а прием |
| `NATURAL JOIN` | Join по одноименным колонкам | `a NATURAL JOIN b` | Лучше избегать в промышленном SQL из-за неявности |
| `JOIN ... USING (...)` | Join по одноименным колонкам | `orders JOIN users USING (user_id)` | Удобно, но менее явно, чем `ON` |
| `LATERAL JOIN` | Join с зависимым подзапросом | `users u LEFT JOIN LATERAL (...) x ON true` | PostgreSQL; в SQL Server близкий аналог - `APPLY` |
| `CROSS APPLY` | Применить табличное выражение к каждой строке | `t CROSS APPLY fn(t.id)` | SQL Server, Oracle |
| `OUTER APPLY` | Как `CROSS APPLY`, но сохраняет строки без результата | `t OUTER APPLY fn(t.id)` | SQL Server, Oracle |

## Строковые операторы

| Оператор | Назначение | Пример | Примечание |
| --- | --- | --- | --- |
| `||` | Конкатенация строк | `first_name || ' ' || last_name` | PostgreSQL, Oracle, SQLite; в MySQL зависит от режима |
| `+` | Конкатенация строк | `first_name + ' ' + last_name` | MS SQL Server |
| `CONCAT()` | Конкатенация строк | `CONCAT(first_name, ' ', last_name)` | Функция, поддержка зависит от СУБД |
| `COLLATE` | Указать collation | `name COLLATE "C"` | Синтаксис зависит от СУБД |
| `LIKE ... ESCAPE` | Escape-символ для шаблона | `code LIKE '10\%%' ESCAPE '\'` | Для поиска символов `%` и `_` |

## Битовые операторы

| Оператор | Назначение | Пример | Примечание |
| --- | --- | --- | --- |
| `&` | Bitwise AND | `flags & 4` | Поддержка зависит от СУБД |
| `|` | Bitwise OR | `flags | 4` |  |
| `^` | Bitwise XOR | `flags ^ 4` | В MySQL - XOR; в PostgreSQL `#` для integer XOR |
| `#` | Bitwise XOR | `flags # 4` | PostgreSQL |
| `~` | Bitwise NOT | `~flags` | Также regex-оператор в PostgreSQL для строк |
| `<<` | Сдвиг влево | `flags << 1` | PostgreSQL, MySQL, SQLite |
| `>>` | Сдвиг вправо | `flags >> 1` | PostgreSQL, MySQL, SQLite |

## Операторы JSON и массивов

| Оператор | Назначение | Пример | Примечание |
| --- | --- | --- | --- |
| `->` | Получить JSON-поле/элемент | `data -> 'user'` | PostgreSQL, MySQL, SQLite JSON; поведение отличается |
| `->>` | Получить JSON-поле как текст | `data ->> 'name'` | PostgreSQL, SQLite; в MySQL есть похожий синтаксис |
| `#>` | Получить JSON по пути | `data #> '{user,name}'` | PostgreSQL |
| `#>>` | Получить JSON по пути как текст | `data #>> '{user,name}'` | PostgreSQL |
| `@>` | Содержит JSON/массив | `data @> '{"active": true}'` | PostgreSQL jsonb/array |
| `<@` | Содержится в JSON/массиве | `ARRAY[1,2] <@ ARRAY[1,2,3]` | PostgreSQL |
| `?` | JSON содержит ключ | `data ? 'user'` | PostgreSQL jsonb |
| `?|` | JSON содержит любой из ключей | `data ?| ARRAY['a','b']` | PostgreSQL jsonb |
| `?&` | JSON содержит все ключи | `data ?& ARRAY['a','b']` | PostgreSQL jsonb |
| `&&` | Массивы пересекаются | `tags && ARRAY['sql','db']` | PostgreSQL arrays |
| `ANY(array)` | Значение равно любому элементу массива | `'sql' = ANY(tags)` | PostgreSQL |
| `ALL(array)` | Условие верно для всех элементов массива | `10 > ALL(scores)` | PostgreSQL |

## Операторы полнотекстового поиска

| Оператор | Назначение | Пример | Примечание |
| --- | --- | --- | --- |
| `@@` | tsvector соответствует tsquery | `search_vector @@ plainto_tsquery('sql')` | PostgreSQL |
| `@@@` | Полнотекстовый поиск | `MATCH(title) AGAINST('sql')` | В MySQL используется не как оператор, а как конструкция `MATCH ... AGAINST` |
| `MATCH ... AGAINST` | Полнотекстовый поиск | `MATCH(title, body) AGAINST('sql')` | MySQL, MariaDB |
| `CONTAINS` | Полнотекстовый поиск | `CONTAINS(description, 'sql')` | MS SQL Server, Oracle - синтаксис отличается |

## Операторы DML - работа с данными

| Оператор / конструкция | Назначение | Пример | Примечание |
| --- | --- | --- | --- |
| `SELECT` | Получить данные | `SELECT * FROM users` |  |
| `INSERT` | Добавить строки | `INSERT INTO users(name) VALUES ('Ann')` |  |
| `UPDATE` | Изменить строки | `UPDATE users SET name = 'Ann' WHERE id = 1` | Всегда проверять `WHERE` |
| `DELETE` | Удалить строки | `DELETE FROM users WHERE id = 1` | Всегда проверять `WHERE` |
| `MERGE` | Upsert/синхронизация данных | `MERGE INTO target USING source ...` | SQL Server, Oracle, PostgreSQL 15+; синтаксис отличается |
| `UPSERT` | Вставить или обновить | `INSERT ... ON CONFLICT ... DO UPDATE` | Не единый стандартный оператор; PostgreSQL `ON CONFLICT`, MySQL `ON DUPLICATE KEY UPDATE` |
| `RETURNING` | Вернуть измененные строки | `INSERT ... RETURNING id` | PostgreSQL, SQLite, Oracle; в SQL Server - `OUTPUT` |
| `OUTPUT` | Вернуть измененные строки | `UPDATE ... OUTPUT inserted.id` | MS SQL Server |
| `TRUNCATE` | Быстро очистить таблицу | `TRUNCATE TABLE logs` | DDL/DML-гибрид, зависит от СУБД; обычно быстрее `DELETE` без `WHERE` |

## Операторы DDL - структура БД

| Оператор | Назначение | Пример | Примечание |
| --- | --- | --- | --- |
| `CREATE` | Создать объект | `CREATE TABLE users (...)` | Таблица, индекс, view, schema и т.д. |
| `ALTER` | Изменить объект | `ALTER TABLE users ADD COLUMN email text` |  |
| `DROP` | Удалить объект | `DROP TABLE users` | Опасная операция |
| `RENAME` | Переименовать объект | `ALTER TABLE users RENAME TO app_users` | Синтаксис зависит от СУБД |
| `COMMENT` | Добавить комментарий к объекту | `COMMENT ON TABLE users IS 'Пользователи'` | PostgreSQL, Oracle |
| `CREATE INDEX` | Создать индекс | `CREATE INDEX idx_users_email ON users(email)` |  |
| `DROP INDEX` | Удалить индекс | `DROP INDEX idx_users_email` | Синтаксис зависит от СУБД |
| `CREATE VIEW` | Создать представление | `CREATE VIEW active_users AS SELECT ...` |  |
| `CREATE MATERIALIZED VIEW` | Создать материализованное представление | `CREATE MATERIALIZED VIEW mv AS SELECT ...` | Поддержка зависит от СУБД |

## Операторы DCL - доступы

| Оператор | Назначение | Пример | Примечание |
| --- | --- | --- | --- |
| `GRANT` | Выдать права | `GRANT SELECT ON users TO analyst` |  |
| `REVOKE` | Отозвать права | `REVOKE SELECT ON users FROM analyst` |  |
| `DENY` | Явно запретить право | `DENY SELECT ON users TO analyst` | MS SQL Server |

## Операторы TCL - транзакции

| Оператор | Назначение | Пример | Примечание |
| --- | --- | --- | --- |
| `BEGIN` | Начать транзакцию | `BEGIN` | В SQL Server часто `BEGIN TRANSACTION` |
| `START TRANSACTION` | Начать транзакцию | `START TRANSACTION` | MySQL, PostgreSQL |
| `COMMIT` | Зафиксировать транзакцию | `COMMIT` |  |
| `ROLLBACK` | Откатить транзакцию | `ROLLBACK` |  |
| `SAVEPOINT` | Создать точку сохранения | `SAVEPOINT before_update` |  |
| `ROLLBACK TO SAVEPOINT` | Откатиться к savepoint | `ROLLBACK TO SAVEPOINT before_update` |  |
| `RELEASE SAVEPOINT` | Удалить savepoint | `RELEASE SAVEPOINT before_update` |  |
| `SET TRANSACTION` | Настроить параметры транзакции | `SET TRANSACTION ISOLATION LEVEL SERIALIZABLE` |  |
| `LOCK TABLE` | Заблокировать таблицу | `LOCK TABLE users IN EXCLUSIVE MODE` | Синтаксис зависит от СУБД |

## Операторы и конструкции SELECT

| Конструкция | Назначение | Пример | Примечание |
| --- | --- | --- | --- |
| `FROM` | Источник данных | `SELECT * FROM users` |  |
| `WHERE` | Фильтрация строк | `WHERE status = 'ACTIVE'` | Выполняется до группировки |
| `GROUP BY` | Группировка | `GROUP BY user_id` |  |
| `HAVING` | Фильтрация групп | `HAVING COUNT(*) > 1` | После `GROUP BY` |
| `ORDER BY` | Сортировка | `ORDER BY created_at DESC` |  |
| `LIMIT` | Ограничить число строк | `LIMIT 100` | PostgreSQL, MySQL, SQLite |
| `OFFSET` | Пропустить строки | `OFFSET 100` | На больших таблицах может быть медленно |
| `FETCH FIRST` | Ограничить число строк | `FETCH FIRST 100 ROWS ONLY` | SQL standard, Oracle, DB2, PostgreSQL |
| `TOP` | Ограничить число строк | `SELECT TOP 100 * FROM users` | MS SQL Server |
| `DISTINCT` | Убрать дубли | `SELECT DISTINCT user_id FROM orders` |  |
| `DISTINCT ON` | Убрать дубли по выражению | `SELECT DISTINCT ON (user_id) ...` | PostgreSQL |
| `AS` | Алиас | `SELECT name AS user_name` | Для колонок и таблиц |
| `WITH` | CTE | `WITH q AS (...) SELECT * FROM q` | Удобно для декомпозиции запроса |
| `WITH RECURSIVE` | Рекурсивный CTE | `WITH RECURSIVE tree AS (...)` | Иерархии, графы |
| `WINDOW` | Именованное окно | `WINDOW w AS (PARTITION BY user_id)` | PostgreSQL и другие |
| `OVER` | Оконная функция | `COUNT(*) OVER (PARTITION BY user_id)` |  |
| `PARTITION BY` | Разбиение окна | `SUM(amount) OVER (PARTITION BY user_id)` | Не путать с партиционированием таблиц |
| `FILTER` | Фильтр агрегата | `COUNT(*) FILTER (WHERE status = 'NEW')` | PostgreSQL, SQL standard |
| `CASE WHEN` | Условное выражение | `CASE WHEN amount > 0 THEN 'plus' ELSE 'zero' END` |  |

## Операторы сортировки и порядка NULL

| Оператор / конструкция | Назначение | Пример | Примечание |
| --- | --- | --- | --- |
| `ASC` | Сортировка по возрастанию | `ORDER BY created_at ASC` | Обычно по умолчанию |
| `DESC` | Сортировка по убыванию | `ORDER BY created_at DESC` |  |
| `NULLS FIRST` | `NULL` в начале | `ORDER BY closed_at NULLS FIRST` | PostgreSQL, Oracle |
| `NULLS LAST` | `NULL` в конце | `ORDER BY closed_at NULLS LAST` | PostgreSQL, Oracle |

## Операторы upsert и конфликтов

| Оператор / конструкция | Назначение | Пример | Примечание |
| --- | --- | --- | --- |
| `ON CONFLICT DO NOTHING` | Игнорировать конфликт | `INSERT ... ON CONFLICT (id) DO NOTHING` | PostgreSQL, SQLite |
| `ON CONFLICT DO UPDATE` | Обновить при конфликте | `INSERT ... ON CONFLICT (id) DO UPDATE SET ...` | PostgreSQL, SQLite |
| `ON DUPLICATE KEY UPDATE` | Обновить при конфликте ключа | `INSERT ... ON DUPLICATE KEY UPDATE name = VALUES(name)` | MySQL, MariaDB |
| `INSERT IGNORE` | Игнорировать ошибки вставки | `INSERT IGNORE INTO users ...` | MySQL; использовать осторожно |
| `REPLACE INTO` | Удалить старую строку и вставить новую | `REPLACE INTO users ...` | MySQL, SQLite; может иметь побочные эффекты |

## Операторы проверки ограничений

| Оператор / конструкция | Назначение | Пример | Примечание |
| --- | --- | --- | --- |
| `PRIMARY KEY` | Первичный ключ | `id bigint PRIMARY KEY` | Уникальность + not null |
| `FOREIGN KEY` | Внешний ключ | `FOREIGN KEY (user_id) REFERENCES users(id)` | Контроль ссылочной целостности |
| `UNIQUE` | Уникальность | `email text UNIQUE` |  |
| `CHECK` | Проверка условия | `CHECK (amount >= 0)` |  |
| `NOT NULL` | Запретить `NULL` | `email text NOT NULL` |  |
| `DEFAULT` | Значение по умолчанию | `created_at timestamp DEFAULT now()` |  |
| `GENERATED` | Вычисляемая/генерируемая колонка | `total AS (price * qty)` | Синтаксис зависит от СУБД |

## Приоритет операторов - общее правило

| Приоритет | Операторы | Комментарий |
| --- | --- | --- |
| 1 | `()` | Скобки всегда использовать для явного порядка |
| 2 | Унарные `+`, `-`, `NOT` | Может отличаться между СУБД |
| 3 | `*`, `/`, `%` | Арифметика |
| 4 | `+`, `-`, конкатенация | Арифметика / строки |
| 5 | Сравнения: `=`, `<>`, `>`, `<`, `LIKE`, `IN`, `BETWEEN`, `IS` | Возвращают boolean/unknown |
| 6 | `AND` | Выше, чем `OR` |
| 7 | `OR` | Ниже, чем `AND` |

## Частые ловушки

| Ловушка | Почему опасно | Как лучше |
| --- | --- | --- |
| `column = NULL` | Всегда дает unknown, а не true | `column IS NULL` |
| `NOT IN` с `NULL` в подзапросе | Может вернуть пустой результат | `NOT EXISTS` |
| `DELETE` без `WHERE` | Удалит все строки | Сначала проверить через `SELECT` |
| `UPDATE` без `WHERE` | Обновит все строки | Сначала проверить через `SELECT` |
| `UNION` вместо `UNION ALL` | Лишняя сортировка/дедупликация | Использовать `UNION ALL`, если дубли допустимы |
| `OFFSET` на больших таблицах | БД вынуждена пропускать много строк | Keyset pagination / cursor pagination |
| Неявные JOIN через `FROM a, b` | Легко получить декартово произведение | Явный `JOIN ... ON ...` |
| `SELECT *` в API/проде | Лишние данные, хрупкий контракт | Явный список колонок |
