# SYSTEM DESIGN - SENIOR SA ШПАРГАЛКА

## 1. Главный принцип

Архитектура = компромисс (trade-off)

Нет "лучших" технологий. Есть:

- проблема
- ограничения
- цена решения
- последствия

Всегда отвечай:

- Проблема ->
- Почему простое решение не подходит ->
- Что дает решение ->
- Чем платим

## 2. Основа всех distributed systems

LDPF

- L - Latency - задержки
- D - Duplication - дубли
- P - Partial failure - частичные отказы
- F - Freshness - рассинхрон данных

Практически все:

- Kafka
- retries
- idempotency
- CQRS
- saga
- outbox
- eventual consistency

решают одну из этих проблем.

## 3. Архитектура всегда следует из процесса

Не думай:

- "нужна ли Kafka?"

Думай:

- процесс долгий?
- есть ручные шаги?
- есть ожидание внешних систем?
- важен real-time?
- есть SLA?
- есть деньги?

Process -> Constraints -> Architecture

## 4. SCALE - главный framework для system design

S - State

- Где хранится состояние?
- Кто source of truth?
- Кто владеет данными?

C - Consistency

- Нужна strict consistency?
- Или eventual consistency допустима?

A - Availability

- Что должно работать всегда?
- Что можно деградировать?

L - Load

- Что преобладает: read / write / events / files?
- Где hot spots?

E - Evolution

- Как система будет расти?
- Где появится bottleneck?
- Что изменится через 2 года?

## 5. FLOW - framework для интеграций

- F - Failure - что если сервис недоступен?
- L - Latency - что если отвечает 20 секунд?
- O - Ordering - важен ли порядок сообщений?
- W - Weight - какая нагрузка и объем данных?

## 6. Как отвечать на senior-вопрос

1. Назвать проблему
2. Объяснить почему сложно
3. Назвать trade-off
4. Предложить решение
5. Назвать риски решения

Шаблон:

"Проблема здесь в ...
Сложность появляется потому что ...
Если хотим X - жертвуем Y.
Обычно это решают через ...
Но появляются риски ..."

## 7. Highload thinking

Всегда спрашивай себя:

- Что чаще читают?
- Что чаще пишут?
- Что чаще обновляют?
- Что чаще агрегируют?
- Что чаще сортируют?

Это автоматически приводит к:

- cache
- replicas
- CQRS
- partitioning
- denormalization
- async processing

## 8. Когда нужен async

Async нужен если:

- процесс долгий
- пользователь не должен ждать
- внешняя система нестабильна
- есть пики нагрузки
- важен retry
- нужен event processing

Цена:

- eventual consistency
- сложность
- retry
- DLQ
- observability
- idempotency

## 9. Когда нужны микросервисы

НЕ потому что "модно".

А когда:

- разные bounded contexts
- разные команды
- разные нагрузки
- разные SLA
- независимый deployment
- independent scaling

Цена:

- distributed systems complexity
- network latency
- eventual consistency
- tracing
- orchestration
- DevOps overhead

## 10. Главные distributed patterns

- Retry - повтор запроса
- Circuit breaker - не долбить упавший сервис
- Idempotency - повторный запрос не создает повторный эффект
- Outbox - не потерять событие между БД и Kafka
- Inbox - не обработать событие дважды
- Saga - компенсации вместо distributed transaction
- CQRS - разделение read/write модели
- Eventual consistency - данные согласуются не мгновенно

## 11. Что любят слышать на senior интервью

- "Это trade-off"
- "Нужно понимать SLA"
- "Зависит от read/write profile"
- "Нужно определить source of truth"
- "Я бы сначала посмотрел метрики"
- "Важно понять lifecycle сущности"
- "Это вопрос consistency vs availability"
- "Я бы начал с бизнес-процесса"

## 12. Самое важное

Senior отличается не количеством технологий.

Senior:

- понимает последствия решений
- видит ограничения
- думает через процессы
- умеет объяснять trade-off
- начинает с проблемы, а не с технологии
