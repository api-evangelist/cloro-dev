---
name: cloro
description: Use when building integrations to monitor AI provider responses and
  search results. Reach for this skill when you need to extract structured data
  from ChatGPT, Google Search, Perplexity, Copilot, Gemini, Grok, AI Mode, or
  Google News; when you're building monitoring systems, market research tools,
  or SEO tracking applications; or when you need to handle async task queuing,
  webhooks, or concurrent API requests at scale.
metadata:
  mintlify-proj: cloro
  version: "1.0"
---

# Cloro Skill

## Product summary

Cloro is a REST API platform for monitoring AI responses and search results across multiple providers (ChatGPT, Google Search, Perplexity, Copilot, Gemini, Grok, AI Mode, Google News). It returns structured data—parsed markdown, sources, citations, shopping cards, ads, entities—as JSON. Requests run through real browser sessions routed through geo-proxies, not direct API calls, so responses reflect what users actually see in each region.

**Base URL**: `https://api.cloro.dev`  
**Authentication**: Bearer token in `Authorization` header  
**Key files**: API key from dashboard at `https://dashboard.cloro.dev`  
**CLI**: No CLI; use curl, HTTP clients, or language SDKs (Python, TypeScript, Node.js)  
**Primary docs**: https://cloro.dev/docs

## When to use

- **Monitoring AI responses**: Track how ChatGPT, Perplexity, Copilot, or Gemini answer queries across countries and regions
- **Search result extraction**: Scrape Google Search, Google News, or AI Overview for SEO monitoring, market research, or competitive analysis
- **Structured data extraction**: Get shopping cards, ads, entities, citations, and sources from AI provider responses
- **Geo-targeted monitoring**: Test how responses differ by country (and US state) using the `country` and `state` parameters
- **High-volume processing**: Submit batches of up to 500 tasks, use async queuing, or run concurrent workers for large workloads
- **Webhook-driven workflows**: Process results asynchronously via webhooks instead of polling
- **Credit-aware applications**: Track credit spend per request and manage budget across multiple endpoints

## Quick reference

### Request structure (synchronous)

```json
{
  "prompt": "Your query (1-10,000 characters)",
  "country": "US",
  "include": {
    "markdown": false,
    "html": false,
    "shopping": false,
    "ads": false,
    "searchQueries": false
  }
}
```

### Common parameters

| Parameter | Required | Notes |
|-----------|----------|-------|
| `prompt` | Yes | Query text; use `query` for Google Search instead |
| `country` | Yes | ISO 3166-1 alpha-2 uppercase (e.g., `"US"`, `"GB"`, `"DE"`) |
| `state` | No | US state code (e.g., `"CA"`); supported on ChatGPT, Copilot, Perplexity, Gemini |
| `include.markdown` | No | Include markdown-formatted response (no extra cost) |
| `include.html` | No | Include CDN URL to full scraped page (no extra cost; expires 24h) |

### Endpoints by provider

| Provider | Base Cost | Endpoint | Best for |
|----------|-----------|----------|----------|
| ChatGPT | 5 credits | `/v1/monitor/chatgpt` | E-commerce, shopping data, web search |
| Google Search | 3 credits | `/v1/monitor/google` | SEO, SERPs, multi-page results |
| Perplexity | 4 credits | `/v1/monitor/perplexity` | Real-time research, citations |
| Copilot | 5 credits | `/v1/monitor/copilot` | Microsoft ecosystem, web search |
| Gemini | 4 credits | `/v1/monitor/gemini` | General reasoning, state targeting |
| AI Mode | 4 credits | `/v1/monitor/aimode` | General knowledge, product results |
| Google News | 3 credits | `/v1/monitor/google-news` | News monitoring, media tracking |
| Grok | ~~4 credits~~ | ~~`/v1/monitor/grok`~~ | ~~Current events~~ (currently unavailable) |

### Response structure

```json
{
  "success": true,
  "result": {
    "text": "AI response text",
    "sources": [
      {
        "position": 1,
        "url": "https://example.com",
        "label": "Article Title",
        "description": "Snippet..."
      }
    ],
    "html": "https://cdn.cloro.dev/results/...",
    "markdown": "Markdown version...",
    "shoppingCards": [],
    "ads": [],
    "citationPills": []
  }
}
```

### Response headers (sync requests)

| Header | Purpose |
|--------|---------|
| `X-Request-Id` | Unique request ID for support lookups |
| `X-Credits-Charged` | Credits billed for this request |
| `X-Credits-Remaining` | Account balance after this request |
| `X-Concurrent-Limit` | Your plan's concurrent slot limit |
| `X-Concurrent-Current` | Requests currently processing |
| `X-Concurrent-Remaining` | Available slots when request arrived |
| `X-RateLimit-Limit` | 1,000 requests/sec per endpoint |
| `X-RateLimit-Remaining` | Requests left in current second |
| `X-Latency-Ms` | API processing time in milliseconds |

### Async task submission

```json
{
  "taskType": "CHATGPT",
  "priority": 5,
  "idempotencyKey": "your-unique-id-123",
  "webhook": {
    "url": "https://your-app.com/webhook-handler"
  },
  "payload": {
    "prompt": "Your query",
    "country": "US"
  }
}
```

**Task states**: `QUEUED` → `PROCESSING` → `COMPLETED` or `FAILED`  
**Queue limit**: 100,000 tasks per organization  
**Retention**: Tasks and HTML URLs expire 24 hours after completion  
**Cost**: Async is 2 credits cheaper than sync (no +2 surcharge)

## Decision guidance

### When to use sync vs. async

| Scenario | Choose | Why |
|----------|--------|-----|
| Need immediate answer in same request | Sync | Returns result in HTTP response |
| Serverless function with short timeout | Async | Submit task, get taskId, process later |
| Submitting 50+ requests at once | Async + batch | Batch endpoint takes up to 500 tasks per call |
| Real-time user-facing request | Sync | User waits for result |
| Background job or bulk processing | Async + webhook | Fire and forget, receive results on webhook |
| Small batch, need results now | Sync + concurrent workers | Run multiple workers within concurrency limit |

### When to use which provider

| Use case | Provider |
|----------|----------|
| Product research, shopping data | ChatGPT (has shopping cards) |
| SEO monitoring, SERP analysis | Google Search |
| Real-time research with citations | Perplexity |
| Microsoft ecosystem integration | Copilot |
| General reasoning, state-level targeting | Gemini |
| General knowledge without login wall | AI Mode |
| News monitoring, media tracking | Google News |

### Concurrency patterns

| Pattern | Use case | Complexity |
|---------|----------|-----------|
| Async + webhooks | Large batches (100+), non-time-sensitive | Low—cloro handles queuing |
| Concurrent workers | Real-time batches (10-50), need results now | Medium—manage your own workers |
| Single sync request | One-off queries | Minimal |

## Workflow

### 1. Set up authentication

- Get API key from https://dashboard.cloro.dev
- Store in environment variable: `CLORO_API_KEY`
- Include in every request: `Authorization: Bearer YOUR_API_KEY`

### 2. Choose request mode

- **Sync**: Need result immediately? Use `/v1/monitor/{provider}`
- **Async**: Submitting many tasks or in serverless? Use `/v1/async/task` or `/v1/async/task/batch`

### 3. Build and send request

For sync:
```bash
curl -X POST "https://api.cloro.dev/v1/monitor/chatgpt" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"prompt": "Your query", "country": "US"}'
```

For async:
```bash
curl -X POST "https://api.cloro.dev/v1/async/task" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "taskType": "CHATGPT",
    "webhook": {"url": "https://your-app.com/webhook"},
    "payload": {"prompt": "Your query", "country": "US"}
  }'
```

### 4. Handle response

**Sync**: Parse `result` object immediately; check `X-Credits-Charged` header  
**Async**: Store `task.id`; wait for webhook or poll `/v1/async/task/{taskId}`

### 5. Monitor usage

- Track `X-Credits-Remaining` on sync requests
- Call `GET /v1/credits` for async-only workloads
- Watch `X-Concurrent-Remaining` to avoid 429 errors
- Check `X-RateLimit-Remaining` if hitting 1,000 RPS limit

### 6. Verify results

- Check `success: true` in response
- Inspect `result.sources` to confirm data was extracted
- For empty responses, retry once (provider may have returned no answer)
- Store `X-Request-Id` for support lookups

## Common gotchas

- **Country code must be uppercase**: `"US"` works, `"us"` returns validation error
- **Google Search uses `query` not `prompt`**: Other endpoints use `prompt`
- **Sync requests have +2 credit surcharge**: Async is cheaper; use async for bulk work
- **HTML URLs expire after 24 hours**: Download and store if you need long-term access
- **Empty responses are not errors**: Provider returned no answer; retry once, but don't retry policy/age-restricted topics
- **Concurrency limit is hard**: 11th simultaneous request on a 10-slot plan gets 429 immediately, not queued
- **Idempotency keys must be unique**: Resubmitting with same key within 24 hours returns 409 conflict
- **Failed async tasks don't release idempotency key for 24 hours**: Use new key to retry within that window
- **Webhooks can arrive out of order**: Don't assume batch results arrive in submission order
- **Webhook retries can deliver duplicates**: Deduplicate on `task.id` in your handler
- **Mobile-web ChatGPT responses lack metadata**: `result.model`, `result.searchQueries`, `result.mapSearchQueries` may be empty
- **Shopping cards only appear on product-related prompts**: "Best laptops" works; "What is photosynthesis" won't return cards
- **State targeting only works for US**: `state` parameter ignored for non-US countries
- **Grok endpoint is currently unavailable**: Check status page before building against it
- **Canceled sync requests are still charged**: Don't close connections early to save credits

## Verification checklist

Before submitting work:

- [ ] API key is set and not exposed in code (use environment variable)
- [ ] Country code is uppercase ISO 3166-1 alpha-2 (e.g., `"US"`, not `"us"`)
- [ ] Prompt is 1–10,000 characters (or `query` for Google Search)
- [ ] Response has `"success": true` (not false or missing)
- [ ] `result.sources` is populated (or expected to be empty for certain queries)
- [ ] For async: `task.id` is stored and webhook URL is reachable
- [ ] For async: webhook handler responds with `2xx` status code
- [ ] Credit balance is sufficient for planned workload (check `X-Credits-Remaining` or `GET /v1/credits`)
- [ ] Concurrency usage is within plan limits (check `X-Concurrent-Remaining` or `GET /v1/async/status`)
- [ ] Error handling covers 401 (auth), 403 (credits), 429 (rate/concurrency), 500/502 (upstream)
- [ ] Retry logic includes exponential backoff for transient failures
- [ ] For webhooks: signature verification is enabled if performing sensitive operations

## Resources

- **[llms.txt](https://cloro.dev/docs/llms.txt)** — Complete page index for AI assistant navigation
- **[Making requests guide](https://cloro.dev/docs/guides/making-requests)** — Sync vs. async comparison and when to use each
- **[Providers & pricing](https://cloro.dev/docs/guides/providers)** — Credit costs, feature support matrix, and provider comparison
- **[Error handling guide](https://cloro.dev/docs/guides/error-handling)** — HTTP status codes, error codes, retry strategies, and troubleshooting

---

> For additional documentation and navigation, see: https://cloro.dev/docs/llms.txt