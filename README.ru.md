<div align="center">

```

███╗   ███╗ ██████╗ ██╗  ██╗██╗███╗   ██╗
████╗ ████║██╔═══██╗╚██╗██╔╝██║████╗  ██║
██╔████╔██║██║   ██║ ╚███╔╝ ██║██╔██╗ ██║
██║╚██╔╝██║██║   ██║ ██╔██╗ ██║██║╚██╗██║
██║ ╚═╝ ██║╚██████╔╝██╔╝ ██╗██║██║ ╚████║
╚═╝     ╚═╝ ╚═════╝ ╚═╝  ╚═╝╚═╝╚═╝  ╚═══╝
```

# www-moxin-studio-bulk-apikey

**Массовое создание аккаунтов и ключей Moxin Studio (new-api) — инференс, совместимый с OpenAI, на https://www.moxin.studio/v1, финансируемый за счёт ежедневной отметки в панели.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-143%20served-412991?style=for-the-badge)](https://www.moxin.studio/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Быстрый старт](#-quick-start) · [🎁 Бесплатный тариф](#-free-tier--how-much-you-get) · [🤖 Модели](#-models--all-143-of-them) · [🧪 Использование API](#-how-to-use-the-api) · [✅ Проверено](#-verified-working--every-key-is-tested) · [🧭 Как это работает](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [🔌 OmniRoute](#-connect-to-omniroute) · [⚙️ Опции](#️-cli-options) · [🛰️ Прокси](#️-behind-a-proxy) · [🐞 Проблемы](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Зачем это нужно

Moxin Studio — это форк new-api (`v1.0.0-rc.45`, `system_name "studio"`) с **полным отсутствием капчи** (`turnstile_check=false`) и регистрацией по коду из письма — но **без бонуса за регистрацию**. Ценность аккаунта заключается в **ежедневной отметке**: `POST /api/user/checkin` начислил `{"quota_awarded":54438}` 2026-10-21 (≈ **$0.109** при `quota_per_unit 500000`), так что один аккаунт стоит одного дня кредита и продолжает приносить доход, если отмечаться ежедневно. За этим стоит каталог из **143 моделей** (DeepSeek · Gemini · Claude · GPT · GLM · Qwen · MiniMax … в виде строк с тегами групп, таких как `[次]deepseek-v4.1-flash`), обслуживаемый с совместимостью с OpenAI на `https://www.moxin.studio/v1`. Его родственный хост `www.moxin.plus` («墨信AI», `rc.44`) — это **отдельная база данных**: ключ, созданный здесь, отвечает `Invalid token` там.

## 🎁 Бесплатный тариф — сколько вы получаете

| Что | Значение |
| --- | --- |
| Бонус за регистрацию | **нет** — новый аккаунт начинается с `quota 0` (измерено дважды: аккаунт от 2026-10-18 и сегодняшние новые) |
| Ежедневная отметка | **случайная для каждого аккаунта каждый день, наблюдалось 12 275 … 79 697 quota (≈ $0.025–$0.16; медиана ~54 000 ≈ $0.11)** (`POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}`); повторный вызов в тот же день отвечает `Already checked in today` |
| Каталог | 143 строки, **0 строго бесплатных** (`model_ratio 0` + `model_price 0`): всё платное, кредит за отметку — это то, что вы тратите |
| Капча | **отсутствует** (`turnstile_check=false`) — обычный HTTP, без браузера, без решателя |
| Эл. почта | `email_verification=true`, код действителен 10 мин; на практике работают **только адреса класса Gmail** (публичный пул gmail) |
| Группы | `default` (ratio 1) · `DeepSeek[羊毛] 0.35` · `Codex 0.2` · `Claude[特价] 0.6` · `Claude[折扣] 1.2` · `Claude[高质] 2` · `GPT[官方] 5` · `Gemini…` |
| Лимит/срок действия | опубликованного лимита на ключ нет; кредит за отметку не истекает |

> 💡 Бесплатный тариф устанавливается Moxin Studio и может измениться. Проверяйте актуальные цифры на [www.moxin.studio](https://www.moxin.studio) — и пусть решение принимает собственный шлюз инструмента:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Модели — все 143

`GET /v1/models` вернул **108 id** на живом ключе (2026-10-21). Id каталога помечены тегами групп (`[次]deepseek-v4.1-flash`, `[官方]gemini-…`, `[vertex特价]gemini-4-argon`), и **id без префикса также разрешается**; эти были проверены чатом от начала до конца:

| Model ID | примечание |
| --- | --- |
| `deepseek-v4.1-flash` | значение по умолчанию для проверки — 200 `"MX-PONG"` с трёх независимых выходов |

Лента `/api/pricing` перечисляет 143 строки: `deepseek-v4-flash`, `gemini-2.5-flash`, `glm-4.6`, `gpt-4o-mini`, `claude-haiku-4-5`, `qwen3.8-flash`, `MiniMax-M3`, `kimi-k3`, `grok-4.6` и другие, каждая в одной или нескольких из вышеперечисленных групп.

## 🧪 Как использовать API

Базовый URL **`https://www.moxin.studio/v1`** — совместим с OpenAI:

```bash
curl --fail-with-body https://www.moxin.studio/v1/chat/completions \
  -H "Authorization: Bearer $MOXIN_STUDIO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"deepseek-v4.1-flash","messages":[{"role":"user","content":"Reply with the single word: ok"}]}'
```

```python
from openai import OpenAI
c = OpenAI(base_url="https://www.moxin.studio/v1", api_key="<your key>")
print(c.chat.completions.create(model="deepseek-v4.1-flash",
      messages=[{"role": "user", "content": "hi"}]).choices[0].message.content)
```

Пополните ключ, отметившись (токен сессии, а не API-ключ): `POST /api/user/checkin` с `Authorization: Bearer <access_token>`.

## 🛠 Быстрый старт

```bash
cd www-moxin-studio-bulk-apikey
node public-cli.js --count 3        # three accounts: e-mail code -> check-in -> key -> chat-200
node public-cli.js --verify         # re-test every key in keys.txt
node public-cli.js --pool proxies.txt   # optional: one exit per account
```

Без браузера, без решателя капчи, без npm install. `keys.txt` записывается только после **реального завершения**, а `accounts.jsonl` хранит доказательства отметки по каждому аккаунту (`quotaBefore` → `quotaAwarded` → `quota`).

### 🖱️ Лаунчер

## ✅ Проверено в работе — каждый ключ протестирован

Все проверки ниже были повторно выполнены **2026-10-21** из пула выходов (без WARP):

| Проверка | Результат |
| --- | --- |
| `GET /api/status` | **200** — `register_enabled`+`password_register_enabled` true, `email_verification=true`, **`turnstile_check=false`**, `checkin_enabled=true`, прайсинг из 143 строк |
| регистрация | **✔** `GET /api/verification?email=<gmail>` 200 → код → `POST /api/user/register {username,password,password2,email,verification_code}` 200 |
| финансирование отметкой | **✔** `POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}` ⇒ `quota 0 → 54 438` |
| ключ | **✔** `POST /api/token/` → `GET /api/token/?p=0&size=20` → **`POST /api/token/{id}/key`** раскрывает полный ключ |
| живой чат | **✔** `deepseek-v4.1-flash` → 200 `"MX-PONG"`, повторно доказано с **трёх независимых выходов** (`http://51.15.20.32:3129`, `socks5://183.106.215.208:1080`, `http://3.224.130.91:3128`) |
| масштаб | **✔ 10 аккаунтов создано, каждый ключ отвечает на реальное завершение** (`deepseek-v4.1-flash` → 200 `"ok"`), один выход на аккаунт; наблюдаемые награды за отметку: `12 275 … 79 697` quota (≈ $0.025–$0.16), т.е. **ежедневный кредит рандомизирован** |

> **Контроль качества.** Каждый ключ здесь прошёл живой `chat-200` прежде чем был записан в `keys.txt`
> (мёртвые ключи никогда не поставляются). Комбинации роутера следуют тому же правилу на уровне модели: запись
> добавляется **только если модель отвечает `chat-200` и умеет вызывать инструменты** — модель без поддержки инструментов,
> мёртвый ключ или мёртвый провайдер отбрасывается, а не добавляется. Повторяйте шаг проверки в любое время;
> сбои сообщаются, никогда не скрываются.

## 🧭 Как это работает

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: Moxin Studio page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://www.moxin.studio/v1/chat/completions → real content = the key is live
```

| Шаг | Вызов | Примечания |
| --- | --- | --- |
| почтовый ящик | публичный пул Gmail | панель принимает адреса класса Gmail; **переработанный** ящик может уже существовать на панели (`Email address is already in use`) — CLI повторяет попытку со свежим ящиком |
| код | `GET /api/verification?email=…` затем чтение ящика | ⚠ **фильтр бренда должен опираться на ОТПРАВИТЕЛЯ** (`moxin|studio|墨信`): общие ящики также содержат коды других панелей (`OpenArt.ai`, `WhatsToken.AI` …) и общая тема «verification code» не является доказательством — оба случая давали `Verification code is incorrect or has expired` до того, как фильтр был ужесточён |
| регистрация | `POST /api/user/register {username,password,password2,email,verification_code}` | ответ может содержать собственный `access_token`; иначе `POST /api/user/login {username,password}` (подходит и имя пользователя, и эл. почта) |
| финансирование | `POST /api/user/checkin` | весь смысл: `quota_awarded ≈ 54 438` на аккаунт в день |
| ключ | `POST /api/token/` → список → `POST /api/token/{id}/key` | этот форк маскирует ключ при каждом чтении, маршрут `…/{id}/key` раскрывает его |
| шлюз | `POST /v1/chat/completions` | ключ записывается в `keys.txt` только после возврата реального содержимого |

## ⚙️ Опции CLI

```bash
node public-cli.js --count 5                    # five accounts, serial
node public-cli.js --model deepseek-v4-flash    # model used for the live gate
node public-cli.js --alt-models glm-4.6,gpt-4o-mini
node public-cli.js --code-wait 480000           # the panel's SMTP can be slow (code TTL 10 min)
node public-cli.js --timeout 25000              # per-request timeout
node public-cli.js --verify                     # re-check keys.txt
node public-cli.js --pool proxies.txt           # exit per account
node public-cli.js --connect-9router --router9 http://localhost:20128 --password 123456
node public-cli.js --help
```

## 🛰️ За прокси

**Необязательно.** `--pool <file>` (или `MOXIN_STUDIO_PROXIES`) принимает по одному выходу на строку — `http://host:port`, `https://host:port` (CONNECT) или `socks5://host:port` (с необязательным `user:pass@`) — и CLI берёт один выход на аккаунт; без пула он работает напрямую. Встроенная обвязка — это ~80 строк `net`/`tls`, без библиотек.

## 📄 Файлы

| Путь | Содержимое |
| --- | --- |
| `keys.txt` | один проверенный в работе ключ на строку |
| `accounts.jsonl` | эл. почта, ключ, доказательства отметки (`quotaBefore` / `quotaAwarded` / `quota`), модель, выход |
| `proxies.txt` | ваши выходы, необязательно |
| `out/` | записи о запусках по каждой попытке |

## 🔌 Подключение к 9Router

[9Router](https://9router.com) — это самостоятельно размещаемый роутер, совместимый с OpenAI:

```bash
node public-cli.js --connect-9router --router9 http://localhost:20128 --password 123456
```

По умолчанию: префикс `moxin-studio`, база `https://www.moxin.studio/v1`, модель `deepseek-v4.1-flash`; модели обслуживаются как `moxin-studio/<model id>`.

## 🔌 Подключение к OmniRoute

[OmniRoute](https://omniroute.online) — это самостоятельно размещаемый роутер, совместимый с OpenAI:

```bash
node public-cli.js --connect-omniroute --router9 http://localhost:20128 --password 123456
```

По умолчанию: префикс `moxin-studio`, база `https://www.moxin.studio/v1`, модель `deepseek-v4.1-flash`; модели обслуживаются как `moxin-studio/<model id>`.

## 🔑 Что даёт один ключ

| | |
| --- | --- |
| 🏢 Провайдер | **Moxin Studio** — [www.moxin.studio](https://www.moxin.studio) |
| 🎁 Что даёт провайдер | **нет бонуса за регистрацию** — ежедневная отметка платит **≈$0.109 на аккаунт в день** |
| 🔑 Что даёт один ключ | каталог из 143 строк на `https://www.moxin.studio/v1` (108 id видно на живом ключе) |
| 📦 Этот репозиторий | **каждый ключ, все проверены чатом** 2026-10-21 (один выход на аккаунт) |
| 🌐 Форма API | совместим с OpenAI: `POST https://www.moxin.studio/v1/chat/completions` |
| 🛡️ Шлюзы | **без капчи**, только код из письма; на общих ящиках требуется фильтр по бренду отправителя |

## 🐞 Проблемы

Если панель изменит свои маршруты регистрации/отметки, свою политику эл. почты или каталог, инструмент остановится там и сообщит об этом. **[Откройте issue](../../issues)** с командой и точным выводом.

## 📄 Лицензия

MIT © 2026 — см. [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 www-moxin-studio-bulk-apikey contributors

</div>