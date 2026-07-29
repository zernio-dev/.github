<div align="center">

<img src="https://zernio.com/brand/logo-primary.svg" alt="Zernio" width="300">

### Ship social features, not sixteen OAuth integrations.

One REST API for publishing, inbox, analytics and paid ads across **16 social platforms** and **7 ad networks**.<br>
The API is closed. Everything we build on top of it is open, and it is all here.

[Docs](https://docs.zernio.com/?ref=github-org-readme) •
[OpenAPI spec](https://zernio.com/openapi.yaml) •
[MCP server](https://docs.zernio.com/mcp?ref=github-org-readme) •
[Get an API key](https://zernio.com/signup?ref=github-org-readme) •
[Status](https://status.zernio.com)

</div>

```bash
npm install @zernio/node
```

```typescript
import Zernio from '@zernio/node';

const zernio = new Zernio(); // reads ZERNIO_API_KEY

const { data: post } = await zernio.posts.createPost({
  body: {
    content: 'Hello world from Zernio!',
    platforms: [
      { platform: 'twitter', accountId: 'acc_xxx' },
      { platform: 'linkedin', accountId: 'acc_yyy' },
      { platform: 'instagram', accountId: 'acc_zzz' },
    ],
    publishNow: true,
  },
});
```

[![@zernio/node](https://img.shields.io/npm/v/%40zernio%2Fnode?label=%40zernio%2Fnode&color=EB3514)](https://www.npmjs.com/package/@zernio/node)
[![zernio-sdk](https://img.shields.io/pypi/v/zernio-sdk?label=zernio-sdk&color=EB3514)](https://pypi.org/project/zernio-sdk/)
[![OpenAPI 3.1](https://img.shields.io/badge/OpenAPI-3.1-EB3514)](https://zernio.com/openapi.yaml)
[![Docs](https://img.shields.io/badge/docs-zernio.com-555)](https://docs.zernio.com/?ref=github-org-readme)

No SDK, no problem. Base URL `https://zernio.com/api/v1`, bearer token, that is the whole contract:

```bash
# 1. Which accounts can this key post to?
curl https://zernio.com/api/v1/accounts \
  -H "Authorization: Bearer $ZERNIO_API_KEY"

# 2. Schedule a post to one of them.
#    scheduledFor is any future ISO 8601 UTC timestamp; omit it and pass
#    "publishNow": true to send immediately.
curl -X POST https://zernio.com/api/v1/posts \
  -H "Authorization: Bearer $ZERNIO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Hello from Zernio",
    "platforms": [{ "platform": "twitter", "accountId": "ACCOUNT_ID" }],
    "scheduledFor": "2027-01-15T15:00:00.000Z"
  }'
```

## Platforms

Same request, same auth, same error contract for every one of them. You change one `platform` value, not one integration.

| Platform | `platform` value |
|---|---|
| [Twitter / X](https://docs.zernio.com/platforms/twitter?ref=github-org-readme) | `twitter` |
| [Instagram](https://docs.zernio.com/platforms/instagram?ref=github-org-readme) | `instagram` |
| [Facebook](https://docs.zernio.com/platforms/facebook?ref=github-org-readme) | `facebook` |
| [LinkedIn](https://docs.zernio.com/platforms/linkedin?ref=github-org-readme) | `linkedin` |
| [TikTok](https://docs.zernio.com/platforms/tiktok?ref=github-org-readme) | `tiktok` |
| [YouTube](https://docs.zernio.com/platforms/youtube?ref=github-org-readme) | `youtube` |
| [WhatsApp](https://docs.zernio.com/platforms/whatsapp?ref=github-org-readme) | `whatsapp` |
| [Pinterest](https://docs.zernio.com/platforms/pinterest?ref=github-org-readme) | `pinterest` |
| [Reddit](https://docs.zernio.com/platforms/reddit?ref=github-org-readme) | `reddit` |
| [Bluesky](https://docs.zernio.com/platforms/bluesky?ref=github-org-readme) | `bluesky` |
| [Threads](https://docs.zernio.com/platforms/threads?ref=github-org-readme) | `threads` |
| [Google Business](https://docs.zernio.com/platforms/google-business?ref=github-org-readme) | `googlebusiness` |
| [Telegram](https://docs.zernio.com/platforms/telegram?ref=github-org-readme) | `telegram` |
| [Snapchat](https://docs.zernio.com/platforms/snapchat?ref=github-org-readme) | `snapchat` |
| [Discord](https://docs.zernio.com/platforms/discord?ref=github-org-readme) | `discord` |
| [Slack](https://docs.zernio.com/platforms/slack?ref=github-org-readme) | `slack` |

**Ad networks:** `metaads`, `googleads`, `tiktokads`, `linkedinads`, `pinterestads`, `xads`, [`openaiads`](https://docs.zernio.com/platforms/openai-ads?ref=github-org-readme).

> [!NOTE]
> Platform list and enum values come from the `/v1/connect/{platform}` enum in the [published OpenAPI spec](https://zernio.com/openapi.yaml), which is the contract that decides what you can actually connect. Some SDK READMEs lag a platform behind; the enum is the contract.

## What you can call

One key, one base URL, 369 paths in the published spec. A sample of the surface:

<!-- Recompute the path count with: curl -s https://zernio.com/openapi.yaml | grep -cE '^  /v1/'
     It only ever grows, so a stale number understates rather than overstates. -->

| Area | Endpoints | Does |
|---|---|---|
| **Publish** | `POST /v1/posts` · `/v1/posts/bulk-upload` · `/v1/posts/{postId}/retry` · `/v1/media/presign` | Schedule or publish now, per-platform overrides, retries, direct media upload |
| **Queue** | `/v1/queue/slots` · `/v1/queue/next-slot` · `/v1/queue/preview` | Slot-based scheduling without picking timestamps by hand |
| **Inbox** | `/v1/inbox/conversations` · `/v1/inbox/comments` · `/v1/inbox/mentions` · `/v1/inbox/reviews` | DMs, comments, mentions and reviews from every network in one shape |
| **Analytics** | `/v1/analytics` · `/v1/analytics/best-time` · `/v1/analytics/content-decay` · `/v1/analytics/daily-metrics` | Post and account insights, demographics, best time to post |
| **Ads** | `/v1/ads/campaigns` · `/v1/ads/creatives` · `/v1/ads/audiences` · `/v1/ads/insights` · `/v1/ads/lead-forms` | Campaigns, creatives, targeting, conversions and lead gen across 7 ad networks |
| **WhatsApp** | `/v1/whatsapp/templates` · `/v1/whatsapp/flows` · `/v1/whatsapp/phone-numbers` · `/v1/whatsapp/calls` | Templates, Flows, number provisioning with KYC, calling, sandbox |
| **CRM** | `/v1/contacts` · `/v1/broadcasts` · `/v1/sequences` · `/v1/comment-automations` | Contacts, custom fields, broadcasts, drip sequences, auto-replies |
| **Plumbing** | `/v1/connect/{platform}` · `/v1/webhooks/settings` · `/v1/api-keys` · `/v1/usage` | Hosted OAuth, webhooks with delivery logs, keys, usage metering |

> [!TIP]
> Building multi-tenant? Start at the [multi-tenant guide](https://docs.zernio.com/multi-tenant?ref=github-org-readme). Prefer to generate your own client? The [spec](https://zernio.com/openapi.yaml) is public and complete.

## SDKs

<!-- Curation rule: one row per officially maintained client library. Do not add a row until the
     package is published and the install command in it actually works. -->

| Language | Install | Version |
|---|---|---|
| [Node.js](https://github.com/zernio-dev/zernio-node) | `npm install @zernio/node` | [![npm](https://img.shields.io/npm/v/%40zernio%2Fnode?label=&color=EB3514)](https://www.npmjs.com/package/@zernio/node) |
| [Python](https://github.com/zernio-dev/zernio-python) | `pip install zernio-sdk` | [![PyPI](https://img.shields.io/pypi/v/zernio-sdk?label=&color=EB3514)](https://pypi.org/project/zernio-sdk/) |
| [Go](https://github.com/zernio-dev/zernio-go) | `go get github.com/zernio-dev/zernio-go` | [![Go](https://img.shields.io/github/v/tag/zernio-dev/zernio-go?label=&color=EB3514)](https://github.com/zernio-dev/zernio-go/tags) |
| [Ruby](https://github.com/zernio-dev/zernio-ruby) | `gem install zernio-sdk` | [![Gem](https://img.shields.io/gem/v/zernio-sdk?label=&color=EB3514)](https://rubygems.org/gems/zernio-sdk) |
| [PHP](https://github.com/zernio-dev/zernio-php) | `composer require zernio-dev/zernio-php` | [![Packagist](https://img.shields.io/packagist/v/zernio-dev/zernio-php?label=&color=EB3514)](https://packagist.org/packages/zernio-dev/zernio-php) |
| [Rust](https://github.com/zernio-dev/zernio-rust) | `cargo add zernio` | [![crates.io](https://img.shields.io/crates/v/zernio?label=&color=EB3514)](https://crates.io/crates/zernio) |
| [.NET](https://github.com/zernio-dev/zernio-dotnet) | `dotnet add package Zernio` | [![NuGet](https://img.shields.io/nuget/v/Zernio?label=&color=EB3514)](https://www.nuget.org/packages/Zernio) |
| [Java](https://github.com/zernio-dev/zernio-java) | build from source, see the repo README | not on Maven Central yet |
| [CLI](https://github.com/zernio-dev/zernio-cli) | `npm install -g @zernio/cli` | [![npm](https://img.shields.io/npm/v/%40zernio%2Fcli?label=&color=EB3514)](https://www.npmjs.com/package/@zernio/cli) |

<details>
<summary><strong>Python quickstart</strong></summary>

<br>

```python
from zernio import Zernio

# Reads ZERNIO_API_KEY from environment (or pass explicitly)
client = Zernio()

post = client.posts.create(
    content="Hello world from Zernio!",
    platforms=[
        {"platform": "twitter", "accountId": "acc_xxx"},
        {"platform": "linkedin", "accountId": "acc_yyy"},
        {"platform": "instagram", "accountId": "acc_zzz"},
    ],
    publish_now=True,
)
```

</details>

## Agents, editors and workflow tools

| Repo | What it is |
|---|---|
| [zernio-claude-plugin](https://github.com/zernio-dev/zernio-claude-plugin) | Claude Code plugin bundling the hosted MCP server at `https://mcp.zernio.com/mcp`. `/plugin marketplace add zernio-dev/zernio-claude-plugin`. [MCP docs](https://docs.zernio.com/mcp?ref=github-org-readme) |
| [zernio-cli](https://github.com/zernio-dev/zernio-cli) | Posts, inbox, broadcasts and automations from the terminal. JSON output by default, so it pipes into `jq` and into agents. [CLI docs](https://docs.zernio.com/cli?ref=github-org-readme) |
| [chat-sdk-adapter](https://github.com/zernio-dev/chat-sdk-adapter) | Official adapter for [Chat SDK](https://chat-sdk.dev/adapters/vendor-official/zernio), listed by the Chat SDK project. `npm install @zernio/chat-sdk-adapter` |
| [n8n-nodes-zernio](https://github.com/zernio-dev/n8n-nodes-zernio) | Community node for n8n. [![npm](https://img.shields.io/npm/v/n8n-nodes-zernio?label=&color=EB3514)](https://www.npmjs.com/package/n8n-nodes-zernio) |
| [cursor-plugin](https://github.com/zernio-dev/cursor-plugin) | The same MCP server, wired into Cursor |

## Apps built on the API

Real products, MIT licensed, deployable today. The fastest way to see what the API can carry.

| Project | Stars | What it is |
|---|---|---|
| **[zernflow](https://github.com/zernio-dev/zernflow)** | [![stars](https://img.shields.io/github/stars/zernio-dev/zernflow?style=flat&label=stars&color=EB3514)](https://github.com/zernio-dev/zernflow) | Open source ManyChat alternative. Visual flow builder, live chat inbox, contact CRM, broadcasts, sequences, A/B split. Live at [zernflow.com](https://zernflow.com) |
| **[latewiz](https://github.com/zernio-dev/latewiz)** | [![stars](https://img.shields.io/github/stars/zernio-dev/latewiz?style=flat&label=stars&color=EB3514)](https://github.com/zernio-dev/latewiz) | A complete scheduler UI: calendar, queue and composer. One-click deploy to Vercel or Railway. Live at [latewiz.com](https://latewiz.com) |
| **[unified-inbox](https://github.com/zernio-dev/unified-inbox)** | [![stars](https://img.shields.io/github/stars/zernio-dev/unified-inbox?style=flat&label=stars&color=EB3514)](https://github.com/zernio-dev/unified-inbox) | One inbox for WhatsApp, Instagram, Messenger, Telegram, X, Reddit and Bluesky. Stateless, no database, the key never reaches the browser |
| **[ads-dashboard](https://github.com/zernio-dev/ads-dashboard)** | [![stars](https://img.shields.io/github/stars/zernio-dev/ads-dashboard?style=flat&label=stars&color=EB3514)](https://github.com/zernio-dev/ads-dashboard) | Read-only ads reporting across Meta, Google, TikTok, LinkedIn, Pinterest, X and ChatGPT. Paste an API key, see your spend. Live at [ads-dashboard.zernio.com](https://ads-dashboard.zernio.com) |
| **[zernio-shopify](https://github.com/zernio-dev/zernio-shopify)** | [![stars](https://img.shields.io/github/stars/zernio-dev/zernio-shopify?style=flat&label=stars&color=EB3514)](https://github.com/zernio-dev/zernio-shopify) | Shopify app that turns products into scheduled posts, with templates, bulk scheduling and UTM tracking. Live at [store.zernio.com](https://store.zernio.com) |

<details>
<summary><strong>Also in this org</strong></summary>

<br>

<!-- Curation rule for future editors: the tables above are the supported set, not the org repo list
     (GitHub already renders that below this README). A repo is promoted out of this fold only when
     it is actively maintained, carries a LICENSE file, and is useful to someone integrating the API. -->

| Repo | What it is |
|---|---|
| [openapi-specs](https://github.com/zernio-dev/openapi-specs) | OpenAPI specs we maintain for the underlying platform APIs (Instagram, TikTok, LinkedIn and friends). Useful even if you never call Zernio |
| [zernio-api](https://github.com/zernio-dev/zernio-api) | Claude Code skill for the Zernio API |
| [social-media-api](https://github.com/zernio-dev/social-media-api) | Drop-in replacement for Ayrshare's `social-media-api` SDK, backed by Zernio. Source only, not published to npm yet |
| [crisp-mcp](https://github.com/zernio-dev/crisp-mcp) | MCP server for Crisp. Built for our own support stack, unrelated to the posting API, open sourced because people asked |

Anything not listed on this page is an experiment, a fork or an archive.

</details>

## Start

1. [Create an account](https://zernio.com/signup?ref=github-org-readme) and generate a key in the dashboard.
2. `export ZERNIO_API_KEY=sk_...`
3. Paste either snippet above.

The first two connected accounts are free, so step 3 works before you ever see a bill.

[Pricing](https://docs.zernio.com/pricing?ref=github-org-readme) ·
[Changelog](https://docs.zernio.com/changelog?ref=github-org-readme) ·
[Webhooks](https://docs.zernio.com/webhooks?ref=github-org-readme) ·
[SDK docs](https://docs.zernio.com/sdks?ref=github-org-readme) ·
[Status](https://status.zernio.com)

Bugs and feature requests go in the issue tracker of the relevant repo above.

<div align="center">

[<kbd> <br> Get an API key ➜ <br> </kbd>](https://zernio.com/signup?ref=github-org-readme)
&nbsp;&nbsp;
[<kbd> <br> Read the docs ➜ <br> </kbd>](https://docs.zernio.com/?ref=github-org-readme)

</div>

<!-- Platform counts here follow POSTING_PLATFORM_COUNT (16) and the /v1/connect/{platform} enum,
     which is the contract that decides what can actually be connected. Update both together. -->
