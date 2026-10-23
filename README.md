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

**Bulk-create Moxin Studio (new-api) accounts and keys — OpenAI-compatible inference on https://www.moxin.studio/v1, funded by the panel's daily check-in.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-143%20served-412991?style=for-the-badge)](https://www.moxin.studio/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Quick start](#-quick-start) · [🎁 Free tier](#-free-tier--how-much-you-get) · [🤖 Models](#-models--all-143-of-them) · [🧪 Use the API](#-how-to-use-the-api) · [✅ Verified](#-verified-working--every-key-is-tested) · [🧭 How it works](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [🔌 OmniRoute](#-connect-to-omniroute) · [⚙️ Options](#️-cli-options) · [🛰️ Proxies](#️-behind-a-proxy) · [🐞 Issues](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Why this exists

Moxin Studio is a new-api fork (`v1.0.0-rc.45`, `system_name "studio"`) with **no captcha at all** (`turnstile_check=false`) and an e-mail-code sign-up — but **no sign-up bonus**. What makes an account worth having is the **daily check-in**: `POST /api/user/checkin` paid `{"quota_awarded":54438}` on 2026-10-21 (≈ **$0.109** at `quota_per_unit 500000`), so one account is worth one day's credit and keeps paying if you check in daily. The catalogue behind it is **143 models** (DeepSeek · Gemini · Claude · GPT · GLM · Qwen · MiniMax … as group-tagged rows such as `[次]deepseek-v4.1-flash`), served OpenAI-compatible on `https://www.moxin.studio/v1`. Its sibling host `www.moxin.plus` ("墨信AI", `rc.44`) is a **separate database** — a key minted here answers `Invalid token` there.

## 🎁 Free tier — how much you get

| What | Value |
| --- | --- |
| Sign-up bonus | **none** — a fresh account starts at `quota 0` (measured twice: 2026-10-18 account and today's fresh ones) |
| Daily check-in | **randomised per account per day, observed 12 275 … 79 697 quota (≈ $0.025–$0.16; median ~54 000 ≈ $0.11)** (`POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}`); a second call the same day answers `Already checked in today` |
| Catalogue | 143 rows, **0 strict-free** (`model_ratio 0` + `model_price 0`): everything is paid, the check-in credit is what you spend |
| Captcha | **none** (`turnstile_check=false`) — plain HTTP, no browser, no solver |
| E-mail | `email_verification=true`, code valid 10 min; **only Gmail-class addresses** work in practice (the public gmail pool) |
| Groups | `default` (ratio 1) · `DeepSeek[羊毛] 0.35` · `Codex 0.2` · `Claude[特价] 0.6` · `Claude[折扣] 1.2` · `Claude[高质] 2` · `GPT[官方] 5` · `Gemini…` |
| Rate/expiry | no published per-key limit; the check-in credit does not expire |

> 💡 The free tier is set by Moxin Studio and can change. Check the live numbers at [www.moxin.studio](https://www.moxin.studio) — and let the tool's own gate decide:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Models — all 143 of them

`GET /v1/models` answered **108 ids** on a live key (2026-10-21). Catalog ids are group-tagged (`[次]deepseek-v4.1-flash`, `[官方]gemini-…`, `[vertex特价]gemini-4-argon`) and the **unprefixed id also resolves**; these were chat-verified end to end:

| Model ID | note |
| --- | --- |
| `deepseek-v4.1-flash` | the verification default — 200 `"MX-PONG"` from three independent exits |

The `/api/pricing` feed lists 143 rows: `deepseek-v4-flash`, `gemini-2.5-flash`, `glm-4.6`, `gpt-4o-mini`, `claude-haiku-4-5`, `qwen3.8-flash`, `MiniMax-M3`, `kimi-k3`, `grok-4.6` and more, each in one or more of the groups above.

## 🧪 How to use the API

Base URL **`https://www.moxin.studio/v1`** — OpenAI-compatible:

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

Top the key up by checking in (session token, not the API key): `POST /api/user/checkin` with `Authorization: Bearer <access_token>`.

## 🛠 Quick start

```bash
cd www-moxin-studio-bulk-apikey
node public-cli.js --count 3        # three accounts: e-mail code -> check-in -> key -> chat-200
node public-cli.js --verify         # re-test every key in keys.txt
node public-cli.js --pool proxies.txt   # optional: one exit per account
```

No browser, no captcha solver, no npm install. `keys.txt` is written only after a **real completion**, and `accounts.jsonl` keeps the per-account check-in evidence (`quotaBefore` → `quotaAwarded` → `quota`).

### 🖱️ The launcher

## ✅ Verified working — every key is tested

All checks below were re-run on **2026-10-21** from the exit pool (no WARP):

| Check | Result |
| --- | --- |
| `GET /api/status` | **200** — `register_enabled`+`password_register_enabled` true, `email_verification=true`, **`turnstile_check=false`**, `checkin_enabled=true`, 143-row pricing |
| registration | **✔** `GET /api/verification?email=<gmail>` 200 → code → `POST /api/user/register {username,password,password2,email,verification_code}` 200 |
| check-in funding | **✔** `POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}` ⇒ `quota 0 → 54 438` |
| key | **✔** `POST /api/token/` → `GET /api/token/?p=0&size=20` → **`POST /api/token/{id}/key`** reveals the full key |
| live chat | **✔** `deepseek-v4.1-flash` → 200 `"MX-PONG"`, re-proved from **three independent exits** (`http://51.15.20.32:3129`, `socks5://183.106.215.208:1080`, `http://3.224.130.91:3128`) |
| scale | **✔ 10 accounts minted, every key answers a real completion** (`deepseek-v4.1-flash` → 200 `"ok"`), one exit per account; check-in awards seen: `12 275 … 79 697` quota (≈ $0.025–$0.16), i.e. **the daily credit is randomised** |

> **Quality gates.** Every key here passed a live `chat-200` before it was written to `keys.txt`
> (dead keys are never shipped). Router combos follow the same rule at the model level: an entry is
> added **only if the model answers `chat-200` and can call tools** — a model without tool support,
> a dead key or a dead provider is dropped instead of added. Re-run the verify step any time;
> failures are reported, never hidden.

## 🧭 How it works

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: Moxin Studio page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://www.moxin.studio/v1/chat/completions → real content = the key is live
```

| Step | Call | Notes |
| --- | --- | --- |
| mailbox | public Gmail pool | the panel accepts Gmail-class addresses; a **recycled** box can already exist on the panel (`Email address is already in use`) — the CLI retries with a fresh box |
| code | `GET /api/verification?email=…` then read the inbox | ⚠ **the brand filter must key on the SENDER** (`moxin|studio|墨信`): the shared inboxes also carry other panels' codes (`OpenArt.ai`, `WhatsToken.AI` …) and a generic "verification code" subject is not evidence — both produced `Verification code is incorrect or has expired` before the filter was tightened |
| register | `POST /api/user/register {username,password,password2,email,verification_code}` | the response may carry its own `access_token`; otherwise `POST /api/user/login {username,password}` (username or e-mail both work) |
| fund | `POST /api/user/checkin` | the whole point: `quota_awarded ≈ 54 438` per account per day |
| key | `POST /api/token/` → list → `POST /api/token/{id}/key` | this fork masks the key in every read, the `…/{id}/key` route reveals it |
| gate | `POST /v1/chat/completions` | a key is written to `keys.txt` only after real content comes back |

## ⚙️ CLI options

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

## 🛰️ Behind a proxy

**Optional.** `--pool <file>` (or `MOXIN_STUDIO_PROXIES`) accepts one exit per line — `http://host:port`, `https://host:port` (CONNECT) or `socks5://host:port` (with optional `user:pass@`) — and the CLI takes one exit per account; without a pool it runs direct. The built-in plumbing is ~80 lines of `net`/`tls`, no library.

## 📄 Files

| Path | Contents |
| --- | --- |
| `keys.txt` | one live-verified key per line |
| `accounts.jsonl` | e-mail, key, check-in evidence (`quotaBefore` / `quotaAwarded` / `quota`), model, exit |
| `proxies.txt` | your exits, optional |
| `out/` | per-attempt run records |

## 🔌 Connect to 9Router

[9Router](https://9router.com) is a self-hosted, OpenAI-compatible router:

```bash
node public-cli.js --connect-9router --router9 http://localhost:20128 --password 123456
```

Defaults: prefix `moxin-studio`, base `https://www.moxin.studio/v1`, model `deepseek-v4.1-flash`; models are served as `moxin-studio/<model id>`.

## 🔌 Connect to OmniRoute

[OmniRoute](https://omniroute.online) is a self-hosted, OpenAI-compatible router:

```bash
node public-cli.js --connect-omniroute --router9 http://localhost:20128 --password 123456
```

Defaults: prefix `moxin-studio`, base `https://www.moxin.studio/v1`, model `deepseek-v4.1-flash`; models are served as `moxin-studio/<model id>`.

## 🔑 What one key gets you

| | |
| --- | --- |
| 🏢 Provider | **Moxin Studio** — [www.moxin.studio](https://www.moxin.studio) |
| 🎁 What the provider gives | **no sign-up bonus** — the daily check-in pays **≈$0.109 per account per day** |
| 🔑 What one key gets you | the 143-row catalogue on `https://www.moxin.studio/v1` (108 ids visible on a live key) |
| 📦 This repo | **every key, all chat-verified** on 2026-10-21 (one exit per account) |
| 🌐 API shape | OpenAI-compatible: `POST https://www.moxin.studio/v1/chat/completions` |
| 🛡️ Gates | **no captcha**, e-mail code only; sender-brand filter required on the shared mailboxes |

## 🐞 Issues

If the panel changes its register/check-in routes, its e-mail policy or the catalogue, the tool stops there and says so. **[Open an issue](../../issues)** with the command and the exact output.

## 📄 License

MIT © 2026 — see [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 www-moxin-studio-bulk-apikey contributors

</div>
