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

**Moxin Studio(new-api) 계정과 키를 대량 생성 — https://www.moxin.studio/v1 에서 OpenAI 호환 추론, 패널의 일일 출석 체크로 충전됩니다.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-143%20served-412991?style=for-the-badge)](https://www.moxin.studio/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 빠른 시작](#-quick-start) · [🎁 무료 등급](#-free-tier--how-much-you-get) · [🤖 모델](#-models--all-143-of-them) · [🧪 API 사용법](#-how-to-use-the-api) · [✅ 검증됨](#-verified-working--every-key-is-tested) · [🧭 작동 방식](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ 옵션](#️-cli-options) · [🛰️ 프록시](#️-behind-a-proxy) · [🐞 이슈](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ 이 도구가 존재하는 이유

Moxin Studio는 new-api 포크(`v1.0.0-rc.45`, `system_name "studio"`)로, **캡차가 전혀 없고**(`turnstile_check=false`) 이메일 코드로 가입할 수 있지만 **가입 보너스는 없습니다**. 계정을 가질 가치가 있게 만드는 것은 **일일 출석 체크**입니다: `POST /api/user/checkin`이 2026-10-21에 `{"quota_awarded":54438}`을 지급했으며(`quota_per_unit 500000` 기준 약 **$0.109**), 따라서 계정 하나는 하루치 크레딧의 가치가 있고 매일 출석하면 계속 지급됩니다. 그 뒤의 카탈로그는 **143개 모델**(DeepSeek · Gemini · Claude · GPT · GLM · Qwen · MiniMax … `[次]deepseek-v4.1-flash` 같은 그룹 태그가 붙은 행)이며, `https://www.moxin.studio/v1`에서 OpenAI 호환으로 제공됩니다. 자매 호스트 `www.moxin.plus`("墨信AI", `rc.44`)는 **별도의 데이터베이스**입니다 — 여기서 발급한 키는 그곳에서 `Invalid token`을 반환합니다.

## 🎁 무료 등급 — 얼마나 받을 수 있나

| 항목 | 값 |
| --- | --- |
| 가입 보너스 | **없음** — 새 계정은 `quota 0`에서 시작합니다(두 번 측정: 2026-10-18 계정과 오늘의 새 계정들) |
| 일일 출석 체크 | **계정별·일별 무작위, 관측값 12 275 … 79 697 quota (약 $0.025–$0.16; 중앙값 ~54 000 ≈ $0.11)** (`POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}`); 같은 날 두 번째 호출은 `Already checked in today`를 반환합니다 |
| 카탈로그 | 143개 행, **엄격 무료 0개**(`model_ratio 0` + `model_price 0`): 전부 유료이며, 출석 체크 크레딧이 사용할 재원입니다 |
| 캡차 | **없음**(`turnstile_check=false`) — 순수 HTTP, 브라우저도 솔버도 필요 없음 |
| 이메일 | `email_verification=true`, 코드 유효 10분; 실제로는 **Gmail급 주소만** 작동합니다(공용 gmail 풀) |
| 그룹 | `default` (ratio 1) · `DeepSeek[羊毛] 0.35` · `Codex 0.2` · `Claude[特价] 0.6` · `Claude[折扣] 1.2` · `Claude[高质] 2` · `GPT[官方] 5` · `Gemini…` |
| 속도/만료 | 공개된 키별 제한 없음; 출석 체크 크레딧은 만료되지 않습니다 |

> 💡 무료 등급은 Moxin Studio가 정하며 변경될 수 있습니다. 실시간 수치는 [www.moxin.studio](https://www.moxin.studio)에서 확인하세요 — 그리고 도구 자체의 게이트가 판단하게 하세요:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 모델 — 143개 전부

`GET /v1/models`는 라이브 키에서 **108개 id**를 응답했습니다(2026-10-21). 카탈로그 id는 그룹 태그가 붙어 있으며(`[次]deepseek-v4.1-flash`, `[官方]gemini-…`, `[vertex特价]gemini-4-argon`), **접두사 없는 id도 해석됩니다**; 다음은 처음부터 끝까지 채팅으로 검증되었습니다:

| 모델 ID | 비고 |
| --- | --- |
| `deepseek-v4.1-flash` | 검증 기본값 — 세 개의 독립 출구에서 200 `"MX-PONG"` |

`/api/pricing` 피드는 143개 행을 나열합니다: `deepseek-v4-flash`, `gemini-2.5-flash`, `glm-4.6`, `gpt-4o-mini`, `claude-haiku-4-5`, `qwen3.8-flash`, `MiniMax-M3`, `kimi-k3`, `grok-4.6` 등, 각각 위 그룹 중 하나 이상에 속합니다.

## 🧪 API 사용법

베이스 URL **`https://www.moxin.studio/v1`** — OpenAI 호환:

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

출석 체크로 키를 충전하세요(API 키가 아닌 세션 토큰): `Authorization: Bearer <access_token>`와 함께 `POST /api/user/checkin`.

## 🛠 빠른 시작

```bash
cd www-moxin-studio-bulk-apikey
node public-cli.js --count 3        # three accounts: e-mail code -> check-in -> key -> chat-200
node public-cli.js --verify         # re-test every key in keys.txt
node public-cli.js --pool proxies.txt   # optional: one exit per account
```

브라우저도, 캡차 솔버도, npm install도 필요 없습니다. `keys.txt`는 **실제 완성** 이후에만 작성되며, `accounts.jsonl`은 계정별 출석 체크 증거(`quotaBefore` → `quotaAwarded` → `quota`)를 보관합니다.

### 🖱️ 런처

## ✅ 검증됨 — 모든 키가 테스트됩니다

아래 모든 검사는 **2026-10-21**에 출구 풀에서(WARP 없이) 다시 실행되었습니다:

| 검사 | 결과 |
| --- | --- |
| `GET /api/status` | **200** — `register_enabled`+`password_register_enabled` true, `email_verification=true`, **`turnstile_check=false`**, `checkin_enabled=true`, 143행 가격 |
| 등록 | **✔** `GET /api/verification?email=<gmail>` 200 → 코드 → `POST /api/user/register {username,password,password2,email,verification_code}` 200 |
| 출석 체크 충전 | **✔** `POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}` ⇒ `quota 0 → 54 438` |
| 키 | **✔** `POST /api/token/` → `GET /api/token/?p=0&size=20` → **`POST /api/token/{id}/key`**가 전체 키를 공개 |
| 라이브 채팅 | **✔** `deepseek-v4.1-flash` → 200 `"MX-PONG"`, **세 개의 독립 출구**(`http://51.15.20.32:3129`, `socks5://183.106.215.208:1080`, `http://3.224.130.91:3128`)에서 재입증 |
| 규모 | **✔ 10개 계정 생성, 10/10 키가 실제 완성에 응답**(`deepseek-v4.1-flash` → 200 `"ok"`), 계정당 하나의 출구; 관측된 출석 체크 지급: `12 275 … 79 697` quota (약 $0.025–$0.16), 즉 **일일 크레딧은 무작위입니다** |

## 🧭 작동 방식

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: Moxin Studio page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://www.moxin.studio/v1/chat/completions → real content = the key is live
```

| 단계 | 호출 | 비고 |
| --- | --- | --- |
| 메일함 | 공용 Gmail 풀 | 패널은 Gmail급 주소를 허용합니다; **재활용된** 메일함은 이미 패널에 존재할 수 있습니다(`Email address is already in use`) — CLI는 새 메일함으로 재시도합니다 |
| 코드 | `GET /api/verification?email=…` 후 메일함 읽기 | ⚠ **브랜드 필터는 발신자**(`moxin|studio|墨信`)를 기준으로 해야 합니다: 공유 메일함에는 다른 패널의 코드(`OpenArt.ai`, `WhatsToken.AI` …)도 들어 있고, 일반적인 "verification code" 제목은 증거가 아닙니다 — 필터를 강화하기 전에는 둘 다 `Verification code is incorrect or has expired`를 발생시켰습니다 |
| 등록 | `POST /api/user/register {username,password,password2,email,verification_code}` | 응답에 자체 `access_token`이 있을 수 있습니다; 없으면 `POST /api/user/login {username,password}`(사용자명 또는 이메일 모두 작동) |
| 충전 | `POST /api/user/checkin` | 핵심: 계정당 하루 `quota_awarded ≈ 54 438` |
| 키 | `POST /api/token/` → 목록 → `POST /api/token/{id}/key` | 이 포크는 모든 읽기에서 키를 마스킹하며, `…/{id}/key` 라우트가 이를 공개합니다 |
| 게이트 | `POST /v1/chat/completions` | 실제 콘텐츠가 돌아온 후에만 키가 `keys.txt`에 기록됩니다 |

## ⚙️ CLI 옵션

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

## 🛰️ 프록시 뒤에서

**선택 사항.** `--pool <file>`(또는 `MOXIN_STUDIO_PROXIES`)은 한 줄에 하나의 출구를 받습니다 — `http://host:port`, `https://host:port`(CONNECT) 또는 `socks5://host:port`(선택적 `user:pass@`) — 그리고 CLI는 계정당 하나의 출구를 사용합니다; 풀이 없으면 직접 실행됩니다. 내장 배관은 `net`/`tls` 약 80줄이며 라이브러리가 없습니다.

## 📄 파일

| 경로 | 내용 |
| --- | --- |
| `keys.txt` | 라이브 검증된 키 한 줄에 하나 |
| `accounts.jsonl` | 이메일, 키, 출석 체크 증거(`quotaBefore` / `quotaAwarded` / `quota`), 모델, 출구 |
| `proxies.txt` | 사용자의 출구, 선택 사항 |
| `out/` | 시도별 실행 기록 |

## 🔌 9Router에 연결

[9Router](https://9router.com)는 자체 호스팅, OpenAI 호환 라우터입니다:

```bash
node public-cli.js --connect-9router --router9 http://localhost:20128 --password 123456
```

기본값: 접두사 `moxin-studio`, 베이스 `https://www.moxin.studio/v1`, 모델 `deepseek-v4.1-flash`; 모델은 `moxin-studio/<model id>`로 제공됩니다.

## 🔑 키 하나로 얻는 것

| | |
| --- | --- |
| 🏢 제공자 | **Moxin Studio** — [www.moxin.studio](https://www.moxin.studio) |
| 🎁 제공자가 주는 것 | **가입 보너스 없음** — 일일 출석 체크가 **계정당 하루 약 $0.109**를 지급 |
| 🔑 키 하나로 얻는 것 | `https://www.moxin.studio/v1`의 143행 카탈로그(라이브 키에서 108개 id 표시) |
| 📦 이 저장소 | **10개 키, 전부 채팅 검증됨** 2026-10-21 기준(계정당 하나의 출구) |
| 🌐 API 형태 | OpenAI 호환: `POST https://www.moxin.studio/v1/chat/completions` |
| 🛡️ 게이트 | **캡차 없음**, 이메일 코드만; 공유 메일함에서는 발신자 브랜드 필터가 필요 |

## 🐞 이슈

패널이 등록/출석 체크 라우트, 이메일 정책 또는 카탈로그를 변경하면 도구는 그 지점에서 멈추고 그렇게 알립니다. 명령과 정확한 출력을 포함하여 **[이슈를 열어주세요](../../issues)**.

## 📄 라이선스

MIT © 2026 — [LICENSE](LICENSE) 참조.

<div align="center">

**[MIT](LICENSE)** © 2026 www-moxin-studio-bulk-apikey contributors

</div>

---

## ⚠️ 계정 위험과 최소화
