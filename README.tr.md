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

**Toplu Moxin Studio (new-api) hesabı ve anahtarı oluşturun — https://www.moxin.studio/v1 üzerinde OpenAI uyumlu çıkarım, panelin günlük giriş ödülüyle finanse edilir.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-143%20served-412991?style=for-the-badge)](https://www.moxin.studio/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Hızlı başlangıç](#-quick-start) · [🎁 Ücretsiz katman](#-free-tier--how-much-you-get) · [🤖 Modeller](#-models--all-143-of-them) · [🧪 API'yi kullanma](#-how-to-use-the-api) · [✅ Doğrulandı](#-verified-working--every-key-is-tested) · [🧭 Nasıl çalışır](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [🔌 OmniRoute](#-connect-to-omniroute) · [⚙️ Seçenekler](#️-cli-options) · [🛰️ Proxy'ler](#️-behind-a-proxy) · [🐞 Sorunlar](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Bu neden var

Moxin Studio, **hiç captcha içermeyen** (`turnstile_check=false`) ve e-posta koduyla kayıt olan bir new-api çatalıdır (`v1.0.0-rc.45`, `system_name "studio"`) — ancak **kayıt bonusu yoktur**. Bir hesabı değerli kılan şey **günlük giriş ödülüdür**: `POST /api/user/checkin`, 2026-10-21 tarihinde `{"quota_awarded":54438}` ödedi (`quota_per_unit 500000` ile ≈ **$0.109**), yani bir hesap bir günlük kredi değerindedir ve her gün giriş yaparsanız ödemeye devam eder. Arkasındaki katalog **143 modeldir** (DeepSeek · Gemini · Claude · GPT · GLM · Qwen · MiniMax … `[次]deepseek-v4.1-flash` gibi grup etiketli satırlar olarak), `https://www.moxin.studio/v1` üzerinde OpenAI uyumlu sunulur. Kardeş sunucusu `www.moxin.plus` ("墨信AI", `rc.44`) **ayrı bir veritabanıdır** — burada üretilen bir anahtar orada `Invalid token` yanıtı verir.

## 🎁 Ücretsiz katman — ne kadar alırsınız

| Ne | Değer |
| --- | --- |
| Kayıt bonusu | **yok** — yeni bir hesap `quota 0` ile başlar (iki kez ölçüldü: 2026-10-18 hesabı ve bugünkü yeni hesaplar) |
| Günlük giriş ödülü | **hesap başına günlük rastgele, gözlemlenen 12 275 … 79 697 quota (≈ $0.025–$0.16; medyan ~54 000 ≈ $0.11)** (`POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}`); aynı gün ikinci çağrı `Already checked in today` yanıtı verir |
| Katalog | 143 satır, **0 kesinlikle ücretsiz** (`model_ratio 0` + `model_price 0`): her şey ücretli, harcadığınız şey giriş ödülü kredisidir |
| Captcha | **yok** (`turnstile_check=false`) — düz HTTP, tarayıcı yok, çözücü yok |
| E-posta | `email_verification=true`, kod 10 dk geçerli; pratikte **yalnızca Gmail sınıfı adresler** çalışır (genel gmail havuzu) |
| Gruplar | `default` (oran 1) · `DeepSeek[羊毛] 0.35` · `Codex 0.2` · `Claude[特价] 0.6` · `Claude[折扣] 1.2` · `Claude[高质] 2` · `GPT[官方] 5` · `Gemini…` |
| Hız/sona erme | yayımlanmış anahtar başına sınır yok; giriş ödülü kredisi sona ermez |

> 💡 Ücretsiz katman Moxin Studio tarafından belirlenir ve değişebilir. Güncel sayıları [www.moxin.studio](https://www.moxin.studio) adresinden kontrol edin — ve aracın kendi kapısının karar vermesine izin verin:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modeller — 143'ünün tamamı

`GET /v1/models`, canlı bir anahtarda (2026-10-21) **108 id** yanıtladı. Katalog id'leri grup etiketlidir (`[次]deepseek-v4.1-flash`, `[官方]gemini-…`, `[vertex特价]gemini-4-argon`) ve **öneksiz id de çözümlenir**; bunlar uçtan uca sohbetle doğrulandı:

| Model ID | not |
| --- | --- |
| `deepseek-v4.1-flash` | doğrulama varsayılanı — üç bağımsız çıkıştan 200 `"MX-PONG"` |

`/api/pricing` akışı 143 satır listeler: `deepseek-v4-flash`, `gemini-2.5-flash`, `glm-4.6`, `gpt-4o-mini`, `claude-haiku-4-5`, `qwen3.8-flash`, `MiniMax-M3`, `kimi-k3`, `grok-4.6` ve daha fazlası, her biri yukarıdaki gruplardan birinde veya daha fazlasında.

## 🧪 API nasıl kullanılır

Temel URL **`https://www.moxin.studio/v1`** — OpenAI uyumlu:

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

Anahtarı giriş yaparak yükleyin (API anahtarı değil, oturum belirteci): `Authorization: Bearer <access_token>` ile `POST /api/user/checkin`.

## 🛠 Hızlı başlangıç

```bash
cd www-moxin-studio-bulk-apikey
node public-cli.js --count 3        # three accounts: e-mail code -> check-in -> key -> chat-200
node public-cli.js --verify         # re-test every key in keys.txt
node public-cli.js --pool proxies.txt   # optional: one exit per account
```

Tarayıcı yok, captcha çözücü yok, npm install yok. `keys.txt` yalnızca **gerçek bir tamamlama** sonrasında yazılır ve `accounts.jsonl` hesap başına giriş ödülü kanıtını saklar (`quotaBefore` → `quotaAwarded` → `quota`).

### 🖱️ Başlatıcı

## ✅ Çalıştığı doğrulandı — her anahtar test edilir

Aşağıdaki tüm kontroller **2026-10-21** tarihinde çıkış havuzundan (WARP olmadan) yeniden çalıştırıldı:

| Kontrol | Sonuç |
| --- | --- |
| `GET /api/status` | **200** — `register_enabled`+`password_register_enabled` true, `email_verification=true`, **`turnstile_check=false`**, `checkin_enabled=true`, 143 satırlık fiyatlandırma |
| kayıt | **✔** `GET /api/verification?email=<gmail>` 200 → kod → `POST /api/user/register {username,password,password2,email,verification_code}` 200 |
| giriş ödülü finansmanı | **✔** `POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}` ⇒ `quota 0 → 54 438` |
| anahtar | **✔** `POST /api/token/` → `GET /api/token/?p=0&size=20` → **`POST /api/token/{id}/key`** tam anahtarı ortaya çıkarır |
| canlı sohbet | **✔** `deepseek-v4.1-flash` → 200 `"MX-PONG"`, **üç bağımsız çıkıştan** yeniden kanıtlandı (`http://51.15.20.32:3129`, `socks5://183.106.215.208:1080`, `http://3.224.130.91:3128`) |
| ölçek | **✔ 10 hesap oluşturuldu, her anahtar gerçek bir tamamlama yanıtlıyor** (`deepseek-v4.1-flash` → 200 `"ok"`), hesap başına bir çıkış; görülen giriş ödülleri: `12 275 … 79 697` quota (≈ $0.025–$0.16), yani **günlük kredi rastgeledir** |

> **Kalite kapıları.** Buradaki her anahtar `keys.txt` dosyasına yazılmadan önce canlı bir `chat-200` geçti
> (ölü anahtarlar asla gönderilmez). Yönlendirici kombinasyonları model düzeyinde aynı kuralı izler: bir giriş
> **yalnızca model `chat-200` yanıtlıyorsa ve araçları çağırabiliyorsa** eklenir — araç desteği olmayan bir model,
> ölü bir anahtar veya ölü bir sağlayıcı eklenmek yerine çıkarılır. Doğrulama adımını istediğiniz zaman yeniden çalıştırın;
> hatalar bildirilir, asla gizlenmez.

## 🧭 Nasıl çalışır

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: Moxin Studio page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://www.moxin.studio/v1/chat/completions → real content = the key is live
```

| Adım | Çağrı | Notlar |
| --- | --- | --- |
| posta kutusu | genel Gmail havuzu | panel Gmail sınıfı adresleri kabul eder; **geri dönüştürülmüş** bir kutu panelde zaten var olabilir (`Email address is already in use`) — CLI yeni bir kutuyla yeniden dener |
| kod | `GET /api/verification?email=…` ardından gelen kutusunu oku | ⚠ **marka filtresi GÖNDEREN üzerinden anahtarlanmalıdır** (`moxin|studio|墨信`): paylaşılan gelen kutuları diğer panellerin kodlarını da taşır (`OpenArt.ai`, `WhatsToken.AI` …) ve genel bir "verification code" konusu kanıt değildir — filtre sıkılaştırılmadan önce her ikisi de `Verification code is incorrect or has expired` üretti |
| kayıt | `POST /api/user/register {username,password,password2,email,verification_code}` | yanıt kendi `access_token`'ını taşıyabilir; aksi halde `POST /api/user/login {username,password}` (kullanıcı adı veya e-posta ikisi de çalışır) |
| fonlama | `POST /api/user/checkin` | asıl mesele: hesap başına günlük `quota_awarded ≈ 54 438` |
| anahtar | `POST /api/token/` → listele → `POST /api/token/{id}/key` | bu çatal her okumada anahtarı maskeler, `…/{id}/key` rotası onu ortaya çıkarır |
| kapı | `POST /v1/chat/completions` | bir anahtar `keys.txt` dosyasına yalnızca gerçek içerik geri geldikten sonra yazılır |

## ⚙️ CLI seçenekleri

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

## 🛰️ Proxy arkasında

**İsteğe bağlı.** `--pool <file>` (veya `MOXIN_STUDIO_PROXIES`) satır başına bir çıkış kabul eder — `http://host:port`, `https://host:port` (CONNECT) veya `socks5://host:port` (isteğe bağlı `user:pass@` ile) — ve CLI hesap başına bir çıkış alır; havuz olmadan doğrudan çalışır. Yerleşik altyapı ~80 satır `net`/`tls`'tir, kütüphane yok.

## 📄 Dosyalar

| Yol | İçerik |
| --- | --- |
| `keys.txt` | satır başına bir canlı doğrulanmış anahtar |
| `accounts.jsonl` | e-posta, anahtar, giriş ödülü kanıtı (`quotaBefore` / `quotaAwarded` / `quota`), model, çıkış |
| `proxies.txt` | çıkışlarınız, isteğe bağlı |
| `out/` | deneme başına çalıştırma kayıtları |

## 🔌 9Router'a bağlan

[9Router](https://9router.com) kendi kendine barındırılan, OpenAI uyumlu bir yönlendiricidir:

```bash
node public-cli.js --connect-9router --router9 http://localhost:20128 --password 123456
```

Varsayılanlar: önek `moxin-studio`, temel `https://www.moxin.studio/v1`, model `deepseek-v4.1-flash`; modeller `moxin-studio/<model id>` olarak sunulur.

## 🔌 OmniRoute'a bağlan

[OmniRoute](https://omniroute.online) kendi kendine barındırılan, OpenAI uyumlu bir yönlendiricidir:

```bash
node public-cli.js --connect-omniroute --router9 http://localhost:20128 --password 123456
```

Varsayılanlar: önek `moxin-studio`, temel `https://www.moxin.studio/v1`, model `deepseek-v4.1-flash`; modeller `moxin-studio/<model id>` olarak sunulur.

## 🔑 Bir anahtar size ne kazandırır

| | |
| --- | --- |
| 🏢 Sağlayıcı | **Moxin Studio** — [www.moxin.studio](https://www.moxin.studio) |
| 🎁 Sağlayıcının verdikleri | **kayıt bonusu yok** — günlük giriş ödülü **hesap başına günlük ≈$0.109** öder |
| 🔑 Bir anahtar size ne kazandırır | `https://www.moxin.studio/v1` üzerinde 143 satırlık katalog (canlı bir anahtarda 108 id görünür) |
| 📦 Bu depo | **her anahtar, tamamı sohbetle doğrulanmış** 2026-10-21 tarihinde (hesap başına bir çıkış) |
| 🌐 API biçimi | OpenAI uyumlu: `POST https://www.moxin.studio/v1/chat/completions` |
| 🛡️ Kapılar | **captcha yok**, yalnızca e-posta kodu; paylaşılan posta kutularında gönderen-marka filtresi gerekli |

## 🐞 Sorunlar

Panel kayıt/giriş ödülü rotalarını, e-posta politikasını veya kataloğu değiştirirse, araç orada durur ve bunu söyler. Komut ve tam çıktıyla birlikte **[bir sorun açın](../../issues)**.

## 📄 Lisans

MIT © 2026 — bkz. [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 www-moxin-studio-bulk-apikey katkıda bulunanlar

</div>