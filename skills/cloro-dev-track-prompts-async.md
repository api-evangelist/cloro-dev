---
generated: '2026-10-07'
method: generated
name: track-prompts-async
description: Queue hundreds of prompts in batches with idempotency keys, receive signed webhooks or poll, watch the queue, and clear it if needed.
api: openapi/cloro-dev-openapi.yml
operations:
- createBatchAsyncTasks
- createAsyncTask
- getTaskStatus
- getAsyncStatus
- clearAsyncQueue
- getCredits
source: 'Grounded in openapi/cloro-dev-openapi.yml (operationIds verified verbatim) and the provider docs: https://cloro.dev/docs/guides/making-requests/sync, /async, /webhooks, /concurrency, /error-handling, /billing; provider-published skill saved verbatim at skills/_original/cloro-skill.md'
---

# Track many prompts across engines with async tasks and webhooks

Queue hundreds of prompts in batches with idempotency keys, receive signed webhooks or poll, watch the queue, and clear it if needed.

## Steps

1. **Build the batch**: up to 500 items per `createBatchAsyncTasks` (`POST /v1/async/task/batch`), each `{ "taskType": "CHATGPT", "idempotencyKey": "<your job id>", "priority": 1-10, "webhook": { "url": "https://..." }, "payload": { "prompt": "...", "country": "US" } }`. `payload` takes the same parameters as the provider's sync request. For a single prompt use `createAsyncTask` (`POST /v1/async/task`).
2. **Check every per-task result**: the batch endpoint is partial-success - a `200` carries `results[]` where an item can be `success: false` with `VALIDATION_ERROR`, `RESOURCE_ALREADY_EXISTS` or `INSUFFICIENT_CREDITS`. Store each `task.id` immediately.
3. **Receive results**: with `webhook.url` cloro POSTs the finished task (`COMPLETED` or `FAILED`) and retries failed deliveries up to 5 times (~2/4/8/16 min). Verify `X-Cloro-Signature` (HMAC-SHA256 over the raw body, secret `whsec_...`, enable in the dashboard) and deduplicate on `task.id` (asyncapi/cloro-dev-webhooks.yml). Without a webhook, poll `getTaskStatus` (`GET /v1/async/task/{taskId}`).
4. **Watch the queue** with `getAsyncStatus` (`GET /v1/async/status`): queue depth by priority and concurrency usage. Your plan's concurrency limit caps how many tasks process in parallel; the queue holds up to 100,000 `QUEUED` tasks (`429 QUEUE_LIMIT_EXCEEDED` past that).
5. **Reverse if needed**: `clearAsyncQueue` (`DELETE /v1/async/queue`) discards tasks still `QUEUED`; a `PROCESSING` task is not stopped (conventions/reversibility).
6. **Budget**: credits are charged only on `COMPLETED`; `FAILED` costs nothing. `getCredits` reflects completed charges only, so subtract your own outstanding `creditsToCharge` before deciding a submission fits.

## Rules

- Resubmitting with the same `idempotencyKey` returns `409 RESOURCE_ALREADY_EXISTS` and does not create a second task; the key stays bound until the task record is deleted ~24 hours after creation. Use a new key to retry a failed task.
- Finished tasks are deleted about 24 hours after creation and HTML URLs in a result expire after 24 hours: fetch and store results promptly.
- Webhooks arrive in completion order, not submission order.
