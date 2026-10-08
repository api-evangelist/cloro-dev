---
generated: '2026-10-07'
method: generated
name: monitor-ai-answer
description: Ask ChatGPT, Gemini, Perplexity, Copilot or Google AI Mode a prompt from a chosen country and read the answer text and the sources it cites, synchronously.
api: openapi/cloro-dev-openapi.yml
operations:
- getCountries
- getStates
- monitorChatgpt
- monitorPerplexity
- monitorGemini
- monitorCopilot
- monitorAiMode
- getCredits
source: 'Grounded in openapi/cloro-dev-openapi.yml (operationIds verified verbatim) and the provider docs: https://cloro.dev/docs/guides/making-requests/sync, /async, /webhooks, /concurrency, /error-handling, /billing; provider-published skill saved verbatim at skills/_original/cloro-skill.md'
---

# Monitor an AI answer engine for a prompt

Ask ChatGPT, Gemini, Perplexity, Copilot or Google AI Mode a prompt from a chosen country and read the answer text and the sources it cites, synchronously.

## Steps

1. **Pick the geo.** `getCountries` (`GET /v1/countries?model=chatgpt`) lists the UPPERCASE ISO 3166-1 alpha-2 codes the engine serves; `getStates` (`GET /v1/states?country=US`) lists state codes for state-level targeting. A lowercase code is rejected with `400`.
2. **Check the balance** with `getCredits` (`GET /v1/credits`) if the workload is large. Sync requests cost the provider rate plus a `+2` sync surcharge (ChatGPT full response 7 credits, Perplexity 4, Gemini 4, Copilot 5, AI Mode 4 - async rates; see plans/).
3. **Submit the prompt** with one of `monitorChatgpt`, `monitorPerplexity`, `monitorGemini`, `monitorCopilot`, `monitorAiMode` (`POST /v1/monitor/{provider}`) and a body of `{ "prompt": "...", "country": "US" }`. Leave `include` unset for the leanest response; add `include.markdown`, `include.searchQueries`, `include.shoppingCards` or `include.ads` only when needed.
4. **Give the call a client timeout of at least 300 s.** A sync attempt can run up to five minutes server-side; closing the connection early does not stop it and the retry you send is a new, separately billed request.
5. **Read the result**: `success: true` and `result.text`, `result.sources[]` (citations), plus `result.entities`, `result.shoppingCards`, `result.ads` when present. Read `X-Credits-Charged` / `X-Credits-Remaining` from the headers.
6. **Detect an empty answer structurally**: `sources` empty AND the body under ~160 characters means the engine returned a canned refusal. Retry once (not for age-restricted or policy topics); it was a successful, billed delivery.

## Rules

- Authentication is `Authorization: Bearer <api-key>` (authentication/). One key grants every endpoint; there are no scopes and no sandbox keys.
- 1,000 requests per second per endpoint per key, plus plan concurrency slots (Free = 1). A `429` carries `RATE_LIMIT_EXCEEDED` or `CONCURRENCY_LIMIT_EXCEEDED` and no `Retry-After`: back off exponentially (rate-limits/).
- Sync requests have NO idempotency key (conventions/): never fire-and-retry blindly.
- `monitorGrok` exists in the contract but Grok is temporarily unavailable; do not route to it.
