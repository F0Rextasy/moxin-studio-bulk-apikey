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

**Moxin Studio (new-api)-Konten und -Keys in großen Mengen erstellen — OpenAI-kompatible Inferenz auf https://www.moxin.studio/v1, finanziert durch den täglichen Check-in des Panels.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-143%20served-412991?style=for-the-badge)](https://www.moxin.studio/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Schnellstart](#-quick-start) · [🎁 Kostenlose Stufe](#-free-tier--how-much-you-get) · [🤖 Modelle](#-models--all-143-of-them) · [🧪 Die API verwenden](#-how-to-use-the-api) · [✅ Verifiziert](#-verified-working--every-key-is-tested) · [🧭 So funktioniert es](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Optionen](#️-cli-options) · [🛰️ Proxys](#️-behind-a-proxy) · [🐞 Probleme](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Warum es das gibt

Moxin Studio ist ein new-api-Fork (`v1.0.0-rc.45`, `system_name "studio"`) mit **überhaupt keinem Captcha** (`turnstile_check=false`) und einer E-Mail-Code-Registrierung — aber **keinem Registrierungsbonus**. Was ein Konto wertvoll macht, ist der **tägliche Check-in**: `POST /api/user/checkin` zahlte am 2026-10-21 `{"quota_awarded":54438}` (≈ **$0.109** bei `quota_per_unit 500000`), ein Konto ist also einen Tag Guthaben wert und zahlt weiter, wenn du täglich eincheckst. Der dahinterliegende Katalog umfasst **143 Modelle** (DeepSeek · Gemini · Claude · GPT · GLM · Qwen · MiniMax … als gruppenmarkierte Zeilen wie `[次]deepseek-v4.1-flash`), OpenAI-kompatibel bereitgestellt unter `https://www.moxin.studio/v1`. Der Schwester-Host `www.moxin.plus` ("墨信AI", `rc.44`) ist eine **separate Datenbank** — ein hier erzeugter Key antwortet dort mit `Invalid token`.

## 🎁 Kostenlose Stufe — wie viel du bekommst

| Was | Wert |
| --- | --- |
| Registrierungsbonus | **keiner** — ein frisches Konto startet bei `quota 0` (zweimal gemessen: Konto vom 2026-10-18 und die heutigen frischen) |
| Täglicher Check-in | **pro Konto und Tag randomisiert, beobachtet 12 275 … 79 697 Quota (≈ $0.025–$0.16; Median ~54 000 ≈ $0.11)** (`POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}`); ein zweiter Aufruf am selben Tag antwortet `Already checked in today` |
| Katalog | 143 Zeilen, **0 strikt kostenlos** (`model_ratio 0` + `model_price 0`): alles ist kostenpflichtig, das Check-in-Guthaben ist das, was du ausgibst |
| Captcha | **keines** (`turnstile_check=false`) — reines HTTP, kein Browser, kein Solver |
| E-Mail | `email_verification=true`, Code 10 Min. gültig; praktisch funktionieren **nur Gmail-Klasse-Adressen** (der öffentliche Gmail-Pool) |
| Gruppen | `default` (Ratio 1) · `DeepSeek[羊毛] 0.35` · `Codex 0.2` · `Claude[特价] 0.6` · `Claude[折扣] 1.2` · `Claude[高质] 2` · `GPT[官方] 5` · `Gemini…` |
| Rate/Ablauf | kein veröffentlichtes Limit pro Key; das Check-in-Guthaben läuft nicht ab |

> 💡 Die kostenlose Stufe wird von Moxin Studio festgelegt und kann sich ändern. Prüfe die Live-Zahlen auf [www.moxin.studio](https://www.moxin.studio) — und lass das Gate des Tools selbst entscheiden:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modelle — alle 143

`GET /v1/models` antwortete mit **108 IDs** auf einem Live-Key (2026-10-21). Katalog-IDs sind gruppenmarkiert (`[次]deepseek-v4.1-flash`, `[官方]gemini-…`, `[vertex特价]gemini-4-argon`) und die **ID ohne Präfix wird ebenfalls aufgelöst**; diese wurden Ende-zu-Ende per Chat verifiziert:

| Modell-ID | Hinweis |
| --- | --- |
| `deepseek-v4.1-flash` | der Verifizierungs-Standard — 200 `"MX-PONG"` von drei unabhängigen Ausgängen |

Der `/api/pricing`-Feed listet 143 Zeilen: `deepseek-v4-flash`, `gemini-2.5-flash`, `glm-4.6`, `gpt-4o-mini`, `claude-haiku-4-5`, `qwen3.8-flash`, `MiniMax-M3`, `kimi-k3`, `grok-4.6` und mehr, jeweils in einer oder mehreren der oben genannten Gruppen.

## 🧪 So verwendest du die API

Basis-URL **`https://www.moxin.studio/v1`** — OpenAI-kompatibel:

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

Lade den Key per Check-in auf (Session-Token, nicht der API-Key): `POST /api/user/checkin` mit `Authorization: Bearer <access_token>`.

## 🛠 Schnellstart

```bash
cd www-moxin-studio-bulk-apikey
node public-cli.js --count 3        # three accounts: e-mail code -> check-in -> key -> chat-200
node public-cli.js --verify         # re-test every key in keys.txt
node public-cli.js --pool proxies.txt   # optional: one exit per account
```

Kein Browser, kein Captcha-Solver, kein npm install. `keys.txt` wird erst nach einer **echten Completion** geschrieben, und `accounts.jsonl` bewahrt die Check-in-Nachweise pro Konto auf (`quotaBefore` → `quotaAwarded` → `quota`).

### 🖱️ Der Launcher

## ✅ Verifiziert funktionierend — jeder Key wird getestet

Alle untenstehenden Prüfungen wurden am **2026-10-21** aus dem Exit-Pool erneut ausgeführt (kein WARP):

| Prüfung | Ergebnis |
| --- | --- |
| `GET /api/status` | **200** — `register_enabled`+`password_register_enabled` true, `email_verification=true`, **`turnstile_check=false`**, `checkin_enabled=true`, 143-zeilige Preisliste |
| Registrierung | **✔** `GET /api/verification?email=<gmail>` 200 → Code → `POST /api/user/register {username,password,password2,email,verification_code}` 200 |
| Check-in-Finanzierung | **✔** `POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}` ⇒ `quota 0 → 54 438` |
| Key | **✔** `POST /api/token/` → `GET /api/token/?p=0&size=20` → **`POST /api/token/{id}/key`** gibt den vollständigen Key preis |
| Live-Chat | **✔** `deepseek-v4.1-flash` → 200 `"MX-PONG"`, erneut bewiesen von **drei unabhängigen Ausgängen** (`http://51.15.20.32:3129`, `socks5://183.106.215.208:1080`, `http://3.224.130.91:3128`) |
| Skalierung | **verified** (`deepseek-v4.1-flash` → 200 `"ok"`), ein Ausgang pro Konto; beobachtete Check-in-Gewinne: `12 275 … 79 697` Quota (≈ $0.025–$0.16), d. h. **das tägliche Guthaben ist randomisiert** |

## 🧭 So funktioniert es

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: Moxin Studio page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://www.moxin.studio/v1/chat/completions → real content = the key is live
```

| Schritt | Aufruf | Hinweise |
| --- | --- | --- |
| Postfach | öffentlicher Gmail-Pool | das Panel akzeptiert Gmail-Klasse-Adressen; ein **recyceltes** Postfach kann bereits im Panel existieren (`Email address is already in use`) — die CLI versucht es mit einem frischen Postfach erneut |
| Code | `GET /api/verification?email=…` dann das Postfach lesen | ⚠ **der Markenfilter muss auf den ABSENDER ansetzen** (`moxin|studio|墨信`): die geteilten Postfächer enthalten auch Codes anderer Panels (`OpenArt.ai`, `WhatsToken.AI` …) und ein generischer Betreff "Verification code" ist kein Beweis — beides führte zu `Verification code is incorrect or has expired`, bevor der Filter verschärft wurde |
| Registrierung | `POST /api/user/register {username,password,password2,email,verification_code}` | die Antwort kann ihr eigenes `access_token` enthalten; andernfalls `POST /api/user/login {username,password}` (Benutzername oder E-Mail funktionieren beide) |
| Finanzierung | `POST /api/user/checkin` | der eigentliche Punkt: `quota_awarded ≈ 54 438` pro Konto und Tag |
| Key | `POST /api/token/` → auflisten → `POST /api/token/{id}/key` | dieser Fork maskiert den Key bei jedem Lesen, die Route `…/{id}/key` gibt ihn preis |
| Gate | `POST /v1/chat/completions` | ein Key wird erst dann in `keys.txt` geschrieben, wenn echte Inhalte zurückkommen |

## ⚙️ CLI-Optionen

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

## 🛰️ Hinter einem Proxy

**Optional.** `--pool <file>` (oder `MOXIN_STUDIO_PROXIES`) akzeptiert einen Ausgang pro Zeile — `http://host:port`, `https://host:port` (CONNECT) oder `socks5://host:port` (mit optionalem `user:pass@`) — und die CLI nimmt einen Ausgang pro Konto; ohne Pool läuft sie direkt. Die eingebaute Infrastruktur umfasst ~80 Zeilen `net`/`tls`, keine Bibliothek.

## 📄 Dateien

| Pfad | Inhalt |
| --- | --- |
| `keys.txt` | ein live-verifizierter Key pro Zeile |
| `accounts.jsonl` | E-Mail, Key, Check-in-Nachweise (`quotaBefore` / `quotaAwarded` / `quota`), Modell, Ausgang |
| `proxies.txt` | deine Ausgänge, optional |
| `out/` | Aufzeichnungen pro Versuch |

## 🔌 Mit 9Router verbinden

[9Router](https://9router.com) ist ein selbst gehosteter, OpenAI-kompatibler Router:

```bash
node public-cli.js --connect-9router --router9 http://localhost:20128 --password 123456
```

Standardwerte: Präfix `moxin-studio`, Basis `https://www.moxin.studio/v1`, Modell `deepseek-v4.1-flash`; Modelle werden als `moxin-studio/<model id>` bereitgestellt.

## 🔌 Mit OmniRoute verbinden

[OmniRoute](https://omniroute.online) ist ein selbst gehosteter, OpenAI-kompatibler Router:

```bash
node public-cli.js --connect-omniroute --router9 http://localhost:20128 --password 123456
```

Standardwerte: Präfix `moxin-studio`, Basis `https://www.moxin.studio/v1`, Modell `deepseek-v4.1-flash`; Modelle werden als `moxin-studio/<model id>` bereitgestellt.
## 🔑 Was ein Key dir bringt

| | |
| --- | --- |
| 🏢 Anbieter | **Moxin Studio** — [www.moxin.studio](https://www.moxin.studio) |
| 🎁 Was der Anbieter gibt | **kein Registrierungsbonus** — der tägliche Check-in zahlt **≈$0.109 pro Konto und Tag** |
| 🔑 Was ein Key dir bringt | den 143-zeiligen Katalog auf `https://www.moxin.studio/v1` (108 IDs auf einem Live-Key sichtbar) |
| 📦 Dieses Repo | **keys verified** am 2026-10-21 (ein Ausgang pro Konto) |
| 🌐 API-Form | OpenAI-kompatibel: `POST https://www.moxin.studio/v1/chat/completions` |
| 🛡️ Gates | **kein Captcha**, nur E-Mail-Code; Absender-Markenfilter auf den geteilten Postfächern erforderlich |

## 🐞 Probleme

Wenn das Panel seine Registrierungs-/Check-in-Routen, seine E-Mail-Richtlinie oder den Katalog ändert, stoppt das Tool dort und sagt es dir. **[Öffne ein Issue](../../issues)** mit dem Befehl und der exakten Ausgabe.

## 📄 Lizenz

MIT © 2026 — siehe [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 www-moxin-studio-bulk-apikey contributors

</div>

---

## ⚠️ Konto-Risiko und Minimierung
