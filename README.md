# cloro

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

cloro is one API for Google Search and AI answer engines: a REST surface at api.cloro.dev that submits a prompt to ChatGPT, Gemini, Google AI Mode, Bing AI Mode, Google Search, Google AI Overview and Google News from a chosen country (and US state) and returns the engine's answer as structured JSON with parsed text, markdown, cited sources, brand entities, shopping cards and ads. Sync monitor endpoints answer in-line; async tasks and batches of up to 500 run in the background with polling or signed webhooks. Credit-based subscription billing, a free tier of 500 monthly credits, a hosted MCP server at mcp.cloro.dev with OAuth, official Python and TypeScript SDKs, n8n, Zapier and LangChain integrations.

**Areas:** Company · Search · Artificial Intelligence · Web Scraping · SERP · Generative Engine Optimization · SEO · Brand Monitoring · Market Research · Data Extraction · MCP

## cloro on APIs.io

Everything profiled for cloro is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/cloro-dev/](https://apis.io/providers/cloro-dev/)** — or go straight to one of the APIs or properties below.

### APIs (6)

- [cloro MCP server](https://apis.io/apis/cloro-dev/cloro-mcp-server/) — Hosted Model Context Protocol server (Streamable HTTP) wrapping the sync monitor endpoints of api.cloro.dev as ten tools: scrape_chatgpt, scrape_gemini,…
- [cloro Async API](https://apis.io/apis/cloro-dev/cloro-dev-async-api/) — The Async API from cloro — 5 operation(s) for async.
- [cloro Countries API](https://apis.io/apis/cloro-dev/cloro-dev-countries-api/) — The Countries API from cloro — 1 operation(s) for countries.
- [cloro Credits API](https://apis.io/apis/cloro-dev/cloro-dev-credits-api/) — The Credits API from cloro — 1 operation(s) for credits.
- [cloro Monitor API](https://apis.io/apis/cloro-dev/cloro-dev-monitor-api/) — The Monitor API from cloro — 9 operation(s) for monitor.
- [cloro States API](https://apis.io/apis/cloro-dev/cloro-dev-states-api/) — The States API from cloro — 1 operation(s) for states.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Cloro Dev Webhooks](https://apis.io/asyncapis/cloro-dev/cloro-dev-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [cloro](https://apis.io/servers/cloro-dev/cloro/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Cloro Dev Agentic Access](https://apis.io/agentic-access/cloro-dev/cloro-dev-agentic-access/)

#### JSON Schema (4)

The data models the API reads and writes.

- [AiModeMonitorResponse](https://apis.io/schemas/cloro-dev/cloro-dev-ai-mode-monitor-response/)
- [ChatGPTMonitorResponse](https://apis.io/schemas/cloro-dev/cloro-dev-chat-gptmonitor-response/)
- [GeminiMonitorResponse](https://apis.io/schemas/cloro-dev/cloro-dev-gemini-monitor-response/)
- [GoogleMonitorResponse](https://apis.io/schemas/cloro-dev/cloro-dev-google-monitor-response/)

#### JSON-LD (1)

Linked-data contexts for the provider's vocabulary.

- [Cloro Dev Context](https://apis.io/jsonld/cloro-dev/cloro-dev-context/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Cloro Dev Openapi Overlay](https://apis.io/overlays/cloro-dev/cloro-dev-openapi-overlay/)

#### Governance rules (1)

Spectral rulesets for the provider's contracts.

- [cloro API Rules](https://apis.io/rules/cloro-dev/cloro-dev-rules/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Cloro Dev Authentication](https://apis.io/security/cloro-dev/cloro-dev-authentication/)
- [Cloro Dev Domain Security](https://apis.io/security/cloro-dev/cloro-dev-domain-security/)
- [Cloro Dev Vulnerability Disclosure](https://apis.io/security/cloro-dev/cloro-dev-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Cloro Dev Scopes](https://apis.io/scopes/cloro-dev/cloro-dev-scopes/)

#### Plans and pricing (2)

Published plans, tiers and prices.

- [Cloro Dev Plans Pricing](https://apis.io/plans/cloro-dev/cloro-dev-plans-pricing/)
- [Cloro Dev Price Estimates](https://apis.io/plans/cloro-dev/cloro-dev-price-estimates/)

#### Rate limits (1)

Published request limits.

- [Cloro Dev Rate Limits](https://apis.io/rate-limits/cloro-dev/cloro-dev-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Cloro Dev Finops](https://apis.io/finops/cloro-dev/cloro-dev-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://cloro.dev)
- [Documentation](https://cloro.dev/docs)
- [Getting started](https://cloro.dev/docs/introduction/quickstart)
- [API reference](https://cloro.dev/docs/api-reference/introduction)
- [Pricing](https://cloro.dev/pricing/)
- [Sign up](https://dashboard.cloro.dev/sign-up)
- [Support](https://cloro.dev/contact/)
- [Changelog](https://cloro.dev/docs/changelog)
- [Blog](https://cloro.dev/blog/)
- [Terms of service](https://cloro.dev/terms/)
- [Privacy policy](https://cloro.dev/privacy/)
- [GitHub](https://github.com/cloro-dev)
- [Status](https://status.cloro.dev)

## API Evangelist rating

**Kin Score 76.2** — exemplar · rubric 0.26.0 · scored 2026-11-02

| Facet | Score |
|---|---|
| Discoverability | 76.7 |
| Contract quality | 74.9 |
| Contract governance | 35.6 |
| Operational transparency | 84.2 |
| Developer ergonomics | 75.6 |
| Access clarity | 84.2 |

**Agent readiness 67.0** — agent-native

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | documented (partial credit) |
| Auth Clarity | served (partial credit) |
| Idempotency | verified |
| Error Semantics | verified |
| OpenAPI Examples | verified |
| Rate Limit Signal | verified |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | yes |
| Consent Identity | — |
| Agent Card | conformant |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |
| MCP Tool Quality | na |

**Regulatory alignment 37.3** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/cloro-dev/](https://apis.io/providers/cloro-dev/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for cloro: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/cloro-dev/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
