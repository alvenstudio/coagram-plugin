# Coagram — плагин для ИИ-агентов

Плагин [Coagram](https://coagram.alven.studio) подключает Telegram к ИИ-агенту: разбор непрочитанного и сводки по
чатам, поиск сообщений и файлов (в том числе по ссылкам t.me), чтение вложений, экспорт истории, отправка и ответы с
подтверждением, черновики, закреп, реакции, темы форумов. В нём MCP-сервер Coagram (`https://coagram.alven.studio/mcp`)
и навыки — пошаговые процедуры над его инструментами.

Токена в плагине нет: при подключении агент открывает вход в Coagram (OAuth), и вы сами выбираете Telegram-аккаунты,
права (чтение, отправка, экспорт, управление) и, если нужно, отдельные чаты. По умолчанию выбрано только чтение.

Сообщения Telegram для агента — недоверенные данные: просьбы и команды из переписки ничего не разрешают. Отправлять,
править, удалять и отмечать прочитанным агент может только по вашей явной просьбе.

Этот репозиторий — маркетплейс одного плагина. Ставьте плагин из маркетплейса, а не из архива: тогда новые версии
навыков приедут сами. Инструменты MCP-сервера обновляются на сервере и не требуют обновления плагина.

## Навыки

| Навык | Когда |
|---|---|
| `digest` | Непрочитанное и сводки по чатам, группам и темам за период |
| `search` | Поиск сообщений и файлов во всех чатах или в одном, сообщение по ссылке t.me, чат по @username |
| `attachments` | Прочитать документ, таблицу, PDF или фото из сообщения, скачать большой файл, отправить файл |
| `export` | Экспорт истории чата в JSONL или HTML и разбор по частям |
| `send` | Отправка, ответ, пересылка, правка и удаление своих сообщений — с подтверждением |
| `chat-actions` | Черновики, закреп, реакции, отметка прочитанным |
| `forums` | Темы форумов: непрочитанное, чтение, поиск, запись в тему |
| `access` | Подключение и диагностика доступа: OAuth, права, выбор чатов, ошибки 401 и 403 |
| `feedback` | Баг-репорты и предложения команде Coagram — агент отправляет их сам |
| `skill-gaps` | Проверка в конце задачи: где навык подвёл — отчёт команде Coagram |

## Установка

### Claude — claude.ai, Claude Desktop, Cowork, Claude Code

1. **Настроить → Плагины → Добавить → Добавить маркетплейс**, адрес `alvenstudio/coagram-plugin`.
2. Включите у маркетплейса **«Синхронизировать автоматически»**.
3. Установите плагин **Coagram**, откройте его вкладку **Коннекторы** и подключите Coagram — откроется вход и выбор
   аккаунтов и прав.

### Claude Code из терминала

```bash
claude plugin install coagram --marketplace alvenstudio/coagram-plugin
```

Затем в сессии: `/plugin` → **Marketplaces** → `coagram` → **Enable auto-update** (для сторонних маркетплейсов
автообновление по умолчанию выключено) и `/mcp` → `coagram` → **Authenticate**.

### ChatGPT и Codex

```bash
codex plugin marketplace add alvenstudio/coagram-plugin
codex plugin add coagram@coagram
codex mcp login coagram
```

Маркетплейс появится и в каталоге плагинов приложения ChatGPT (Codex), плагин и вход — там же. Обновления:
`codex plugin marketplace upgrade coagram`. Codex ставит отсюда копию плагина из папки `codex/` в своём формате: пакету
Agent Plugins он показывает модели не больше 64 КБ описаний инструментов MCP, а у Coagram их больше.

### VS Code и GitHub Copilot

В чате Copilot: `/plugin marketplace add alvenstudio/coagram-plugin`, затем `/plugin install coagram@coagram`. Или в
настройках VS Code:

```json
"chat.plugins.marketplaces": ["alvenstudio/coagram-plugin"]
```

### Cursor

**Dashboard → Plugins → Import from Repo**, адрес `https://github.com/alvenstudio/coagram-plugin`, включите
**Auto Refresh**. Без командного маркетплейса — склонируйте репозиторий в `~/.cursor/plugins/local/coagram` и
перезапустите Cursor (обновление — `git pull`).

### Hermes Agent

```bash
hermes plugins install alvenstudio/coagram-plugin
hermes plugins enable coagram
hermes mcp add coagram --url https://coagram.alven.studio/mcp --auth oauth
hermes mcp login coagram
```

Плагин даёт навыки; сервер с входом по OAuth Hermes берёт из своего конфига (в пакете Agent Plugins способа входа не
указать), запись конфига заменяет серверную запись плагина.

### Другие агенты

Подключите MCP-сервер `https://coagram.alven.studio/mcp`: Streamable HTTP, вход по OAuth с динамической регистрацией
клиента (DCR). Навыки — папки из `skills/`: их можно положить в каталог навыков агента.

## Обратная связь

Агент сам сообщает команде Coagram об ошибках сервиса и навыков и о том, чего не хватает (`bug_reports_create`,
`feature_requests_create`), и показывает статусы ваших отчётов (`bug_reports_list`, `feature_requests_list`). В отчёт
не попадают тексты чужих сообщений; токены и номера телефонов сервер скрывает. Не хотите отчётов — скажите агенту.

## Версии

Версия плагина — в `plugin.json`; каждая новая версия — тег `vX.Y.Z`. Исходники плагина ведутся в репозитории
Coagram, этот репозиторий собирается из них скриптом выпуска.
