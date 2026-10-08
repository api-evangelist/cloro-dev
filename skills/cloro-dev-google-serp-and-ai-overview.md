---
generated: '2026-10-07'
method: generated
name: google-serp-and-ai-overview
description: Run a Google search (organic results, sponsored ads, shopping cards, People Also Ask, local pack, AI Overview) or a Google News search from a chosen country, and resolve Google redirect links.
api: openapi/cloro-dev-openapi.yml
operations:
- getCountries
- monitorGoogle
- monitorGoogleNews
- resolveGoogleGoto
source: 'Grounded in openapi/cloro-dev-openapi.yml (operationIds verified verbatim) and the provider docs: https://cloro.dev/docs/guides/making-requests/sync, /async, /webhooks, /concurrency, /error-handling, /billing; provider-published skill saved verbatim at skills/_original/cloro-skill.md'
---

# Pull Google Search results, AI Overview and Google News for a query

Run a Google search (organic results, sponsored ads, shopping cards, People Also Ask, local pack, AI Overview) or a Google News search from a chosen country, and resolve Google redirect links.

## Steps

1. **Pick the geo** with `getCountries` (`GET /v1/countries?model=google`); Google requests take `gl` for the result geography and optional `hl` for interface language (`country` still works as a deprecated alias for `gl`).
2. **Search** with `monitorGoogle` (`POST /v1/monitor/google`): either a `query` plus `gl`, or a complete `google.com/search` URL carrying the query and pagination. Add `include.aioverview` to get the AI Overview block (`result.aiOverview` with sources, places and map). Each extra results page costs +2 credits on top of the 3-credit base.
3. **News** with `monitorGoogleNews` (`POST /v1/monitor/google/news`): `query` plus geography; returns article titles, snippets, sources and times.
4. **Resolve redirect links**: results ship Google `/goto` links beside their destination as `redirectLink`; when you need the final URL for one, call `resolveGoogleGoto` (`POST /v1/monitor/google/goto`). It is excluded from concurrency limits and returns `502` when Google answers without a redirect.
5. **Read** `result.organicResults[]`, `result.sponsoredAds[]`, `result.shoppingCards[]`, `result.peopleAlsoAsk[]`, `result.localPack`, `result.relatedSearches[]`, `result.knowledgeGraph` as present.

## Rules

- Same bearer-key auth, 1,000 RPS per endpoint, plan concurrency and `429` handling as every other monitor endpoint (rate-limits/, errors/).
- Credits are charged on `success: true` even when optional fields such as ads or shopping cards come back empty - an empty field is a valid extraction.
- No idempotency on sync requests; give calls a 300 s client timeout.
