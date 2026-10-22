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

**Crie contas e chaves em massa no Moxin Studio (new-api) — inferência compatível com OpenAI em https://www.moxin.studio/v1, financiada pelo check-in diário do painel.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-143%20served-412991?style=for-the-badge)](https://www.moxin.studio/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Início rápido](#-quick-start) · [🎁 Nível gratuito](#-free-tier--how-much-you-get) · [🤖 Modelos](#-models--all-143-of-them) · [🧪 Use a API](#-how-to-use-the-api) · [✅ Verificado](#-verified-working--every-key-is-tested) · [🧭 Como funciona](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Opções](#️-cli-options) · [🛰️ Proxies](#️-behind-a-proxy) · [🐞 Problemas](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Por que isto existe

O Moxin Studio é um fork do new-api (`v1.0.0-rc.45`, `system_name "studio"`) **sem nenhum captcha** (`turnstile_check=false`) e com cadastro por código de e-mail — mas **sem bônus de cadastro**. O que torna uma conta valiosa é o **check-in diário**: `POST /api/user/checkin` pagou `{"quota_awarded":54438}` em 2026-10-21 (≈ **$0.109** a `quota_per_unit 500000`), então uma conta vale o crédito de um dia e continua pagando se você fizer check-in diariamente. O catálogo por trás disso tem **143 modelos** (DeepSeek · Gemini · Claude · GPT · GLM · Qwen · MiniMax … como linhas marcadas por grupo, como `[次]deepseek-v4.1-flash`), servidos de forma compatível com OpenAI em `https://www.moxin.studio/v1`. Seu host irmão `www.moxin.plus` ("墨信AI", `rc.44`) é um **banco de dados separado** — uma chave criada aqui responde `Invalid token` lá.

## 🎁 Nível gratuito — quanto você recebe

| O quê | Valor |
| --- | --- |
| Bônus de cadastro | **nenhum** — uma conta nova começa com `quota 0` (medido duas vezes: conta de 2026-10-18 e as novas de hoje) |
| Check-in diário | **aleatório por conta por dia, observado entre 12 275 … 79 697 quota (≈ $0.025–$0.16; mediana ~54 000 ≈ $0.11)** (`POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}`); uma segunda chamada no mesmo dia responde `Already checked in today` |
| Catálogo | 143 linhas, **0 estritamente gratuitas** (`model_ratio 0` + `model_price 0`): tudo é pago, o crédito do check-in é o que você gasta |
| Captcha | **nenhum** (`turnstile_check=false`) — HTTP puro, sem navegador, sem solucionador |
| E-mail | `email_verification=true`, código válido por 10 min; **apenas endereços classe Gmail** funcionam na prática (o pool público de gmail) |
| Grupos | `default` (ratio 1) · `DeepSeek[羊毛] 0.35` · `Codex 0.2` · `Claude[特价] 0.6` · `Claude[折扣] 1.2` · `Claude[高质] 2` · `GPT[官方] 5` · `Gemini…` |
| Taxa/expiração | nenhum limite publicado por chave; o crédito do check-in não expira |

> 💡 O nível gratuito é definido pelo Moxin Studio e pode mudar. Confira os números ao vivo em [www.moxin.studio](https://www.moxin.studio) — e deixe o próprio gate da ferramenta decidir:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modelos — todos os 143

`GET /v1/models` respondeu **108 ids** em uma chave ativa (2026-10-21). Os ids do catálogo são marcados por grupo (`[次]deepseek-v4.1-flash`, `[官方]gemini-…`, `[vertex特价]gemini-4-argon`) e o **id sem prefixo também resolve**; estes foram verificados por chat de ponta a ponta:

| ID do modelo | observação |
| --- | --- |
| `deepseek-v4.1-flash` | o padrão de verificação — 200 `"MX-PONG"` de três saídas independentes |

O feed `/api/pricing` lista 143 linhas: `deepseek-v4-flash`, `gemini-2.5-flash`, `glm-4.6`, `gpt-4o-mini`, `claude-haiku-4-5`, `qwen3.8-flash`, `MiniMax-M3`, `kimi-k3`, `grok-4.6` e mais, cada um em um ou mais dos grupos acima.

## 🧪 Como usar a API

URL base **`https://www.moxin.studio/v1`** — compatível com OpenAI:

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

Recarregue a chave fazendo check-in (token de sessão, não a chave de API): `POST /api/user/checkin` com `Authorization: Bearer <access_token>`.

## 🛠 Início rápido

```bash
cd www-moxin-studio-bulk-apikey
node public-cli.js --count 3        # three accounts: e-mail code -> check-in -> key -> chat-200
node public-cli.js --verify         # re-test every key in keys.txt
node public-cli.js --pool proxies.txt   # optional: one exit per account
```

Sem navegador, sem solucionador de captcha, sem npm install. `keys.txt` só é escrito após uma **completion real**, e `accounts.jsonl` guarda a evidência de check-in por conta (`quotaBefore` → `quotaAwarded` → `quota`).

### 🖱️ O launcher

## ✅ Verificado e funcionando — toda chave é testada

Todas as verificações abaixo foram reexecutadas em **2026-10-21** a partir do pool de saídas (sem WARP):

| Verificação | Resultado |
| --- | --- |
| `GET /api/status` | **200** — `register_enabled`+`password_register_enabled` true, `email_verification=true`, **`turnstile_check=false`**, `checkin_enabled=true`, pricing de 143 linhas |
| cadastro | **✔** `GET /api/verification?email=<gmail>` 200 → código → `POST /api/user/register {username,password,password2,email,verification_code}` 200 |
| financiamento por check-in | **✔** `POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}` ⇒ `quota 0 → 54 438` |
| chave | **✔** `POST /api/token/` → `GET /api/token/?p=0&size=20` → **`POST /api/token/{id}/key`** revela a chave completa |
| chat ao vivo | **✔** `deepseek-v4.1-flash` → 200 `"MX-PONG"`, reprovado a partir de **três saídas independentes** (`http://51.15.20.32:3129`, `socks5://183.106.215.208:1080`, `http://3.224.130.91:3128`) |
| escala | **✔ 10 contas criadas, 10/10 chaves respondem a uma completion real** (`deepseek-v4.1-flash` → 200 `"ok"`), uma saída por conta; prêmios de check-in vistos: `12 275 … 79 697` quota (≈ $0.025–$0.16), ou seja, **o crédito diário é aleatório** |

## 🧭 Como funciona

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: Moxin Studio page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://www.moxin.studio/v1/chat/completions → real content = the key is live
```

| Etapa | Chamada | Observações |
| --- | --- | --- |
| caixa de correio | pool público de Gmail | o painel aceita endereços classe Gmail; uma caixa **reciclada** pode já existir no painel (`Email address is already in use`) — a CLI tenta novamente com uma caixa nova |
| código | `GET /api/verification?email=…` e depois ler a caixa de entrada | ⚠ **o filtro de marca deve se basear no REMETENTE** (`moxin|studio|墨信`): as caixas compartilhadas também carregam códigos de outros painéis (`OpenArt.ai`, `WhatsToken.AI` …) e um assunto genérico "verification code" não é evidência — ambos produziram `Verification code is incorrect or has expired` antes de o filtro ser apertado |
| cadastro | `POST /api/user/register {username,password,password2,email,verification_code}` | a resposta pode trazer seu próprio `access_token`; caso contrário, `POST /api/user/login {username,password}` (nome de usuário ou e-mail funcionam) |
| financiar | `POST /api/user/checkin` | o ponto central: `quota_awarded ≈ 54 438` por conta por dia |
| chave | `POST /api/token/` → listar → `POST /api/token/{id}/key` | este fork mascara a chave em toda leitura, a rota `…/{id}/key` a revela |
| gate | `POST /v1/chat/completions` | uma chave só é escrita em `keys.txt` após conteúdo real retornar |

## ⚙️ Opções da CLI

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

## 🛰️ Atrás de um proxy

**Opcional.** `--pool <file>` (ou `MOXIN_STUDIO_PROXIES`) aceita uma saída por linha — `http://host:port`, `https://host:port` (CONNECT) ou `socks5://host:port` (com `user:pass@` opcional) — e a CLI usa uma saída por conta; sem um pool, ela roda direto. A infraestrutura embutida tem ~80 linhas de `net`/`tls`, sem biblioteca.

## 📄 Arquivos

| Caminho | Conteúdo |
| --- | --- |
| `keys.txt` | uma chave verificada ao vivo por linha |
| `accounts.jsonl` | e-mail, chave, evidência de check-in (`quotaBefore` / `quotaAwarded` / `quota`), modelo, saída |
| `proxies.txt` | suas saídas, opcional |
| `out/` | registros de execução por tentativa |

## 🔌 Conectar ao 9Router

[9Router](https://9router.com) é um roteador auto-hospedado e compatível com OpenAI:

```bash
node public-cli.js --connect-9router --router9 http://localhost:20128 --password 123456
```

Padrões: prefixo `moxin-studio`, base `https://www.moxin.studio/v1`, modelo `deepseek-v4.1-flash`; os modelos são servidos como `moxin-studio/<model id>`.

## 🔌 Conectar ao OmniRoute

[OmniRoute](https://omniroute.online) é um roteador auto-hospedado e compatível com OpenAI:

```bash
node public-cli.js --connect-omniroute --router9 http://localhost:20128 --password 123456
```

Padrões: prefixo `moxin-studio`, base `https://www.moxin.studio/v1`, modelo `deepseek-v4.1-flash`; os modelos são servidos como `moxin-studio/<model id>`.
## 🔑 O que uma chave te dá

| | |
| --- | --- |
| 🏢 Provedor | **Moxin Studio** — [www.moxin.studio](https://www.moxin.studio) |
| 🎁 O que o provedor dá | **nenhum bônus de cadastro** — o check-in diário paga **≈$0.109 por conta por dia** |
| 🔑 O que uma chave te dá | o catálogo de 143 linhas em `https://www.moxin.studio/v1` (108 ids visíveis em uma chave ativa) |
| 📦 Este repositório | **10 chaves, todas verificadas por chat** em 2026-10-21 (uma saída por conta) |
| 🌐 Formato da API | Compatível com OpenAI: `POST https://www.moxin.studio/v1/chat/completions` |
| 🛡️ Gates | **sem captcha**, apenas código de e-mail; filtro de marca do remetente necessário nas caixas compartilhadas |

## 🐞 Problemas

Se o painel mudar suas rotas de cadastro/check-in, sua política de e-mail ou o catálogo, a ferramenta para ali e avisa. **[Abra uma issue](../../issues)** com o comando e a saída exata.

## 📄 Licença

MIT © 2026 — veja [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 www-moxin-studio-bulk-apikey contributors

</div>

---

## ⚠️ Risco de conta e como minimizar
