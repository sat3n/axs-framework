# AXS Scoring Methodology v0.1

## Overview

AXS is a composite, objective metric measuring how effectively a digital service supports AI agent interaction. Unlike survey-based metrics (NPS, CSAT), AXS is computed from observable, reproducible measurements across seven dimensions.

- **Score range:** 0–100
- **Reporting:** Overall AXS composite + per-dimension sub-scores
- **Assessment method:** Synthetic agent tasks + structured manual review
- **Licence:** Apache 2.0

---

## Dimension 1: Discoverability (0–15 points)

*Can an agent find and understand what actions are available?*

| Criterion | Points | How to Measure |
|-----------|--------|----------------|
| Machine-readable API spec exists (OpenAPI, MCP manifest, GraphQL schema) | 5 | Check for published spec. Score: 5 = complete and current, 3 = exists but incomplete, 1 = outdated, 0 = absent |
| Action descriptions are unambiguous and semantically clear | 4 | LLM comprehension test: give an agent only the spec/docs and ask it to describe what each endpoint does. Score by accuracy. |
| Available actions are enumerable without prior knowledge | 3 | Can an agent list all available operations from the spec/docs alone? 3 = yes fully, 2 = partially, 0 = no |
| Versioning and changelog are machine-readable | 3 | Check for structured changelog (JSON, RSS, or clearly formatted markdown). 3 = machine-readable and current, 1 = exists but not structured, 0 = absent |

---

## Dimension 2: Schema Quality (0–15 points)

*Are inputs, outputs, and data structures well-defined and consistent?*

| Criterion | Points | How to Measure |
|-----------|--------|----------------|
| All endpoints/tools have typed input/output schemas | 5 | Count typed vs untyped endpoints. 5 = 100%, 3 = 75%+, 1 = 50%+, 0 = below 50% |
| Schemas are internally consistent (no contradictions) | 3 | Automated schema validation against spec. 3 = no issues, 2 = minor issues, 0 = contradictions found |
| Response formats are predictable (same structure across calls) | 4 | Run same request N times. Compare response structures. 4 = identical, 2 = minor variance, 0 = unpredictable |
| Error responses follow a consistent, parseable format | 3 | Trigger 5+ different error types. Check format consistency. 3 = consistent structured format, 1 = partially consistent, 0 = inconsistent |

---

## Dimension 3: Reliability (0–15 points)

*Does the service work consistently under normal agent usage patterns?*

| Criterion | Points | How to Measure |
|-----------|--------|----------------|
| Successful task completion rate | 5 | Run N standardised agent tasks. Score: 5 = 95%+, 3 = 80%+, 1 = 60%+, 0 = below 60% |
| Uptime / availability during test window | 5 | Monitor availability across test period. 5 = no downtime, 3 = brief interruption, 0 = significant outage |
| Rate limiting is clearly communicated (not silent) | 3 | Hit rate limit deliberately. Does response include rate limit info (headers, error message)? 3 = clear and actionable, 1 = mentioned but vague, 0 = silent failure |
| Idempotency support for write operations | 2 | Test duplicate requests on write endpoints. 2 = idempotent with key support, 1 = partial, 0 = no support |

---

## Dimension 4: Recoverability (0–15 points)

*How gracefully does the service handle errors, edge cases, and agent mistakes?*

| Criterion | Points | How to Measure |
|-----------|--------|----------------|
| Error messages are actionable (tell the agent what to fix) | 5 | Send 5 malformed requests. For each error response, ask: does it tell the agent exactly what is wrong and how to fix it? 5 = all actionable, 3 = most actionable, 1 = some, 0 = cryptic |
| Partial failure handling | 3 | For batch operations: does a single item failure break the entire batch? 3 = individual items reported, 1 = partial info, 0 = entire batch fails silently |
| Retry guidance provided | 3 | Check for Retry-After headers, backoff hints, or documented retry strategy. 3 = explicit guidance, 1 = implicit, 0 = none |
| Graceful degradation under load | 4 | Send rapid sequential requests. 4 = clear rate limit response, 2 = degraded but functional, 0 = crash/timeout/gibberish |

---

## Dimension 5: Latency & Efficiency (0–15 points)

*How fast can an agent complete tasks?*

| Criterion | Points | How to Measure |
|-----------|--------|----------------|
| Time-to-first-byte for standard operations | 4 | Measure p50 and p95 response times. 4 = sub-500ms p95, 2 = sub-2s p95, 0 = above 5s |
| Total task completion time for multi-step workflows | 4 | Time the full standardised task. Compare across services in same category. 4 = fastest quartile, 2 = median, 0 = slowest quartile |
| Pagination/bulk operation support | 4 | Can the agent retrieve large datasets efficiently? 4 = cursor pagination + bulk endpoints, 2 = basic pagination, 0 = no pagination |
| Streaming support for long-running operations | 3 | Does the service support streaming or webhooks for async operations? 3 = yes, 1 = polling only, 0 = synchronous only with long waits |

---

## Dimension 6: Auth & Access (0–15 points)

*How painful is agent authentication and authorisation?*

| Criterion | Points | How to Measure |
|-----------|--------|----------------|
| Machine-to-machine auth supported | 5 | Is there API key, OAuth client credentials, or service account auth? 5 = M2M auth documented and working, 3 = exists but poorly documented, 0 = browser-only auth |
| No human-in-the-loop required for ongoing access | 4 | Can an agent maintain access without human intervention (no CAPTCHA, no MFA per call)? 4 = fully automated, 2 = initial human step then automated, 0 = recurring human steps |
| Scoped permissions available | 3 | Can permissions be limited to specific operations? 3 = granular scopes, 1 = broad scopes only, 0 = all-or-nothing |
| Token refresh is automated and documented | 3 | Is token lifecycle documented? Can an agent refresh without human intervention? 3 = automated refresh documented, 1 = manual refresh, 0 = no documentation |

---

## Dimension 7: Determinism & Predictability (0–10 points)

*Same input, same output?*

| Criterion | Points | How to Measure |
|-----------|--------|----------------|
| Identical requests produce identical responses | 4 | Send same request 5 times. Compare responses (excluding timestamps, request IDs). 4 = identical, 2 = minor variance, 0 = significant variance |
| Side effects are documented and predictable | 3 | Review docs for write operations. Are side effects (webhooks triggered, related records updated) documented? 3 = comprehensive, 1 = partial, 0 = undocumented |
| No undocumented behaviour changes | 3 | Compare behaviour at start and end of test period. Check for silent API changes, A/B tests affecting responses. 3 = stable, 1 = minor changes, 0 = undocumented changes observed |

---

## Composite Score

```
AXS = Sum of all dimension scores
Maximum: 100
```

### Tier Classification

| Tier | Score | Label | Interpretation |
|------|-------|-------|---------------|
| A+ | 90–100 | Agent-Native | Built for agents from the ground up |
| A | 80–89 | Agent-Ready | Fully functional for agent use with minor friction |
| B | 65–79 | Agent-Capable | Works for agents but with notable gaps |
| C | 50–64 | Agent-Tolerant | Agents can use it but will hit regular friction |
| D | 35–49 | Agent-Hostile | Significant barriers to agent use |
| F | 0–34 | Agent-Incompatible | Effectively unusable by agents |

### Minimum Dimension Floor

A service cannot be classified at any tier above F if any single dimension scores below 40% of its maximum. This prevents a service from achieving a high composite score while being completely broken in one area.

Floor thresholds per dimension:

| Dimension | Max | 40% Floor |
|-----------|-----|-----------|
| Discoverability | 15 | 6 |
| Schema Quality | 15 | 6 |
| Reliability | 15 | 6 |
| Recoverability | 15 | 6 |
| Latency & Efficiency | 15 | 6 |
| Auth & Access | 15 | 6 |
| Determinism | 10 | 4 |

If a service breaches any floor, its tier is capped at F regardless of composite score. The scorecard must note which dimension(s) triggered the floor cap.

---

## Default Dimension Weights

All dimensions are weighted equally at their raw point values by default (15-15-15-15-15-15-10). No universal weighting adjustment is applied.

**Rationale:** The temptation is to universally upweight gating dimensions like Auth & Access (if an agent cannot authenticate, nothing else matters) or Discoverability (if an agent cannot find the API, nothing else works). But this logic applies symmetrically to every dimension: if the service is down (Reliability), authentication is irrelevant. Gating conditions are handled by the minimum dimension floor, not by universal bias. Equal defaults keep the framework simple, defensible, and category-neutral. Category-specific weights (see [category-weights.md](category-weights.md)) handle domain-specific priorities where they genuinely differ.

Determinism is set at 10 (not 15) because it is a quality signal rather than a functional gating condition. An agent can complete tasks with minor non-determinism; it cannot complete tasks without authentication or discoverability.

---

## Anti-Gaming Policy

Goodhart's Law applies: when a measure becomes a target, it ceases to be a good measure. The following mechanisms keep AXS scores correlated with real Agent Experience, not benchmark-specific optimisation.

### 1. Task rotation

Standardised tasks per category are published in `benchmarks/[category]/tasks.md`. However:

- The task set is refreshed every 6 months. New tasks are added; at least one existing task is retired or modified per cycle.
- Assessors are encouraged to include 1 **undocumented probe task** per assessment: a reasonable task within the service's domain that is not in the published set. This task is scored but reported separately as an `[probe]` result. Probe tasks test whether AX quality extends beyond rehearsed paths.

### 2. Multi-agent verification

Benchmark results submitted for the official leaderboard must be tested with at least two different agent models (e.g. Claude + GPT, or Claude + Gemini). A service that scores well with one agent but poorly with another has optimised for a specific model's behaviour, not for general AX. The published score is the **lower** of the two agent scores (conservative composite).

Results from a single agent are accepted for community-submitted benchmarks but carry a `[single-agent]` tag.

### 3. Temporal consistency

A single-point-in-time assessment can be gamed by temporarily improving a service for the test window. To counter this:

- Official benchmark scores require two assessments at least 14 days apart. The published score is the average of the two.
- Community-submitted single assessments are accepted but carry a `[single-pass]` tag.

### 4. Undocumented behaviour detection

During assessment, assessors must attempt at least 2 requests that are valid but unusual (e.g. maximum field lengths, uncommon but valid characters in strings, empty optional arrays, boundary dates). If the service handles standard requests well but fails on edge cases, this is recorded in the Recoverability dimension and noted in the summary as a potential indicator of benchmark-specific optimisation.

### 5. Score freeze

Once a service is scored, its AXS rating is frozen for 30 days. A service may request a re-assessment only after the freeze period expires and only if accompanied by a public changelog entry documenting what changed. This prevents score churn, forces real engineering investment before re-assessment, and ensures the benchmark reflects sustained quality rather than temporary fixes deployed for the test window.

Community-submitted scores follow the same 30-day freeze. If a community member submits a score for a service that was scored within the last 30 days, the new submission is held and published after the freeze expires.

### Reporting suspected gaming

If an assessor believes a service has been specifically optimised for the published task set (e.g. hard-coded responses to known test inputs, special handling of known test account patterns), this should be reported as a GitHub issue with evidence. The service's score will be flagged pending review.

---

## API Version Policy

AXS scores the API version that an agent encounters through the service's default integration path.

### 1. Default version rule

If a service's documentation landing page, OpenAPI spec, or MCP manifest points to a specific version (e.g. v3), score that version. If no version is specified in the default path, score the latest stable (non-beta, non-deprecated) version.

### 2. Machine-readable version requirement

The API version must be machine-readable: exposed via OpenAPI `info.version`, MCP manifest version field, GraphQL schema header, or `X-API-Version` response header. If the service does not expose version information in a machine-readable format, this is recorded as a finding in the Discoverability dimension (impacts the "versioning and changelog are machine-readable" criterion).

### 3. Version recording

Every scorecard must record the exact API version tested in the Assessment Details table (e.g. `REST API v3.1`, `MCP server 2024-11-05`, `GraphQL schema 2026-03`). Results without a recorded version are invalid.

### 4. Supplementary version scores

Assessors may optionally score additional versions (e.g. a legacy v2 still in wide use). These are recorded as separate scorecards with a `[supplementary]` tag and do not replace the primary score.

### 5. Version deprecation

If a scored version is deprecated by the service after assessment, the score is marked `[deprecated-version]` in the benchmark table. A re-assessment against the replacement version should follow within 90 days.

### 6. Beta / preview APIs

Beta or preview versions are not scored unless they are the only available version. If scored, they carry a `[beta]` tag.

---

## Protocol Neutrality

AXS is protocol-agnostic. The framework does not award bonuses or penalties based on which protocol a service uses (REST, GraphQL, MCP, gRPC, webhooks, or other).

**Rationale:** MCP-native services will naturally score higher on dimensions where MCP provides structural advantages:

- **Discoverability:** MCP manifests are machine-readable by design (up to +5)
- **Schema Quality:** MCP tool definitions enforce typed inputs/outputs (up to +5)
- **Auth & Access:** MCP often handles auth at the protocol layer (up to +4)

These advantages are real and are reflected in the score because the service genuinely performs better on those criteria, not because of a protocol label. A REST API with a complete OpenAPI spec, typed schemas, and clean OAuth scores identically to an MCP server with equivalent properties.

Adding an MCP bonus would:

- Penalise services with excellent AX that use other protocols
- Create an incentive to ship a thin MCP wrapper over a poor API (gaming)
- Tie the framework to a single protocol's adoption trajectory

If MCP becomes the dominant agent-service protocol, MCP-native services will dominate the leaderboard organically. The framework measures outcomes, not protocol choices.

---

## Multi-Pathway Services

Many services expose multiple interaction pathways: REST API, MCP server, GraphQL endpoint, webhook subscriptions, SDK wrappers, or UI automation surfaces.

### Primary score rule

Score the pathway that produces the highest composite AXS. This is the service's published score.

**Rationale:** An agent will use the best available pathway. A service that offers a poor REST API but an excellent MCP server still delivers good AX in practice, because competent agents will route through MCP. The primary score reflects the best real-world agent experience the service can provide.

### Recording requirements

1. The scorecard must state which pathway was scored as primary (e.g. `Primary pathway: MCP server v2024-11-05`)
2. The assessor must list all pathways available (e.g. `Available: REST v3, MCP, Python SDK, webhooks`)
3. If time permits, assessors may score additional pathways as supplementary scorecards with a `[pathway:REST]` or `[pathway:MCP]` tag

### Pathway coverage bonus

None. There is no bonus for offering multiple pathways. The framework measures quality of the best path, not quantity of paths.

### Pathway-specific notes in summary

The per-service summary must note if the primary pathway diverges significantly from other available pathways (e.g. "MCP server scores 82; REST API scores 51. Agents without MCP support will have a materially worse experience"). This context helps consumers of the benchmark make informed decisions based on their agent's capabilities.

---

## Reproducibility Requirements

AXS assessments must be reproducible. Two assessors scoring the same service within a reasonable time window should arrive at similar results (within ±8 points composite). To support this:

### Mandatory recording in every scorecard

| Field | Required |
|-------|----------|
| Service name and version | Yes |
| API version tested | Yes |
| Primary pathway tested | Yes |
| All available pathways | Yes |
| Agent model(s) and version(s) | Yes |
| Date(s) of assessment | Yes |
| Methodology version | Yes |
| Service tier/plan tested | Yes |
| Assessor name or handle | Yes |

### Evidence standards

- Scores must reference specific observations, not impressions
- Error messages must be quoted verbatim (redact API keys and credentials)
- Timing data must include the measurement method (wall clock, programmatic)
- Task completion data must include attempt count and intervention details

### Variance tolerance

If two independent assessments of the same service (same version, same methodology version, within 30 days) diverge by more than 8 points composite, a third assessment is triggered and the median of three is published.

---

## Category-Specific Weights

Different service categories have different AX priorities. See [category-weights.md](category-weights.md) for the full weights table, per-category rationale, and contribution guidelines for new categories.

Rules for category weights:

- All category weight rows must sum to 100
- No dimension may be weighted below 7 or above 18
- Weight adjustments must include a written rationale tied to real agent interaction patterns for that category
- The Default row (15-15-15-15-15-15-10) is used when no category-specific weights exist

---

## Versioning of This Methodology

This methodology is versioned. Changes to scoring criteria, weights, or policies are tracked in the project changelog. Benchmark results reference the methodology version used.

| Version | Date | Summary |
|---------|------|---------|
| v0.1 | March 2026 | Initial publication. Seven dimensions, composite scoring, tier classification, anti-gaming policy (including 30-day score freeze), version handling (including machine-readable version requirement), protocol neutrality, multi-pathway rules, reproducibility requirements. |

### Change policy

- **Patch changes** (v0.1.x): Clarifications, typo fixes, additional examples. Do not change scores.
- **Minor changes** (v0.x): New criteria, weight adjustments, policy additions. Existing scores remain valid but may be re-assessed.
- **Major changes** (vX.0): Structural changes to dimensions or composite formula. All existing scores are marked with prior version and re-assessment is recommended.

All changes are proposed via GitHub PR and require at least one review from a different contributor before merge.
