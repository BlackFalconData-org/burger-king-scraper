# Burger King Scraper [💰$2]

Extract structured data from [bk.com](https://bk.com) — bk.com — records. Every record carries phone, address and price the source publishes. Price is included where the source lists it. Clean rows, ready to import into a CRM or a spreadsheet. The actor supports keyword search and controllable result limits.

**[Burger King Scraper [💰$2] on Apify →](https://apify.com/blackfalcondata/burger-king-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword. Filter by 📝 description format, and more.

**Detail enrichment** — Fetch full descriptions, structured metadata for each product.

**Incremental mode** — Only get new or changed products since your last run. Content hash per product — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how products evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N products (up to 100.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every product returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured products from bk.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor products, track trends, and analyze market dynamics with structured, deduplicated data from bk.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per product is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "Miami, FL",
  "maxStores": 5,
  "maxResults": 1000,
  "radiusKm": 25,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Where to look for restaurants — an address, city or ZIP code. Example: "Miami, FL" or "10001". Pass a JSON array of strings to search several places in one run. |
| `maxStores` | integer | `5` | Maximum number of restaurants to take per search (0 = every restaurant inside the radius). |
| `storeIds` | array | — | Burger King restaurant numbers (for example 12427) to fetch directly — no address needed. Combined with any address or start-URL search; each restaurant is returned once. |
| `includeUnavailable` | boolean | `true` | Also return menu items the restaurant lists but does not sell right now (no price, isAvailable: false). Turn off to get only orderable items. |
| `maxResults` | integer | `1000` | Maximum total menu items returned across all restaurants (0 = unlimited). |
| `lat` | number | — | Search centre latitude. Overrides the address lookup when paired with longitude. |
| `lon` | number | — | Search centre longitude. Pair with latitude. |
| `radiusKm` | integer | `25` | How far around the search centre to look for restaurants. |
| `includeDetails` | boolean | `true` | Enrich each menu item with the full catalog record (internal names, timestamps, vendor configuration). |
| `descriptionMaxLength` | integer | `0` | Truncate item descriptions to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from your search inputs (places, coordinates, radius) so different searches never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental mode is on, skip items whose content matches an item that disappeared from the menu in a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit menu items whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit menu items that were seen before but are no longer on the menu. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED items. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of items included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |
| `startUrls` | array | — | One or more bk.com restaurant pages (https://www.bk.com/store-locator/store/restaurant_…, as linked from the bk.com store locator). Each page returns that restaurant's full menu. When set, the address search is ignored. |

---

## Output fields

Every product returns the same 53-field schema. Missing values are `null` — never omitted.

- `listingId`
- `type`
- `storeId`
- `storeNumber`
- `storeName`
- `storeAddress`
- `storeCity`
- `storeState`
- `storePostalCode`
- `storeCountry`
- `storeLatitude`
- `storeLongitude`
- `storePhone`
- `storeStatus`
- `hasDriveThru`
- `storeUrl`
- `searchedAddress`
- `itemId`
- `itemName`
- `itemType`
- `sectionName`
- `sections`
- `description`
- `dayParts`
- `price`
- `pickupPrice`
- `deliveryPrice`
- `currency`
- `isAvailable`
- `calories`
- `caloriesMin`
- `caloriesMax`
- `nutrition`
- `allergens`
- `resolvedItemId`
- `variants`
- `modifierGroups`
- `comboItems`
- `imageUrl`
- `productUrl`
- `portalUrl`
- `internalName`
- `operationalItemId`
- `vendorConfigs`
- `createdAt`
- `updatedAt`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per product. Here is a real example from a production run:

```json
{
  "listingId": "dd8e15ec91a22dc544fe6185c23c2f498d934bdf5de0a012aa91e9b2a3e60ddf",
  "type": "menu-item",
  "storeId": "17389",
  "storeNumber": "17389",
  "storeName": "2700 University Blvd BIRMINGHAM, Alabama 35233-3422 - USA",
  "storeAddress": "2700 University Blvd",
  "storeCity": "BIRMINGHAM",
  "storeState": "Alabama",
  "storePostalCode": "35233-3422",
  "storeCountry": "USA",
  "storeLatitude": 33.509809,
  "storeLongitude": -86.789221
}
```

*Truncated — full records contain 53 fields. See Output fields for the complete schema.*

**[Try Burger King Scraper [💰$2] now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/burger-king-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.002 |

See the [actor on Apify](https://apify.com/blackfalcondata/burger-king-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape bk.com?**
Use this actor on Apify to extract structured data from bk.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get bk.com data as JSON, CSV, or Excel?**
The actor writes each product to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape bk.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check bk.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each product gets a content hash. On subsequent runs, only new or changed products are emitted — saving time, compute, and storage. Expired products can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Burger King Scraper [💰$2]](https://apify.com/blackfalcondata/burger-king-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 11*
