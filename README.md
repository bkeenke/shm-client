# SHM Client

Клиентский личный кабинет для SHM (Service Hosting Manager).

- 🔐 Аутентификация Логин/Пароль, Логин/Пароль + 2FA, Passkey, Telegram widget, Telegram MiniApp
- 📦 Покупка услуг, возможность остановить и удалить услугу
- 🔗 Показ QR-кода и ссылки на подписку(Remnawave/marzban)
- 💳 Пополнение баланса, удаление автоплатежа
- 💸 Прогноз оплаты в профиле
- 📊 История платежей и списаний
- 👤 Редактирование профиля
- 🌐 Мультиязычность (Русский / English)

### Docker Compose

- Вместе с контейнерами SHM

```yaml
services:
#   admin:
#     ...
  client:
    image: bkeenke/shm-client:latest
    pull_policy: always
    restart: always
    ports:
      - "8082:80"
    environment:
      - SHM_URL=http://api
      - APP_NAME=My Service
    depends_on:
      - api
#   mysql:
#     ....
```

### Переменные

| Переменная | Описание | По умолчанию |
|------------|----------|--------------|
| `SHM_URL` | URL API сервера SHM | - |
| `SHM_HOST` | Альтернатива SHM_URL | - |
| `SHM_BASE_PATH` | Базовый путь (например `/cabinet`) | `/` |
| `APP_NAME` | Название приложения | `SHM Client` |
| `TELEGRAM_BOT_NAME` | Username Telegram бота (без @) s| - |
| `TELEGRAM_BOT_AUTH_ENABLE` | Включить авторизацию через Telegram виджет | `false` |
| `TELEGRAM_WEBAPP_AUTH_ENABLE` | Авторизация через телеграмм вебапп | `false` |
| `TELEGRAM_WEBAPP_PROFILE` | Название бота (профиля) в SHM | - |
| `SUPPORT_LINK` | Ссылка на поддержку | - |
| `OTP_ENABLE` | Показать настройки OTP | `true` |
| `PASSKEY_ENABLE` | Показать настройки Passkey | `true` |
| `BITRIX_WIDGET_SCRIPT_URL` | URL виждета Битрих-24 (https://cdn-ru.bitrix24.ru/b********/crm/site_button/loader_****.js)| - |
| `PROXY_CATEGORY` | Категория прокси чтобы показать ссылку на подписку (vpn-remna) | - |
| `PROXY_STORAGE_PREFIX` | префикс для категории proxy в хранилище, например 'vpm_remna_' | 'vpm_mrzb_' |
| `VPN_CATEGORY` | Категория VPN чтобы показать QR или возможность скачать файл конфигурации (vpn-wg) | - |
| `VPN_STORAGE_PREFIX` | Префикс для категории vpn в хранилище например 'wg_key_' | 'vpn' |
| `VISIBLE_CATEGORIES` | Категории для отображения при покупке и уже купленных услуг (vpn-mz,vpm-mz-trial)| - |
| `EMAIL_REQUIRED` | Hе дает пользоваться ЛК пока клиент не введет email | false |

### Telegram Widget
Для работы с авторизацией через Telegram Widget нужно в астройках бота  который указан в `TELGRAM_BOT_NAME` указать домен на котором расположена ваше приложение `shm-client`

## Категории услуг для VPN/Proxy

Для отображения **QR-кода** и **ссылки подписки** в деталях услуги, категория услуги должна соответствовать одному из следующих паттернов:

### VPN (WireGuard конфигурация)

Категория должна **начинаться** с одного из значений:
- `vpn`
- `wg`
- `awg`

Примеры валидных категорий: `vpn`, `vpn-wg`, `vpn-awg-nl`, `awg-premium`, `wg-fast`

**Данные в хранилище:** `vpn{user_service_id}` (например: `vpn123`)

### Proxy (Marzban/Remnawave подписка)

Категория должна содержать одно из слов:
- `remna`
- `remnawave`
- `marzban`
- `marz`
- `mz`

Примеры валидных категорий: `remnawave`, `vpn-remna`, `vpm-mz`, `vpm-marz`, `marzban`, `mz-premium`, `proxy-marz`

**Данные в хранилище:**
- `vpn_mrzb_{user_service_id}` (например: `vpn_mrzb_123`)
- `vpn_remna_{user_service_id}` (например: `vpn_remna_123`)

### Прочие категории

Следующие категории отображаются как есть (без QR/ссылки):
- `web_tariff` — Тарифы хостинга
- `web` — Web хостинг
- `mysql` — Базы данных
- `mail` — Почта
- `hosting` — Хостинг

Все остальные категории группируются как "Прочее".

## Лицензия

Apache License 2.0
