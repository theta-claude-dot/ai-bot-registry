# AI bot registry

Which crawlers and fetchers of AI companies visit a website, how to recognise
them by user agent, and how to verify them by IP. Maintained by the
[Parallel World](https://pw.migurei.ai/?utm_source=github) lab
(migurei.ai), which measures how AI apps find, read and cite websites in Japan.
The files are exported from the lab's registry and kept up to date by Theta,
the Parallel World research agent.

日本語は下にあります。

## Research / 調査

What these bots actually did on our own site, counted from its access logs
(each page links its English version):
このデータのボットが当社の検証用サイトで実際に何をしたかを、アクセスログから数えた記録です。

- https://pw.migurei.ai/ja/research/oai-searchbot-reads-only-robots
  OAI-SearchBot reads only robots.txt and fetches no page: daily counts, comparison with GPTBot, ShapBot and KeenableBot, what we tried.
  OAI-SearchBot が robots.txt だけを読み、ページを取得しない記録（日ごとの回数、他のボットとの比較、試した対策）。
- https://pw.migurei.ai/ja/research/ai-bot-first-visits
  When AI search bots first came to a new subdomain: first-visit time and pages fetched per bot over 14 days.
  新しいサブドメインに AI 検索ボットが初めて来た時刻と、ボットごとの取得ページ数（14 日間）。
- [LLMO and GEO measurement tools compared](https://pw.migurei.ai/en/compare?src=gh) /
  [LLMO・GEO 測定ツール比較](https://pw.migurei.ai/compare?src=gh)

## Files

| File | Content |
|---|---|
| `bots.json` | Bots: operator, purpose, user-agent pattern, how to verify, official documentation |
| `ai-referrers.json` | AI assistants: referrer hosts and `utm_source` values seen when a person clicks a link in an AI answer |

Every entry links to the operator's own page. Nothing here is guessed from
traffic alone: a bot is listed with the operator's published name and, where
one exists, its published IP list.

## Categories (`category`)

| Value | Meaning |
|---|---|
| `training` | Collects pages to train models (e.g. GPTBot, ClaudeBot, CCBot) |
| `search_index` | Builds the search index an AI app answers from (e.g. OAI-SearchBot, Claude-SearchBot, PerplexityBot, Googlebot, bingbot) |
| `user_fetch` | Fetches a page because a person asked the app to (e.g. ChatGPT-User, Claude-User, Perplexity-User) |
| `other` | Known non-AI tools: SEO crawlers, link previews, scanners |
| `unknown` | Claims to be an AI bot, but the operator publishes no purpose or IP list |

## Verifying a request

A user agent can be copied by anyone. `verify_method` says how to check:

- `ip_feed`: the source IP must fall inside one of the ranges in the bot's
  `feeds` (JSON published by the operator; fetch them yourself, they change).
- `none`: the operator publishes no IP list; treat the user agent as a claim.

Order matters: the first bot whose `ua_pattern` matches wins (patterns are
JavaScript regular expressions, case-insensitive unless `flags` says otherwise).
`generic_bot.ua_patterns` catches automated clients that match no named bot.

`needs_review: true` marks an entry whose details the operator does not fully
publish (for example a full user-agent string we observed from the operator's
own IP range but could not find in its documentation).

## Versions

`version` and `effective_from` name the registry version the files were
exported from. Older versions are not overwritten in our lab; this repository
holds the latest.

## License

Data: [CC BY 4.0](LICENSE). Please credit "Parallel World (migurei.ai)".

---

# AI ボット・レジストリ

AI 企業のクローラーとフェッチャーについて、サイトを訪れるボットの一覧、ユーザーエージェントでの見分け方、IP での確認方法をまとめたデータです。日本で AI アプリがサイトをどう見つけ、読み、引用するかを計測している [Parallel World](https://pw.migurei.ai/?utm_source=github) のラボ（migurei.ai）が公開・更新しています。ファイルはラボのレジストリから書き出したもので、Parallel World の調査エージェント Theta が更新しています。調査記録へのリンクは上の「Research / 調査」にあります。

## ファイル

| ファイル | 内容 |
|---|---|
| `bots.json` | ボット：運営元、目的、ユーザーエージェントのパターン、確認方法、公式ドキュメント |
| `ai-referrers.json` | AI アシスタント：AI の回答内のリンクを人がクリックしたときのリファラーのホストと `utm_source` の値 |

各項目には運営元の公式ページへのリンクがあります。アクセスログだけを根拠に登録した項目はなく、運営元が公表している名称と、公開されている場合は IP リストに基づいています。

## 分類（`category`）

| 値 | 意味 |
|---|---|
| `training` | モデルの学習用にページを収集（例：GPTBot、ClaudeBot、CCBot） |
| `search_index` | AI アプリが回答に使う検索インデックスを構築（例：OAI-SearchBot、Claude-SearchBot、PerplexityBot、Googlebot、bingbot） |
| `user_fetch` | ユーザーの依頼を受けて、アプリがページを取得（例：ChatGPT-User、Claude-User、Perplexity-User） |
| `other` | AI 以外の既知のツール：SEO クローラー、リンクプレビュー、スキャナー |
| `unknown` | AI ボットを名乗るが、運営元が目的も IP リストも公開していないもの |

## リクエストの確認

ユーザーエージェントは誰でも偽装できます。確認方法は `verify_method` に記載しています。

- `ip_feed`：送信元 IP が、そのボットの `feeds`（運営元が公開する JSON）の範囲内にあること。範囲は随時変わるため、最新のものを各自で取得してください。
- `none`：運営元が IP リストを公開していません。ユーザーエージェントは自己申告として扱ってください。

`bots.json` の記載順に照合し、最初に `ua_pattern` が一致したボットを採用します（JavaScript の正規表現。`flags` で指定しない限り、大文字小文字は区別されません）。`generic_bot.ua_patterns` は、名前付きのどのボットにも一致しない自動化クライアントを拾います。

`needs_review: true` は、運営元が詳細を十分に公開していない項目です（例：運営元の IP 範囲から観測したが、公式ドキュメントに記載のないユーザーエージェント文字列）。

## バージョン

`version` と `effective_from` は、書き出し元のレジストリのバージョンです。ラボでは過去のバージョンを上書きせず保管しており、このリポジトリには最新版を置いています。

## ライセンス

データ：[CC BY 4.0](LICENSE)。利用の際は「Parallel World (migurei.ai)」のクレジットを表示してください。
