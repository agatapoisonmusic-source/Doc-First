# HIGHLOAD системы

## Короткая формула для запоминания

**Масштабировать - Кэшировать - Асинхронить - Оптимизировать - Наблюдать - Деградировать**

## Вопросы при разборе highload-кейса

- где узкое место: клиент, сеть, backend, БД, внешняя система?
- нагрузка на чтение или на запись?
- проблема постоянная или только в пиках?
- можно ли кэшировать?
- можно ли вынести в async?
- можно ли уменьшить payload?
- можно ли предрассчитать результат?
- что будет при отказе зависимости?
- как мы поймем, что стало лучше?

## Проблемы, решения и инструменты

| Проблема | Решение | Инструменты / технологии |
| --- | --- | --- |
| Сервис не справляется с количеством запросов | Горизонтально масштабировать приложение | Load Balancer, Kubernetes, Auto Scaling |
| Один сервер перегружен | Вертикально увеличить ресурсы | CPU, RAM, disk IOPS |
| Пользователи ждут долгий ответ | Оптимизировать latency критичных операций | Profiling, tracing, APM |
| Пиковая нагрузка ломает систему | Сглаживать нагрузку | Queue, Kafka, RabbitMQ, rate limiting |
| Тяжелая операция выполняется в основном запросе | Вынести в асинхронную обработку | MQ, Kafka, RabbitMQ, background workers |
| Часто запрашиваются одни и те же данные | Кэшировать данные | Redis, Memcached, CDN |
| Статика грузит backend | Отдавать статику через CDN | CDN, object storage |
| База данных перегружена чтением | Разделить чтение и запись | Read replicas, CQRS |
| База данных перегружена записью | Оптимизировать запись и батчить операции | Batch insert, queue, write-behind |
| Запросы к БД работают медленно | Оптимизировать SQL и индексы | EXPLAIN, indexes, query profiling |
| Индексов не хватает | Добавить нужные индексы | B-tree index, composite index |
| Индексов слишком много | Удалить лишние индексы | Index audit, query analysis |
| В таблице слишком много данных | Разделить данные | Partitioning, sharding, archiving |
| OFFSET-пагинация стала медленной | Использовать cursor pagination | Cursor pagination, keyset pagination |
| API возвращает слишком много данных | Ограничить размер ответа | Pagination, filtering, field selection |
| Клиент часто дергает API | Снизить количество запросов | BFF, aggregation API, caching |
| Один запрос дергает много сервисов | Уменьшить цепочку синхронных вызовов | API composition, async events, denormalized read model |
| Сервисы ждут друг друга и тормозят | Перейти на async там, где не нужен мгновенный ответ | Kafka, RabbitMQ, event-driven architecture |
| Внешняя система медленная | Изолировать зависимость | Timeout, retry, circuit breaker, cache |
| Внешняя система недоступна | Деградировать функциональность | Fallback, graceful degradation, circuit breaker |
| Повторные запросы создают дубли | Сделать операции идемпотентными | Idempotency key, request deduplication |
| Пользователь отправляет запрос несколько раз | Защититься от дублей | Idempotency key, optimistic locking, disabled button |
| Несколько процессов меняют одни данные | Управлять конкурентным доступом | Optimistic locking, pessimistic locking, distributed lock |
| Очередь задач растет быстрее обработки | Масштабировать consumer'ы и контролировать backlog | Consumer scaling, monitoring lag, DLQ |
| Сообщения обрабатываются повторно | Делать обработку идемпотентной | Deduplication, idempotency key, exactly-once semantics where available |
| Сообщения приходят не по порядку | Проектировать порядок обработки | Partition key, ordering key, sequence number |
| Нагрузка на один микросервис выше остальных | Масштабировать только узкое место | Kubernetes HPA, service autoscaling |
| Система падает из-за одного проблемного сервиса | Изолировать отказы | Circuit breaker, bulkhead, timeout |
| Нет понимания, где узкое место | Включить наблюдаемость | Metrics, logs, traces, APM |
| Сложно расследовать медленные запросы | Использовать распределенный трейсинг | OpenTelemetry, Jaeger, Zipkin |
| Не видно технических метрик | Собирать метрики | Prometheus, Grafana |
| Не видно ошибок пользователей | Собирать продуктовые и frontend-метрики | Sentry, Firebase Crashlytics, Real User Monitoring |
| Система деградирует незаметно | Настроить алерты | Alertmanager, Grafana alerts, PagerDuty |
| Релиз ухудшил производительность | Проверять нагрузку до релиза | Load testing, stress testing, k6, JMeter, Gatling |
| Непонятно, сколько выдержит система | Проводить нагрузочное тестирование | k6, JMeter, Gatling |
| Кэш отдает устаревшие данные | Настроить TTL и инвалидацию | Cache TTL, cache invalidation, event-based invalidation |
| Кэш резко опустел и БД легла | Защититься от cache stampede | Locking, request coalescing, cache warming |
| Много пользователей одновременно запрашивают одно и то же | Предварительно прогревать кэш | Cache warming, precompute |
| Нужно быстро отдавать агрегаты | Предрассчитывать витрины | Materialized views, read models, scheduled jobs |
| Отчеты грузятся слишком долго | Делать отчеты асинхронно | Report queue, background worker, notification |
| Данные нужны в реальном времени | Использовать streaming или push | WebSocket, Server-Sent Events, Kafka |
| Клиенты постоянно опрашивают сервер | Заменить polling на push, если оправдано | WebSocket, SSE, push notifications |
| Слишком большой JSON/XML | Уменьшить payload | Compression, gzip, field filtering, protobuf |
| Сеть становится узким местом | Сократить сетевые вызовы и объем данных | Batching, compression, CDN |
| Монолит тяжело масштабировать частями | Сначала выделить модули и узкие места | Modular monolith, profiling |
| Микросервисы создают слишком много сетевых вызовов | Укрупнить границы или агрегировать данные | BFF, API Gateway aggregation, domain redesign |
| Нужно выдерживать отказ дата-центра | Делать отказоустойчивую архитектуру | Multi-AZ, replication, failover |
| Один компонент является single point of failure | Добавить резервирование | HA cluster, replication, load balancing |
| Хранилище переполнено историческими данными | Архивировать старые данные | Archiving, cold storage, S3 |
| Логи занимают слишком много места | Управлять хранением логов | Log retention, sampling, aggregation |
| Пользовательские запросы слишком частые | Ограничить или приоритизировать | Rate limiting, throttling, quotas |
| Важные клиенты должны обслуживаться первыми | Ввести приоритеты | Priority queue, QoS |
| Система не успевает обрабатывать все данные | Масштабировать обработку потоков | Kafka partitions, stream processing, Flink, Spark Streaming |
| Нужно быстро искать по большим данным | Использовать отдельный поисковый индекс | Elasticsearch, OpenSearch |
| Основная БД используется и для OLTP, и для аналитики | Разделить операционные и аналитические нагрузки | OLTP DB, DWH, ClickHouse, BigQuery |
| Аналитические запросы тормозят прод | Вывести аналитику в отдельное хранилище | ETL/ELT, ClickHouse, DWH |
| Нужно снизить нагрузку на API авторизации | Кэшировать проверки или использовать короткие локальные проверки | JWT, token introspection cache |
| Пользователь видит ошибку при частичном сбое | Сделать graceful degradation | Fallback UI, cached data, degraded mode |
