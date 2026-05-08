# Связи сервисов CNETS - 01 core

## 1. Аутентификация

### cnets.api.authorization
Предоставляет API для проверки токена в Redis & KeyCloak, получения списка флагов стран и верификации OTP в контексте 2FA.

Зависимости: ITI_Integration, trade, RMS, KeyCloak, Redis, SmsService

### cnets.mx.key-cloak.block.out
Предоставляет API для рассылки email/sms-уведомлений о блокировке пользователей.

Зависимости: ITI_Integration, trade, RMS, KeyCloak, SmsService

### cnets.api.reset-password.in
Предоставляет API для смены и восстановления паролей пользователей.

Зависимости: ITI_Integration, RMS, KeyCloak, SmsService

### cnets.key-cloak.users.in
Обрабатывает заявки из очереди personal.kc.UsersQueue на создание и изменение паролей пользователей в KeyCloak.

Зависимости: ITI_Integration, trade, keycloak_trade, personal

### cnets.api.user-block-sync
Предоставляет внутрений API для синхронизации блокировки пользователей CNETS->>RMS и установки/снятия блокировки в KeyCloak.

Зависимости: ITI_Integration, keycloak_trade, RMS, KeyCloak

### cnets.logger.action.in
Логирует некоторые действия клиентов, переписывает значение поля username в запросах на получение токена в KeyCloak и кэширует токены в Redis.

Зависимости: ITI_Integration, trade, RMS, keycloak_trade, KeyCloak, Redis

## 2. Пополнение счёта

### cnets.api.account.top-up
Предоставляет API для получения списка банков-партнёров и реквизитов для пополнения через банковский перевод и перевод ценных бумаг.

Зависимости: ITI_Integration, trade

### cnets.api.card-payment.out
Предоставляет API для пополнения счёта через банковскую карту.

Зависимости: RSB, RMS, BO напрямую

### cnets.deposit.proxy.sbp.in
[ПРОКСИ] Предоставляет API для пополнения счёта через СБП. Deprecated, будет выведен из эксплуатации; не забыть сделать описание сервисов замены.

### cnets.api.payments.sbp
...

### cnets.internal.payments.sbp
... (/api/v1/sbp/internal/createPayment)

### cnets.api.info.bic.out
Предоставляет API для поиска банковских реквизитов по БИК.

Зависимости: CBR

## 3. ЭДО

### cnets.api.edo.crypto-keys
Предоставляет API для подписания документов усиленной электронной подписью.

Зависимости: ITI_Integration, SimpleSign, CryptoKeys

### cnets.api.edo.unsigned-docs
Предоставляет API для получения списка документов, их содержания и подписания простой электронной подписью.

Зависимости: ITI_Integration, trade, RMS, SimpleSign, SmsService, FileStorage

### mx.cnets.rms.unsigned-docs.in
Выполняет синхронизацию документов RMS->>CNETS.

Зависимости: ITI_Integration, RMS

### cnets.api.document.template.out
Предоставляет API для получения шаблонов документов и генерации документов через Битрикс.

Зависимости: ITI_Integration, trade, RMS, SimpleSign, Bitrix напрямую для получения шаблона документа; для генерации документа через ESB
