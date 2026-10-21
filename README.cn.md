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

**批量创建 Moxin Studio（new-api）账户和密钥 —— 在 https://www.moxin.studio/v1 上进行 OpenAI 兼容推理，由面板的每日签到提供资金。**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-143%20served-412991?style=for-the-badge)](https://www.moxin.studio/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 快速开始](#-quick-start) · [🎁 免费额度](#-free-tier--how-much-you-get) · [🤖 模型](#-models--all-143-of-them) · [🧪 使用 API](#-how-to-use-the-api) · [✅ 已验证](#-verified-working--every-key-is-tested) · [🧭 工作原理](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ 选项](#️-cli-options) · [🛰️ 代理](#️-behind-a-proxy) · [🐞 问题反馈](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ 为什么会有这个项目

Moxin Studio 是 new-api 的一个分支（`v1.0.0-rc.45`，`system_name "studio"`），**完全没有验证码**（`turnstile_check=false`），注册使用邮箱验证码 —— 但**没有注册奖励**。让一个账户有价值的是**每日签到**：`POST /api/user/checkin` 在 2026-10-21 支付了 `{"quota_awarded":54438}`（按 `quota_per_unit 500000` 计算约 **$0.109**），所以一个账户价值一天的额度，如果你每天签到就会持续获得。其背后的目录包含 **143 个模型**（DeepSeek · Gemini · Claude · GPT · GLM · Qwen · MiniMax …… 以分组标记的行形式呈现，如 `[次]deepseek-v4.1-flash`），在 `https://www.moxin.studio/v1` 上以 OpenAI 兼容方式提供服务。它的兄弟主机 `www.moxin.plus`（"墨信AI"，`rc.44`）是一个**独立的数据库** —— 在这里生成的密钥在那里会返回 `Invalid token`。

## 🎁 免费额度 —— 你能获得多少

| 项目 | 值 |
| --- | --- |
| 注册奖励 | **无** —— 新账户从 `quota 0` 开始（测量了两次：2026-10-18 的账户和今天的新账户） |
| 每日签到 | **每个账户每天随机，观察到 12 275 … 79 697 quota（约 $0.025–$0.16；中位数约 54 000 ≈ $0.11）**（`POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}`）；同一天第二次调用会返回 `Already checked in today` |
| 目录 | 143 行，**0 个严格免费**（`model_ratio 0` + `model_price 0`）：一切都是付费的，签到额度就是你花费的东西 |
| 验证码 | **无**（`turnstile_check=false`）—— 纯 HTTP，无需浏览器，无需打码器 |
| 邮箱 | `email_verification=true`，验证码有效期 10 分钟；实际上**只有 Gmail 类地址**可用（公共 gmail 池） |
| 分组 | `default`（比率 1）· `DeepSeek[羊毛] 0.35` · `Codex 0.2` · `Claude[特价] 0.6` · `Claude[折扣] 1.2` · `Claude[高质] 2` · `GPT[官方] 5` · `Gemini…` |
| 速率/过期 | 没有公布的每密钥限制；签到额度不会过期 |

> 💡 免费额度由 Moxin Studio 设定，可能会变化。在 [www.moxin.studio](https://www.moxin.studio) 查看实时数字 —— 并让工具自己的关卡来决定：

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 模型 —— 全部 143 个

`GET /v1/models` 在一个活跃密钥上返回了 **108 个 id**（2026-10-21）。目录 id 带有分组标记（`[次]deepseek-v4.1-flash`、`[官方]gemini-…`、`[vertex特价]gemini-4-argon`），并且**无前缀的 id 也能解析**；以下这些经过了端到端的聊天验证：

| 模型 ID | 备注 |
| --- | --- |
| `deepseek-v4.1-flash` | 验证默认值 —— 从三个独立出口返回 200 `"MX-PONG"` |

`/api/pricing` 源列出了 143 行：`deepseek-v4-flash`、`gemini-2.5-flash`、`glm-4.6`、`gpt-4o-mini`、`claude-haiku-4-5`、`qwen3.8-flash`、`MiniMax-M3`、`kimi-k3`、`grok-4.6` 等等，每一个都位于上述一个或多个分组中。

## 🧪 如何使用 API

基础 URL **`https://www.moxin.studio/v1`** —— OpenAI 兼容：

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

通过签到为密钥充值（使用会话令牌，而非 API 密钥）：`POST /api/user/checkin`，带 `Authorization: Bearer <access_token>`。

## 🛠 快速开始

```bash
cd www-moxin-studio-bulk-apikey
node public-cli.js --count 3        # three accounts: e-mail code -> check-in -> key -> chat-200
node public-cli.js --verify         # re-test every key in keys.txt
node public-cli.js --pool proxies.txt   # optional: one exit per account
```

无需浏览器，无需打码器，无需 npm install。`keys.txt` 只在**真实完成**之后才会写入，而 `accounts.jsonl` 保留每个账户的签到证据（`quotaBefore` → `quotaAwarded` → `quota`）。

### 🖱️ 启动器

## ✅ 已验证可用 —— 每个密钥都经过测试

以下所有检查均在 **2026-10-21** 从出口池（无 WARP）重新运行：

| 检查 | 结果 |
| --- | --- |
| `GET /api/status` | **200** —— `register_enabled`+`password_register_enabled` 为 true，`email_verification=true`，**`turnstile_check=false`**，`checkin_enabled=true`，143 行定价 |
| 注册 | **✔** `GET /api/verification?email=<gmail>` 200 → 验证码 → `POST /api/user/register {username,password,password2,email,verification_code}` 200 |
| 签到充值 | **✔** `POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}` ⇒ `quota 0 → 54 438` |
| 密钥 | **✔** `POST /api/token/` → `GET /api/token/?p=0&size=20` → **`POST /api/token/{id}/key`** 揭示完整密钥 |
| 实时聊天 | **✔** `deepseek-v4.1-flash` → 200 `"MX-PONG"`，从**三个独立出口**重新证明（`http://51.15.20.32:3129`、`socks5://183.106.215.208:1080`、`http://3.224.130.91:3128`） |
| 规模 | **✔ 铸造了 10 个账户，10/10 密钥都能响应真实完成**（`deepseek-v4.1-flash` → 200 `"ok"`），每个账户一个出口；观察到的签到奖励：`12 275 … 79 697` quota（约 $0.025–$0.16），即**每日额度是随机的** |

## 🧭 工作原理

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: Moxin Studio page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://www.moxin.studio/v1/chat/completions → real content = the key is live
```

| 步骤 | 调用 | 备注 |
| --- | --- | --- |
| 邮箱 | 公共 Gmail 池 | 面板接受 Gmail 类地址；一个**被回收的**邮箱可能已经存在于面板上（`Email address is already in use`）—— CLI 会用新邮箱重试 |
| 验证码 | `GET /api/verification?email=…` 然后读取收件箱 | ⚠ **品牌过滤必须以发件人为准**（`moxin|studio|墨信`）：共享收件箱也携带其他面板的验证码（`OpenArt.ai`、`WhatsToken.AI` ……），而通用的"验证码"主题并不能作为证据 —— 在过滤收紧之前，两者都产生了 `Verification code is incorrect or has expired` |
| 注册 | `POST /api/user/register {username,password,password2,email,verification_code}` | 响应可能自带 `access_token`；否则 `POST /api/user/login {username,password}`（用户名或邮箱都可以） |
| 充值 | `POST /api/user/checkin` | 关键所在：每个账户每天 `quota_awarded ≈ 54 438` |
| 密钥 | `POST /api/token/` → 列表 → `POST /api/token/{id}/key` | 这个分支在每次读取时都会遮蔽密钥，`…/{id}/key` 路由会揭示它 |
| 关卡 | `POST /v1/chat/completions` | 只有在返回真实内容之后，密钥才会被写入 `keys.txt` |

## ⚙️ CLI 选项

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

## 🛰️ 在代理后面

**可选。** `--pool <file>`（或 `MOXIN_STUDIO_PROXIES`）接受每行一个出口 —— `http://host:port`、`https://host:port`（CONNECT）或 `socks5://host:port`（可选 `user:pass@`）—— CLI 为每个账户取一个出口；没有池则直接运行。内置的管道是约 80 行的 `net`/`tls`，没有库。

## 📄 文件

| 路径 | 内容 |
| --- | --- |
| `keys.txt` | 每行一个经过实时验证的密钥 |
| `accounts.jsonl` | 邮箱、密钥、签到证据（`quotaBefore` / `quotaAwarded` / `quota`）、模型、出口 |
| `proxies.txt` | 你的出口，可选 |
| `out/` | 每次尝试的运行记录 |

## 🔌 连接到 9Router

[9Router](https://9router.com) 是一个自托管的、OpenAI 兼容的路由器：

```bash
node public-cli.js --connect-9router --router9 http://localhost:20128 --password 123456
```

默认值：前缀 `moxin-studio`，基础 `https://www.moxin.studio/v1`，模型 `deepseek-v4.1-flash`；模型以 `moxin-studio/<model id>` 形式提供。

## 🔑 一个密钥能给你什么

| | |
| --- | --- |
| 🏢 提供商 | **Moxin Studio** —— [www.moxin.studio](https://www.moxin.studio) |
| 🎁 提供商给予什么 | **无注册奖励** —— 每日签到支付**每个账户每天约 $0.109** |
| 🔑 一个密钥能给你什么 | `https://www.moxin.studio/v1` 上的 143 行目录（活跃密钥上可见 108 个 id） |
| 📦 本仓库 | **10 个密钥，全部经过聊天验证**，2026-10-21（每个账户一个出口） |
| 🌐 API 形态 | OpenAI 兼容：`POST https://www.moxin.studio/v1/chat/completions` |
| 🛡️ 关卡 | **无验证码**，仅邮箱验证码；共享邮箱上需要发件人品牌过滤 |

## 🐞 问题反馈

如果面板更改了其注册/签到路由、邮箱策略或目录，工具会停在那里并说明原因。**[提交 issue](../../issues)**，附上命令和确切的输出。

## 📄 许可证

MIT © 2026 —— 见 [LICENSE](LICENSE)。

<div align="center">

**[MIT](LICENSE)** © 2026 www-moxin-studio-bulk-apikey contributors

</div>

---

## ⚠️ 账号风险与降低方法
