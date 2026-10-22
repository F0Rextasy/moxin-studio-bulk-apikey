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

**Créer en masse des comptes et des clés Moxin Studio (new-api) — inférence compatible OpenAI sur https://www.moxin.studio/v1, financée par le check-in quotidien du panel.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-143%20served-412991?style=for-the-badge)](https://www.moxin.studio/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Démarrage rapide](#-quick-start) · [🎁 Offre gratuite](#-free-tier--how-much-you-get) · [🤖 Modèles](#-models--all-143-of-them) · [🧪 Utiliser l'API](#-how-to-use-the-api) · [✅ Vérifié](#-verified-working--every-key-is-tested) · [🧭 Comment ça marche](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Options](#️-cli-options) · [🛰️ Proxys](#️-behind-a-proxy) · [🐞 Problèmes](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Pourquoi ce projet existe

Moxin Studio est un fork de new-api (`v1.0.0-rc.45`, `system_name "studio"`) avec **aucun captcha** (`turnstile_check=false`) et une inscription par code e-mail — mais **aucun bonus d'inscription**. Ce qui rend un compte intéressant, c'est le **check-in quotidien** : `POST /api/user/checkin` a payé `{"quota_awarded":54438}` le 2026-10-21 (≈ **0,109 $** à `quota_per_unit 500000`), donc un compte vaut le crédit d'une journée et continue de payer si vous faites le check-in chaque jour. Le catalogue derrière compte **143 modèles** (DeepSeek · Gemini · Claude · GPT · GLM · Qwen · MiniMax … sous forme de lignes étiquetées par groupe comme `[次]deepseek-v4.1-flash`), servis en compatible OpenAI sur `https://www.moxin.studio/v1`. Son hôte frère `www.moxin.plus` (« 墨信AI », `rc.44`) est une **base de données distincte** — une clé créée ici répond `Invalid token` là-bas.

## 🎁 Offre gratuite — ce que vous obtenez

| Élément | Valeur |
| --- | --- |
| Bonus d'inscription | **aucun** — un compte neuf démarre à `quota 0` (mesuré deux fois : compte du 2026-10-18 et comptes neufs d'aujourd'hui) |
| Check-in quotidien | **aléatoire par compte et par jour, observé entre 12 275 … 79 697 quota (≈ 0,025–0,16 $ ; médiane ~54 000 ≈ 0,11 $)** (`POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}`) ; un second appel le même jour répond `Already checked in today` |
| Catalogue | 143 lignes, **0 strictement gratuit** (`model_ratio 0` + `model_price 0`) : tout est payant, c'est le crédit du check-in que vous dépensez |
| Captcha | **aucun** (`turnstile_check=false`) — HTTP simple, pas de navigateur, pas de solveur |
| E-mail | `email_verification=true`, code valable 10 min ; **seules les adresses de type Gmail** fonctionnent en pratique (le pool gmail public) |
| Groupes | `default` (ratio 1) · `DeepSeek[羊毛] 0.35` · `Codex 0.2` · `Claude[特价] 0.6` · `Claude[折扣] 1.2` · `Claude[高质] 2` · `GPT[官方] 5` · `Gemini…` |
| Débit/expiration | aucune limite publiée par clé ; le crédit du check-in n'expire pas |

> 💡 L'offre gratuite est définie par Moxin Studio et peut changer. Vérifiez les chiffres en direct sur [www.moxin.studio](https://www.moxin.studio) — et laissez la propre barrière de l'outil décider :

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modèles — les 143

`GET /v1/models` a répondu **108 ids** sur une clé active (2026-10-21). Les ids du catalogue sont étiquetés par groupe (`[次]deepseek-v4.1-flash`, `[官方]gemini-…`, `[vertex特价]gemini-4-argon`) et **l'id sans préfixe résout aussi** ; ceux-ci ont été vérifiés par chat de bout en bout :

| ID du modèle | note |
| --- | --- |
| `deepseek-v4.1-flash` | la valeur par défaut de vérification — 200 `"MX-PONG"` depuis trois sorties indépendantes |

Le flux `/api/pricing` liste 143 lignes : `deepseek-v4-flash`, `gemini-2.5-flash`, `glm-4.6`, `gpt-4o-mini`, `claude-haiku-4-5`, `qwen3.8-flash`, `MiniMax-M3`, `kimi-k3`, `grok-4.6` et plus encore, chacun dans un ou plusieurs des groupes ci-dessus.

## 🧪 Comment utiliser l'API

URL de base **`https://www.moxin.studio/v1`** — compatible OpenAI :

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

Rechargez la clé en faisant le check-in (jeton de session, pas la clé API) : `POST /api/user/checkin` avec `Authorization: Bearer <access_token>`.

## 🛠 Démarrage rapide

```bash
cd www-moxin-studio-bulk-apikey
node public-cli.js --count 3        # three accounts: e-mail code -> check-in -> key -> chat-200
node public-cli.js --verify         # re-test every key in keys.txt
node public-cli.js --pool proxies.txt   # optional: one exit per account
```

Pas de navigateur, pas de solveur de captcha, pas de npm install. `keys.txt` n'est écrit qu'après une **complétion réelle**, et `accounts.jsonl` conserve la preuve du check-in par compte (`quotaBefore` → `quotaAwarded` → `quota`).

### 🖱️ Le lanceur

## ✅ Vérifié fonctionnel — chaque clé est testée

Toutes les vérifications ci-dessous ont été relancées le **2026-10-21** depuis le pool de sorties (sans WARP) :

| Vérification | Résultat |
| --- | --- |
| `GET /api/status` | **200** — `register_enabled`+`password_register_enabled` true, `email_verification=true`, **`turnstile_check=false`**, `checkin_enabled=true`, tarification à 143 lignes |
| inscription | **✔** `GET /api/verification?email=<gmail>` 200 → code → `POST /api/user/register {username,password,password2,email,verification_code}` 200 |
| financement par check-in | **✔** `POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}` ⇒ `quota 0 → 54 438` |
| clé | **✔** `POST /api/token/` → `GET /api/token/?p=0&size=20` → **`POST /api/token/{id}/key`** révèle la clé complète |
| chat en direct | **✔** `deepseek-v4.1-flash` → 200 `"MX-PONG"`, re-prouvé depuis **trois sorties indépendantes** (`http://51.15.20.32:3129`, `socks5://183.106.215.208:1080`, `http://3.224.130.91:3128`) |
| échelle | **✔ 10 comptes créés, 10/10 clés répondent à une complétion réelle** (`deepseek-v4.1-flash` → 200 `"ok"`), une sortie par compte ; récompenses de check-in observées : `12 275 … 79 697` quota (≈ 0,025–0,16 $), c'est-à-dire **le crédit quotidien est aléatoire** |

## 🧭 Comment ça marche

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: Moxin Studio page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://www.moxin.studio/v1/chat/completions → real content = the key is live
```

| Étape | Appel | Notes |
| --- | --- | --- |
| boîte mail | pool Gmail public | le panel accepte les adresses de type Gmail ; une boîte **recyclée** peut déjà exister sur le panel (`Email address is already in use`) — le CLI réessaie avec une boîte neuve |
| code | `GET /api/verification?email=…` puis lire la boîte de réception | ⚠ **le filtre de marque doit porter sur l'EXPÉDITEUR** (`moxin|studio|墨信`) : les boîtes partagées transportent aussi les codes d'autres panels (`OpenArt.ai`, `WhatsToken.AI` …) et un objet générique « verification code » n'est pas une preuve — les deux ont produit `Verification code is incorrect or has expired` avant que le filtre ne soit resserré |
| inscription | `POST /api/user/register {username,password,password2,email,verification_code}` | la réponse peut porter son propre `access_token` ; sinon `POST /api/user/login {username,password}` (nom d'utilisateur ou e-mail, les deux fonctionnent) |
| financement | `POST /api/user/checkin` | tout l'intérêt : `quota_awarded ≈ 54 438` par compte et par jour |
| clé | `POST /api/token/` → lister → `POST /api/token/{id}/key` | ce fork masque la clé à chaque lecture, la route `…/{id}/key` la révèle |
| barrière | `POST /v1/chat/completions` | une clé n'est écrite dans `keys.txt` qu'après le retour d'un contenu réel |

## ⚙️ Options CLI

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

## 🛰️ Derrière un proxy

**Optionnel.** `--pool <file>` (ou `MOXIN_STUDIO_PROXIES`) accepte une sortie par ligne — `http://host:port`, `https://host:port` (CONNECT) ou `socks5://host:port` (avec `user:pass@` optionnel) — et le CLI prend une sortie par compte ; sans pool, il fonctionne en direct. La plomberie intégrée fait ~80 lignes de `net`/`tls`, sans bibliothèque.

## 📄 Fichiers

| Chemin | Contenu |
| --- | --- |
| `keys.txt` | une clé vérifiée en direct par ligne |
| `accounts.jsonl` | e-mail, clé, preuve de check-in (`quotaBefore` / `quotaAwarded` / `quota`), modèle, sortie |
| `proxies.txt` | vos sorties, optionnel |
| `out/` | enregistrements d'exécution par tentative |

## 🔌 Connexion à 9Router

[9Router](https://9router.com) est un routeur auto-hébergé compatible OpenAI :

```bash
node public-cli.js --connect-9router --router9 http://localhost:20128 --password 123456
```

Valeurs par défaut : préfixe `moxin-studio`, base `https://www.moxin.studio/v1`, modèle `deepseek-v4.1-flash` ; les modèles sont servis sous `moxin-studio/<model id>`.

## 🔌 Connexion à OmniRoute

[OmniRoute](https://omniroute.online) est un routeur auto-hébergé compatible OpenAI :

```bash
node public-cli.js --connect-omniroute --router9 http://localhost:20128 --password 123456
```

Valeurs par défaut : préfixe `moxin-studio`, base `https://www.moxin.studio/v1`, modèle `deepseek-v4.1-flash` ; les modèles sont servis sous `moxin-studio/<model id>`.
## 🔑 Ce qu'une clé vous apporte

| | |
| --- | --- |
| 🏢 Fournisseur | **Moxin Studio** — [www.moxin.studio](https://www.moxin.studio) |
| 🎁 Ce que le fournisseur donne | **aucun bonus d'inscription** — le check-in quotidien paie **≈ 0,109 $ par compte et par jour** |
| 🔑 Ce qu'une clé vous apporte | le catalogue de 143 lignes sur `https://www.moxin.studio/v1` (108 ids visibles sur une clé active) |
| 📦 Ce dépôt | **10 clés, toutes vérifiées par chat** le 2026-10-21 (une sortie par compte) |
| 🌐 Forme de l'API | compatible OpenAI : `POST https://www.moxin.studio/v1/chat/completions` |
| 🛡️ Barrières | **aucun captcha**, code e-mail uniquement ; filtre de marque d'expéditeur requis sur les boîtes partagées |

## 🐞 Problèmes

Si le panel modifie ses routes d'inscription/check-in, sa politique e-mail ou le catalogue, l'outil s'arrête là et le signale. **[Ouvrez un ticket](../../issues)** avec la commande et la sortie exacte.

## 📄 Licence

MIT © 2026 — voir [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 www-moxin-studio-bulk-apikey contributors

</div>

---

## ⚠️ Risque de compte et minimisation
