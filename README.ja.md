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

**Moxin Studio (new-api) のアカウントとキーを一括作成 — https://www.moxin.studio/v1 で OpenAI 互換の推論を、パネルの毎日チェックインで賄う。**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-143%20served-412991?style=for-the-badge)](https://www.moxin.studio/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 クイックスタート](#-quick-start) · [🎁 無料枠](#-free-tier--how-much-you-get) · [🤖 モデル](#-models--all-143-of-them) · [🧪 API の使い方](#-how-to-use-the-api) · [✅ 検証済み](#-verified-working--every-key-is-tested) · [🧭 仕組み](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [🔌 OmniRoute](#-connect-to-omniroute) · [⚙️ オプション](#️-cli-options) · [🛰️ プロキシ](#️-behind-a-proxy) · [🐞 イシュー](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ これが存在する理由

Moxin Studio は new-api のフォーク（`v1.0.0-rc.45`、`system_name "studio"`）で、**キャプチャは一切なし**（`turnstile_check=false`）、メールコードによるサインアップがある — ただし**サインアップボーナスはない**。アカウントを持つ価値があるのは**毎日のチェックイン**だ。`POST /api/user/checkin` は 2026-10-21 に `{"quota_awarded":54438}` を支払った（`quota_per_unit 500000` で約 **$0.109**）。つまり 1 アカウントは 1 日分のクレジットに相当し、毎日チェックインすれば支払いが続く。その背後にあるカタログは **143 モデル**（DeepSeek · Gemini · Claude · GPT · GLM · Qwen · MiniMax … が `[次]deepseek-v4.1-flash` のようなグループタグ付き行として並ぶ）で、`https://www.moxin.studio/v1` で OpenAI 互換として提供されている。姉妹ホスト `www.moxin.plus`（「墨信AI」、`rc.44`）は**別のデータベース**だ — ここで発行したキーはそちらでは `Invalid token` を返す。

## 🎁 無料枠 — いくら得られるか

| 項目 | 値 |
| --- | --- |
| サインアップボーナス | **なし** — 新規アカウントは `quota 0` から始まる（2 回計測: 2026-10-18 のアカウントと本日の新規アカウント） |
| 毎日のチェックイン | **アカウントごと・日ごとにランダム化、観測値 12 275 … 79 697 quota（約 $0.025–$0.16、中央値 ~54 000 ≈ $0.11）**（`POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}`）。同日 2 回目の呼び出しは `Already checked in today` を返す |
| カタログ | 143 行、**厳密な無料は 0**（`model_ratio 0` + `model_price 0`）: すべて有料で、チェックインのクレジットが使う原資 |
| キャプチャ | **なし**（`turnstile_check=false`）— 素の HTTP、ブラウザ不要、ソルバー不要 |
| メール | `email_verification=true`、コードは 10 分有効。実際には **Gmail クラスのアドレスのみ**機能する（公開 gmail プール） |
| グループ | `default`（比率 1）· `DeepSeek[羊毛] 0.35` · `Codex 0.2` · `Claude[特価] 0.6` · `Claude[折扣] 1.2` · `Claude[高質] 2` · `GPT[官方] 5` · `Gemini…` |
| レート/有効期限 | 公開されたキーごとの制限はなし。チェックインのクレジットは失効しない |

> 💡 無料枠は Moxin Studio が設定するもので、変更されうる。最新の数値は [www.moxin.studio](https://www.moxin.studio) で確認を — そしてツール自身のゲートに判断させよう:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 モデル — 143 すべて

`GET /v1/models` はライブキーで **108 個の id** を返した（2026-10-21）。カタログの id はグループタグ付き（`[次]deepseek-v4.1-flash`、`[官方]gemini-…`、`[vertex特価]gemini-4-argon`）で、**プレフィックスなしの id も解決される**。以下はエンドツーエンドでチャット検証済み:

| モデル ID | 備考 |
| --- | --- |
| `deepseek-v4.1-flash` | 検証のデフォルト — 3 つの独立した出口から 200 `"MX-PONG"` |

`/api/pricing` フィードは 143 行を列挙する: `deepseek-v4-flash`、`gemini-2.5-flash`、`glm-4.6`、`gpt-4o-mini`、`claude-haiku-4-5`、`qwen3.8-flash`、`MiniMax-M3`、`kimi-k3`、`grok-4.6` など、それぞれが上記グループの 1 つ以上に属する。

## 🧪 API の使い方

ベース URL **`https://www.moxin.studio/v1`** — OpenAI 互換:

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

チェックインでキーにチャージする（API キーではなくセッショントークンを使用）: `POST /api/user/checkin` に `Authorization: Bearer <access_token>` を付けて。

## 🛠 クイックスタート

```bash
cd www-moxin-studio-bulk-apikey
node public-cli.js --count 3        # three accounts: e-mail code -> check-in -> key -> chat-200
node public-cli.js --verify         # re-test every key in keys.txt
node public-cli.js --pool proxies.txt   # optional: one exit per account
```

ブラウザ不要、キャプチャソルバー不要、npm install 不要。`keys.txt` は**実際の補完**の後にのみ書き込まれ、`accounts.jsonl` はアカウントごとのチェックイン証跡（`quotaBefore` → `quotaAwarded` → `quota`）を保持する。

### 🖱️ ランチャー

## ✅ 動作検証済み — すべてのキーがテストされる

以下のチェックはすべて **2026-10-21** に出口プール（WARP なし）から再実行された:

| チェック | 結果 |
| --- | --- |
| `GET /api/status` | **200** — `register_enabled`+`password_register_enabled` true、`email_verification=true`、**`turnstile_check=false`**、`checkin_enabled=true`、143 行の pricing |
| 登録 | **✔** `GET /api/verification?email=<gmail>` 200 → コード → `POST /api/user/register {username,password,password2,email,verification_code}` 200 |
| チェックインによる資金供給 | **✔** `POST /api/user/checkin` → `{"checkin_date":"2026-10-22","quota_awarded":54438}` ⇒ `quota 0 → 54 438` |
| キー | **✔** `POST /api/token/` → `GET /api/token/?p=0&size=20` → **`POST /api/token/{id}/key`** が完全なキーを公開 |
| ライブチャット | **✔** `deepseek-v4.1-flash` → 200 `"MX-PONG"`、**3 つの独立した出口**（`http://51.15.20.32:3129`、`socks5://183.106.215.208:1080`、`http://3.224.130.91:3128`）から再証明 |
| スケール | **✔ 10 アカウントを発行、すべてのキーが実際の補完に応答**（`deepseek-v4.1-flash` → 200 `"ok"`）、アカウントごとに 1 つの出口。観測されたチェックイン報酬: `12 275 … 79 697` quota（約 $0.025–$0.16）、つまり **毎日のクレジットはランダム化されている** |

> **品質ゲート。** ここにあるすべてのキーは `keys.txt` に書き込まれる前にライブの `chat-200` を通過している
> （死んだキーは決して出荷されない）。ルーターのコンボもモデルレベルで同じルールに従う: エントリは
> **モデルが `chat-200` を返し、かつツールを呼び出せる場合にのみ**追加される — ツールサポートのないモデル、
> 死んだキー、死んだプロバイダは追加されずに除外される。検証ステップはいつでも再実行可能。
> 失敗は報告され、決して隠されない。

## 🧭 仕組み

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: Moxin Studio page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://www.moxin.studio/v1/chat/completions → real content = the key is live
```

| ステップ | 呼び出し | 備考 |
| --- | --- | --- |
| メールボックス | 公開 Gmail プール | パネルは Gmail クラスのアドレスを受け付ける。**リサイクルされた**ボックスは既にパネルに存在する可能性がある（`Email address is already in use`）— CLI は新しいボックスでリトライする |
| コード | `GET /api/verification?email=…` の後、受信箱を読む | ⚠ **ブランドフィルタは送信者を基準にしなければならない**（`moxin|studio|墨信`）: 共有受信箱には他パネルのコード（`OpenArt.ai`、`WhatsToken.AI` …）も届き、一般的な「verification code」という件名は証拠にならない — フィルタを厳格化する前は、どちらも `Verification code is incorrect or has expired` を引き起こした |
| 登録 | `POST /api/user/register {username,password,password2,email,verification_code}` | レスポンスが独自の `access_token` を含む場合がある。そうでなければ `POST /api/user/login {username,password}`（ユーザー名またはメールのどちらでも可） |
| 資金供給 | `POST /api/user/checkin` | まさに本題: 1 アカウント 1 日あたり `quota_awarded ≈ 54 438` |
| キー | `POST /api/token/` → 一覧 → `POST /api/token/{id}/key` | このフォークはすべての読み取りでキーをマスクするが、`…/{id}/key` ルートがそれを公開する |
| ゲート | `POST /v1/chat/completions` | 実際のコンテンツが返ってきた後にのみ、キーは `keys.txt` に書き込まれる |

## ⚙️ CLI オプション

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

## 🛰️ プロキシの背後で

**任意。** `--pool <file>`（または `MOXIN_STUDIO_PROXIES`）は 1 行につき 1 つの出口を受け付ける — `http://host:port`、`https://host:port`（CONNECT）、または `socks5://host:port`（任意で `user:pass@`）— そして CLI はアカウントごとに 1 つの出口を使う。プールがなければ直接実行する。組み込みの配管は `net`/`tls` の約 80 行で、ライブラリは使わない。

## 📄 ファイル

| パス | 内容 |
| --- | --- |
| `keys.txt` | 1 行につき 1 つのライブ検証済みキー |
| `accounts.jsonl` | メール、キー、チェックイン証跡（`quotaBefore` / `quotaAwarded` / `quota`）、モデル、出口 |
| `proxies.txt` | あなたの出口、任意 |
| `out/` | 試行ごとの実行記録 |

## 🔌 9Router に接続

[9Router](https://9router.com) はセルフホストの OpenAI 互換ルーター:

```bash
node public-cli.js --connect-9router --router9 http://localhost:20128 --password 123456
```

デフォルト: プレフィックス `moxin-studio`、ベース `https://www.moxin.studio/v1`、モデル `deepseek-v4.1-flash`。モデルは `moxin-studio/<model id>` として提供される。

## 🔌 OmniRoute に接続

[OmniRoute](https://omniroute.online) はセルフホストの OpenAI 互換ルーター:

```bash
node public-cli.js --connect-omniroute --router9 http://localhost:20128 --password 123456
```

デフォルト: プレフィックス `moxin-studio`、ベース `https://www.moxin.studio/v1`、モデル `deepseek-v4.1-flash`。モデルは `moxin-studio/<model id>` として提供される。

## 🔑 1 つのキーで得られるもの

| | |
| --- | --- |
| 🏢 プロバイダ | **Moxin Studio** — [www.moxin.studio](https://www.moxin.studio) |
| 🎁 プロバイダが提供するもの | **サインアップボーナスなし** — 毎日のチェックインが **1 アカウント 1 日あたり約 $0.109** を支払う |
| 🔑 1 つのキーで得られるもの | `https://www.moxin.studio/v1` の 143 行カタログ（ライブキーで 108 id が可視） |
| 📦 このリポジトリ | **すべてのキー、すべてチャット検証済み** 2026-10-21（アカウントごとに 1 つの出口） |
| 🌐 API 形式 | OpenAI 互換: `POST https://www.moxin.studio/v1/chat/completions` |
| 🛡️ ゲート | **キャプチャなし**、メールコードのみ。共有メールボックスでは送信者ブランドフィルタが必須 |

## 🐞 イシュー

パネルが登録/チェックインのルート、メールポリシー、またはカタログを変更した場合、ツールはそこで停止し、その旨を報告する。コマンドと正確な出力を添えて **[イシューを作成](../../issues)** してほしい。

## 📄 ライセンス

MIT © 2026 — [LICENSE](LICENSE) を参照。

<div align="center">

**[MIT](LICENSE)** © 2026 www-moxin-studio-bulk-apikey contributors

</div>