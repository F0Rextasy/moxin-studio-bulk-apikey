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

**Crea cuentas y claves de Moxin Studio (new-api) en masa — inferencia compatible con OpenAI en https://www.moxin.studio/v1, financiada por el check-in diario del panel.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-143%20served-412991?style=for-the-badge)](https://www.moxin.studio/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Inicio rápido](#-quick-start) · [🎁 Nivel gratuito](#-free-tier--how-much-you-get) · [🤖 Modelos](#-models--all-143-of-them) · [🧪 Usa la API](#-how-to-use-the-api) · [✅ Verificado](#-verified-working--every-key-is-tested) · [🧭 Cómo funciona](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Opciones](#️-cli-options) · [🛰️ Proxies](#️-behind-a-proxy) · [🐞 Problemas](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Por qué existe esto

Moxin Studio es un fork de new-api (`v1.0.0-rc.45`, `system_name "studio"`) **sin captcha alguno** (`turnstile_check=false`) y con registro mediante código de correo electrónico — pero **sin bono de registro**. Lo que hace que valga la pena tener una cuenta es el **check-in diario**: `POST /api/user/checkin` pagó `{"quota_awarded":54438}` el 2026-10-21 (≈ **$0.109** a `quota_per_unit 500000`), así que una cuenta vale el crédito de un día y sigue pagando si haces check-in a diario. El catálogo detrás es de **143 modelos** (DeepSeek · Gemini · Claude · GPT · GLM · Qwen · MiniMax … como filas etiquetadas por grupo como `[次]deepseek-v4.1-flash`), servidos de forma compatible con OpenAI en `https://www.moxin.studio/v1`. Su host hermano `www.moxin.plus` ("墨信AI", `rc.44`) es una **base de datos separada** — una clave creada aquí responde `Invalid token` allí.

## 🎁 Nivel gratuito — cuánto obtienes

| Qué | Valor |
| --- | --- |
| Bono de registro | **ninguno** — una cuenta nueva empieza en `quota 0` (medido dos veces: cuenta del 2026-10-18 y las nuevas de hoy) |
| Check-in diario | **aleatorio por cuenta y por día, observado 12 275 … 79 697 quota (≈ $0.025–$0.16; mediana ~54 000 ≈ $0.11)** (`POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}`); una segunda llamada el mismo día responde `Already checked in today` |
| Catálogo | 143 filas, **0 estrictamente gratuitas** (`model_ratio 0` + `model_price 0`): todo es de pago, el crédito del check-in es lo que gastas |
| Captcha | **ninguno** (`turnstile_check=false`) — HTTP simple, sin navegador, sin solucionador |
| Correo electrónico | `email_verification=true`, código válido 10 min; **solo direcciones tipo Gmail** funcionan en la práctica (el pool público de gmail) |
| Grupos | `default` (ratio 1) · `DeepSeek[羊毛] 0.35` · `Codex 0.2` · `Claude[特价] 0.6` · `Claude[折扣] 1.2` · `Claude[高质] 2` · `GPT[官方] 5` · `Gemini…` |
| Límite/caducidad | sin límite publicado por clave; el crédito del check-in no caduca |

> 💡 El nivel gratuito lo define Moxin Studio y puede cambiar. Consulta los números en vivo en [www.moxin.studio](https://www.moxin.studio) — y deja que la propia puerta de la herramienta decida:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modelos — los 143

`GET /v1/models` respondió **108 ids** con una clave en vivo (2026-10-21). Los ids del catálogo están etiquetados por grupo (`[次]deepseek-v4.1-flash`, `[官方]gemini-…`, `[vertex特价]gemini-4-argon`) y el **id sin prefijo también resuelve**; estos fueron verificados de extremo a extremo por chat:

| Model ID | nota |
| --- | --- |
| `deepseek-v4.1-flash` | el predeterminado de verificación — 200 `"MX-PONG"` desde tres salidas independientes |

El feed `/api/pricing` lista 143 filas: `deepseek-v4-flash`, `gemini-2.5-flash`, `glm-4.6`, `gpt-4o-mini`, `claude-haiku-4-5`, `qwen3.8-flash`, `MiniMax-M3`, `kimi-k3`, `grok-4.6` y más, cada uno en uno o más de los grupos anteriores.

## 🧪 Cómo usar la API

URL base **`https://www.moxin.studio/v1`** — compatible con OpenAI:

```bash
curl --fail-with-body https://www.moxin.studio/v1/chat/completions \
  -H "Authorization: Bearer $MOXIN_STUDIO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"deepseek-v4.1-flash","messages":[{"role":"user","content":"Reply with the single word: ok"}]}'
```

```python
from openai import OpenAI
c = OpenAI(base_url="https://www.moxin.studio/v1", api_key="YOUR_KEY")
print(c.chat.completions.create(model="deepseek-v4.1-flash",
      messages=[{"role": "user", "content": "hi"}]).choices[0].message.content)
```

Recarga la clave haciendo check-in (token de sesión, no la clave de API): `POST /api/user/checkin` con `Authorization: Bearer <access_token>`.

## 🛠 Inicio rápido

```bash
cd www-moxin-studio-bulk-apikey
node public-cli.js --count 3        # three accounts: e-mail code -> check-in -> key -> chat-200
node public-cli.js --verify         # re-test every key in keys.txt
node public-cli.js --pool proxies.txt   # optional: one exit per account
```

Sin navegador, sin solucionador de captcha, sin npm install. `keys.txt` se escribe solo tras una **completación real**, y `accounts.jsonl` guarda la evidencia del check-in por cuenta (`quotaBefore` → `quotaAwarded` → `quota`).

### 🖱️ El lanzador

## ✅ Verificado funcionando — cada clave se prueba

Todas las comprobaciones siguientes se volvieron a ejecutar el **2026-10-21** desde el pool de salidas (sin WARP):

| Comprobación | Resultado |
| --- | --- |
| `GET /api/status` | **200** — `register_enabled`+`password_register_enabled` true, `email_verification=true`, **`turnstile_check=false`**, `checkin_enabled=true`, pricing de 143 filas |
| registro | **✔** `GET /api/verification?email=<gmail>` 200 → código → `POST /api/user/register {username,password,password2,email,verification_code}` 200 |
| financiación por check-in | **✔** `POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}` ⇒ `quota 0 → 54 438` |
| clave | **✔** `POST /api/token/` → `GET /api/token/?p=0&size=20` → **`POST /api/token/{id}/key`** revela la clave completa |
| chat en vivo | **✔** `deepseek-v4.1-flash` → 200 `"MX-PONG"`, reprobado desde **tres salidas independientes** (`http://51.15.20.32:3129`, `socks5://183.106.215.208:1080`, `http://3.224.130.91:3128`) |
| escala | **✔ 10 cuentas creadas, 10/10 claves responden a una completación real** (`deepseek-v4.1-flash` → 200 `"ok"`), una salida por cuenta; premios de check-in vistos: `12 275 … 79 697` quota (≈ $0.025–$0.16), es decir, **el crédito diario es aleatorio** |

## 🧭 Cómo funciona

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: Moxin Studio page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://www.moxin.studio/v1/chat/completions → real content = the key is live
```

| Paso | Llamada | Notas |
| --- | --- | --- |
| buzón | pool público de Gmail | el panel acepta direcciones tipo Gmail; un buzón **reciclado** puede existir ya en el panel (`Email address is already in use`) — el CLI reintenta con un buzón nuevo |
| código | `GET /api/verification?email=…` y luego leer el buzón | ⚠ **el filtro de marca debe basarse en el REMITENTE** (`moxin|studio|墨信`): los buzones compartidos también llevan códigos de otros paneles (`OpenArt.ai`, `WhatsToken.AI` …) y un asunto genérico de "verification code" no es evidencia — ambos produjeron `Verification code is incorrect or has expired` antes de ajustar el filtro |
| registro | `POST /api/user/register {username,password,password2,email,verification_code}` | la respuesta puede traer su propio `access_token`; si no, `POST /api/user/login {username,password}` (funcionan tanto el nombre de usuario como el correo) |
| financiar | `POST /api/user/checkin` | el objetivo: `quota_awarded ≈ 54 438` por cuenta y día |
| clave | `POST /api/token/` → listar → `POST /api/token/{id}/key` | este fork enmascara la clave en cada lectura, la ruta `…/{id}/key` la revela |
| puerta | `POST /v1/chat/completions` | una clave se escribe en `keys.txt` solo tras recibir contenido real |

## ⚙️ Opciones del CLI

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

## 🛰️ Detrás de un proxy

**Opcional.** `--pool <file>` (o `MOXIN_STUDIO_PROXIES`) acepta una salida por línea — `http://host:port`, `https://host:port` (CONNECT) o `socks5://host:port` (con `user:pass@` opcional) — y el CLI toma una salida por cuenta; sin un pool se ejecuta directo. La plomería integrada son ~80 líneas de `net`/`tls`, sin librería.

## 📄 Archivos

| Ruta | Contenido |
| --- | --- |
| `keys.txt` | una clave verificada en vivo por línea |
| `accounts.jsonl` | correo, clave, evidencia de check-in (`quotaBefore` / `quotaAwarded` / `quota`), modelo, salida |
| `proxies.txt` | tus salidas, opcional |
| `out/` | registros de ejecución por intento |

## 🔌 Conectar con 9Router

[9Router](https://9router.com) es un router autoalojado y compatible con OpenAI:

```bash
node public-cli.js --connect-9router --router9 http://localhost:20128 --password 123456
```

Valores predeterminados: prefijo `moxin-studio`, base `https://www.moxin.studio/v1`, modelo `deepseek-v4.1-flash`; los modelos se sirven como `moxin-studio/<model id>`.

## 🔌 Conectar con OmniRoute

[OmniRoute](https://omniroute.online) es un router autoalojado y compatible con OpenAI:

```bash
node public-cli.js --connect-omniroute --router9 http://localhost:20128 --password 123456
```

Valores predeterminados: prefijo `moxin-studio`, base `https://www.moxin.studio/v1`, modelo `deepseek-v4.1-flash`; los modelos se sirven como `moxin-studio/<model id>`.
## 🔑 Qué te da una clave

| | |
| --- | --- |
| 🏢 Proveedor | **Moxin Studio** — [www.moxin.studio](https://www.moxin.studio) |
| 🎁 Qué da el proveedor | **sin bono de registro** — el check-in diario paga **≈$0.109 por cuenta y día** |
| 🔑 Qué te da una clave | el catálogo de 143 filas en `https://www.moxin.studio/v1` (108 ids visibles con una clave en vivo) |
| 📦 Este repo | **10 claves, todas verificadas por chat** el 2026-10-21 (una salida por cuenta) |
| 🌐 Forma de la API | compatible con OpenAI: `POST https://www.moxin.studio/v1/chat/completions` |
| 🛡️ Puertas | **sin captcha**, solo código de correo; se requiere filtro de marca del remitente en los buzones compartidos |

## 🐞 Problemas

Si el panel cambia sus rutas de registro/check-in, su política de correo o el catálogo, la herramienta se detiene ahí y lo indica. **[Abre un issue](../../issues)** con el comando y la salida exacta.

## 📄 Licencia

MIT © 2026 — consulta [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 www-moxin-studio-bulk-apikey contributors

</div>

---

## ⚠️ Riesgo de cuenta y cómo minimizarlo
