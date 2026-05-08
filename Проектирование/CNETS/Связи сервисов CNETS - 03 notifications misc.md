# Связи сервисов CNETS - 03 notifications misc

## 9. Уведомления

### cnets.push.receiver.inout
Сервис получает сообщения по роуту POST /api/v1/push/send-notification; после получения: 1) сохраняет сообщения в БД, 2) выполняет передачу сообщений в сервис отправки cnets.push.provider.inout через Kafka или, при недоступности Kafka, по резервному роуту POST /api/v1/push/send-notification-reserve; выполняет периодический перенос в архив старых сообщений старше 30 дней.

### rms.cnets.push.notify
Вычитывает пуш сообщения event, marginCall из очереди RMS ins_Notification и передает в сервис cnets.push.receiver.inout для последующей отправки.

### cnets.push.alerts.out
Сервис мониторинга рынка на основе пользовательских правил и отправки алертов в очередь.

### cnets.push.provider.inout
Получает сообщения от сервиса cnets.push.receiver.inout из Kafka или по резервному роуту. Выполняет отправку сообщения на устройства пользователя через FCM.

### cnets.push.api.inout
API сервис для получения истории, количестве непрочитанных, массовой рассылки и тд.

## 10. Разное

### mx.cnet.rmsdata.in
Выполняет загрузку сообщений из очереди RMS->>CNETS всех типов, кроме тех, для которых есть специализированные читатели; блокирует пользователей в KeyCloak при получении соответствующего сообщения usp_ImportUsers.

Зависимости: ITI_Integration, RMS, KeyCloak

### mx.cnets.db.import-clients.in
Выполняет сверку договоров, портфелей и позиций RMS->>CNETS.

Зависимости: ITI_Integration, trade, RMS

### cnets.esb.http.backcall.in
Предоставляет API для заказа обратного звонка.

Зависимости: trade, Redis, Bitrix - передаём заявку на обр. звонок через ESB

### front.cnets.api.reports.out
Предоставляет API для заказа брокерского и депозитарного отчётов из BO и реестра поручений из архива ордеров.

Зависимости: ITI_Integration, ORDERS для генерации реестра поручений, BO для генерации брокерского и депозитарного отчётов через ESB

### cnets.api.client.app.source.out
Извлекает из ITI_Integration.dbo.ITI_action_logger данные об устройствах пользователей и сохраняет в ITI_Integration.api.ITI_ClientAppSource.

Зависимости: ITI_Integration

### cnets.api.account.personal-data
Предоставляет API для получения персональных данных клиента: ИНН, адрес, физ./юр. лицо.

Зависимости: ITI_Integration

### cnets.api.clientOptions.inout
Предоставляет API для управления параметрами и признаками пользователя: статус онбординга, настройки.

Зависимости: ITI_Integration, trade, MinIO

### cnets.plug.tariffs.out
Предоставляет API для получения информации по тарифам Компании.

Зависимости: нет зависимостей

### cnets.api.portfolio.tariff.out
Предоставляет API для получения информации по клиентским тарифам из BO.

Зависимости: ITI_Integration, RMS, BO - получение справочника тарифов через ESB

### cnets.api.products.info.out
Предоставляет API для получения сведений по продуктам: баннеры, витрина, и управления статусом баннеров: закрытие.

Зависимости: ITI_Integration, RMS, CMS

### cnets.api.blocked.redemption
Предоставляет API для регистрации заявок на выкуп заблокированных бумаг.

Зависимости: ITI_Integration, RMS, EDO

### cnets.api.mobile.version.inout
Предоставляет API для получения минимальной/актуальной версии МП.

Зависимости: ITI_Integration

### cnets.api.picture_loader
Предоставляет API для получения изображений.

Зависимости: MinIO

### cnets.api.maintenance.out
Предоставляет API для получения сведений о технических работах.

Зависимости: ITI_Integration

### api-dh-api
[deprecated] Транслирует авторизованные запросы к API в соответствующие хранимые процедуры в схеме trade.api.

Зависимости: trade и что-то ещё

### api-dh-gk
[deprecated] Авторизует запросы к проприетарному API; транслирует идентификаторы KeyCloak в trade.dbo.Users.User_id.

Зависимости: trade и что-то ещё
