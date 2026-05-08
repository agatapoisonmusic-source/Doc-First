# Связи сервисов CNETS - 02 trade portfolio market

## 4. Торговля

### cnets.api.trading.orders
Предоставляет API для выставления заявок через Матрицу.

Зависимости: trade, RMS

### mx.cnets.rms.orders.in
[ЕЩЁ НЕ В БОЮ] Загружает ордера из очереди RMS->>CNETS.

Зависимости: trade, RMS

### mx.cnet.rms.orders.status.out
Загружает статусы ордеров из очереди RMS->>CNETS.

Зависимости: trade, RMS

### mx.cnets.rms.stop-orders.in
Загружает стоп-ордера из очереди RMS->>CNETS.

Зависимости: trade, RMS

### cnets.mx.rms.order-id.out
Выполняет синхронизацию идентификаторов внешних заявок CNETS->>RMS и сверку числа заявок RMS->>CNETS.

Зависимости: trade, RMS

### mx.cnets.gates.deals.in
Агрегирует потоки данных по сделкам из биржевых стримеров Матрицы в CNETS.

Зависимости: ITI_Integration, trade, RMS, Streamers

### mx.cnets.rms.deals.sync
Выполняет сверку сделок RMS->>CNETS; читает из очереди RMS сообщения ins_QuikTrades.

Зависимости: ITI_Integration, trade, RMS

## 5. События

### cnets.api.account.events
Предоставляет API для просмотра событий и FIFO-истории позиций.

Зависимости: ITI_Integration, trade, RMS

### cnets.api.events.collect
Выполняет извлечение событий биржевой торговли из БД trade и загрузку событий, полученных из BO.

Зависимости: ITI_Integration, trade, BO через ESB

## 6. Переводы и вывод ДС

### cnets.api.account.external-transfer
Предоставляет API для расчёта налога при выводе ДС и регистрации заявок на вывод.

Зависимости: ITI_Integration, trade, RMS, SimpleSign, Bitrix для генерации документа через ESB

### cnets.api.account.internal-transfer
Предоставляет API для выполнения межпортфельных переводов.

Зависимости: ITI_Integration, trade, RMS, QuikInstr

### cnets.api.account.templates
Предоставляет API для управления шаблонами банковских реквизитов для вывода ДС.

Зависимости: ITI_Integration

### mx.bank.details.sync
Выгружает банковские реквизиты клиентов из RMS и транслирует их в шаблоны в CNETS.

Зависимости: ITI_Integration, RMS

## 7. Портфель

### mx.rms.cnets.positions.in
Ежедневно выгружает все позиции из RMS->>CNETS.

Зависимости: trade, RMS

### cnet.portfolio.state.ts
Предоставляет API для получения статуса торгового дня, признаков портфеля и истории ликвидационной стоимости.

Зависимости: ITI_Integration, trade

### front.cnets.adapter.http.out
Предоставляет API для получения списка позиций.

Зависимости: ITI_Integration, trade, RMS, Kafka

### cnets.api.account.portfolio-values
Предоставляет API для получения сведений по портфелям.

Зависимости: ITI_Integration, trade, RMS

### mx.cnets.rms.portfolio-values.in
Выполняет кэширование данных по портфелям из RMS в CNETS.

Зависимости: ITI_Integration, trade, RMS, Redis

## 8. Рынок

### mx.cnets.gates.alltrades.in
Агрегирует потоки обезличенных данных по сделкам из биржевых стримеров Матрицы, создаёт из них минутные свечи и пишет их в Кафку.

Зависимости: Streamers, Kafka

### cnets.data.candles.writer
Читает минутные свечи из Кафки и обновляет соответствующие свечи всех таймфреймов в InfluxDB; предоставляет внутрений API для обогащения ценовых рядов из ODAS и MOEX ISS.

Зависимости: Kafka, InfluxDB, ODAS, trade, MOEX

### mx.cnets.api.candles.out
Предоставляет API для получения исторических рыночных данных (свечек).

Зависимости: InfluxDB, trade

### cnets.api.security.recommends
Предоставляет API для получения списка ТОП-инструментов по взлётам/падениям/оборотам.

Зависимости: ITI_Integration, trade

### mx.cnets.rms.missed-rates.in
Запрашивает из RMS недостающие в CNETS котировки.

Зависимости: trade, RMS

### mx.cnets.gates.orderbook.ws
Агрегирует потоки данных по стаканам из биржевых стримеров Матрицы и транслирует их подписчикам по WebSocket.

Зависимости: Streamers

### mx.cnets.gates.rates.in
Агрегирует потоки данных по котировкам из биржевых стримеров Матрицы в CNETS.

Зависимости: Streamers, ITI_Integration

### mx.cnets.gates.rates.ws
Агрегирует потоки данных по котировкам из биржевых стримеров Матрицы и транслирует их подписчикам по WebSocket.

Зависимости: Streamers

### mx.rms.cnets.instruments.in
Выполняет синхронизацию справочника инструментов RMS->>CNETS.

Зависимости: trade, RMS

### cnets.save.instruments.quikrates
Выполняет импорт и обновление инструментов на основе котировок.

Зависимости: trade

### mx.cnets.gates.securities.in
Агрегирует потоки данных по инструментам из биржевых стримеров Матрицы и актуализирует их торговый статус в CNETS.

Зависимости: Streamers, trade

### cnets.api.trading.schedule
Предоставляет API для получения расписания торгов.

Зависимости: RMS

### cnets.api.currency.rate
Предоставляет API для получения текущего курса основных валют.

Зависимости: trade

### cnets.api.instrument.search.out
Предоставляет API для поиска инструментов.

Зависимости: trade

### cnets.api.security.notes.inout
Предоставляет API для работы с заметками по инструментам.

Зависимости: ITI_Integration
