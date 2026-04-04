# AXS Benchmark Assessment: Plane

You are an AI agent tasked with conducting a standardised AXS (Agent Experience Score)
assessment of [SERVICE], an open-source [CATEGORY] service.

AXS measures how well a digital service supports AI agent interaction. It scores the
*service*, not the agent. You will act as both the assessor and the test agent,
following the full protocol below.

---

## Step 0: Read the Framework

Before doing anything else, read the following files from the AXS repository:

- `framework/scoring-methodology.md` — Full rubrics for all 7 dimensions, composite
  formula, tier classification, minimum dimension floor, anti-gaming policy, protocol
  neutrality, multi-pathway rules, and reproducibility requirements.
- `framework/test-protocol.md` — Step-by-step assessment procedure.
- `framework/category-weights.md` — Category-specific dimension weights.
- `templates/axs-scorecard.md` — The scorecard template you will fill out and save.

Do not proceed to Step 1 until you have read all four files.

---

## Step 1: Setup (~10 min)

1. **Identify the service category.** Choose from: CRM/Sales, Project Management,
   Communication, Payments/Fintech, Data/Analytics, Cloud Infrastructure,
   Content/CMS, E-commerce. If none fit, use Default.

2. **Apply category weights.** Look up the matching row in `category-weights.md`.
   These weights replace the default (15-15-15-15-15-15-10) and adjust what each
   dimension is worth out of 100.

3. **Identify all available pathways.** List every interaction pathway the service
   exposes: REST API, MCP server, GraphQL, SDK, webhooks, etc. You will score the
   best-performing pathway as primary. Record all pathways in the scorecard.

4. **Record the API version.** Identify the API version through the default integration
   path (documentation landing page, OpenAPI `info.version`, MCP manifest version,
   or `X-API-Version` header). Record it exactly (e.g. `REST API v3.1`,
   `MCP server 2024-11-05`). A scorecard without a recorded API version is invalid.

5. **Define 3 standardised tasks** representative of common agent use cases for this
   service type. Tasks must be:
   - Achievable on a free, developer, or self-hosted tier
   - Specific enough to be reproducible by another assessor
   - Representative of what an agent would realistically do with this service

6. **Define 1 probe task.** Choose a reasonable task within the service's domain that
   is *not* an obvious first-thing-to-try. This tests whether AX quality extends
   beyond the happy path. Mark all probe results with `[probe]` in the scorecard.

7. Record your agent model and version (e.g. `Claude Sonnet 4.6`), assessment date,
   and the service tier/plan being tested.

---

## Step 2: Discoverability — max 15 pts (~15 min)

Review public-facing documentation without using the API:

- Search for the service + "API documentation". Can you reach it within 2 clicks?
- Look for a machine-readable spec: OpenAPI, MCP manifest, or GraphQL schema.
  - Score 5 if complete and current, 3 if exists but incomplete, 1 if outdated, 0 if absent.
- Read 5+ endpoint/tool descriptions. Are they unambiguous? Could you infer correct
  usage from the description alone, without guessing?
  - Score by accuracy: how well does an agent's description of each endpoint match reality?
- Can you enumerate all available actions from the spec/docs alone, without prior knowledge?
  - Score 3 if fully enumerable, 2 if partially, 0 if not possible.
- Is there a machine-readable changelog or versioning (JSON, RSS, or clearly structured
  markdown)?
  - Score 3 if machine-readable and current, 1 if exists but unstructured, 0 if absent.

**Scoring rubric reference:** `scoring-methodology.md`, Dimension 1.

---

## Step 3: Schema Quality — max 15 pts (~15 min)

Examine the API specification or documentation:

- Count total endpoints/tools. What percentage have fully typed input/output schemas?
  - Score 5 if 100%, 3 if 75%+, 1 if 50%+, 0 if below 50%.
- Check internal consistency: do related endpoints use the same field names and types
  for the same concepts (e.g. `user_id` is always a string, or always an integer)?
  - Score 3 if no issues found, 2 if minor issues, 0 if contradictions present.
- Run the same GET request twice. Compare response structures field by field.
  - Score 4 if identical, 2 if minor variance, 0 if unpredictable.
- Trigger 5+ different error types. Is the error response format consistent and parseable?
  - Score 3 if consistent and structured, 1 if partially consistent, 0 if inconsistent.

**Scoring rubric reference:** `scoring-methodology.md`, Dimension 2.

---

## Step 4: Auth & Access — max 15 pts (~15 min)

Obtain API credentials and assess authentication:

- Is machine-to-machine auth supported (API key, OAuth client credentials, service account)?
  - Score 5 if M2M auth is documented and working, 3 if it exists but is poorly documented,
    0 if only browser-based/human auth is available.
- Document every step from "visit the site" to "have a working API key or token".
  Record all friction points: CAPTCHA, phone verification, email confirmation, dashboard
  navigation complexity.
  - Can an agent maintain access without human intervention (no recurring MFA, no CAPTCHA)?
  - Score 4 if fully automated, 2 if a one-time human step is required, 0 if recurring
    human steps are required.
- Are scoped permissions available (limit access to specific operations)?
  - Score 3 if granular scopes exist, 1 if only broad scopes, 0 if all-or-nothing.
- Is token refresh automated and documented? Can an agent refresh without human help?
  - Score 3 if automated refresh is documented, 1 if only manual refresh, 0 if undocumented.

**Scoring rubric reference:** `scoring-methodology.md`, Dimension 6.

---

## Step 5: Execute Agent Tasks (~20–40 min)

For each of the 3 standardised tasks and the 1 probe task:

1. Start with a clean context — no prior memory of this service's API behaviour for
   this attempt.
2. Attempt the task using only the service's API and its documentation. Do not contact
   support.
3. Record for each attempt:
   - Did you complete the task? (Yes / No)
   - How many iterations or retries were needed?
   - What errors did you encounter? Quote error messages verbatim (redact credentials).
   - Were error messages actionable enough to self-correct without guessing?
   - Total time, or total number of API calls, to completion.
4. Repeat each task 3 times to test consistency.

Also during this phase:

- Deliberately hit the rate limit at least once. Record the exact response (status code,
  headers, body).
- Test idempotency: send a duplicate write request. Record the behaviour.
- Attempt at least 2 edge-case requests: maximum field lengths, unusual-but-valid
  characters, empty optional arrays, boundary dates. Record outcomes in Recoverability.

---

## Step 6: Score Reliability — max 15 pts (From Task Data)

Using data collected in Step 5:

- Task completion rate across all standardised task attempts (excluding the probe):
  - Score 5 if 95%+, 3 if 80%+, 1 if 60%+, 0 if below 60%.
- Uptime / availability during the test window:
  - Score 5 if no downtime observed, 3 if brief interruption, 0 if significant outage.
- Rate limit response quality:
  - Score 3 if the response is clear and actionable (includes Retry-After, limit info),
    1 if mentioned but vague, 0 if silent failure.
- Idempotency support for write operations:
  - Score 2 if idempotent with key support, 1 if partial, 0 if no support.

**Scoring rubric reference:** `scoring-methodology.md`, Dimension 3.

---

## Step 7: Score Recoverability — max 15 pts (~10 min)

Deliberately trigger errors:

- Send a request with a missing required field.
- Send a request with the wrong data type.
- Send a request to a non-existent resource.
- Send a request with an invalid or expired token.
- If the API supports batch operations, include one invalid item in a valid batch.

For each error, ask: does the response tell you exactly what is wrong and how to fix it?

- Actionable error messages: Score 5 if all errors are actionable, 3 if most, 1 if some,
  0 if cryptic.
- Partial failure handling: Score 3 if individual item failures are reported without
  breaking the batch, 1 if partial info, 0 if entire batch fails silently.
- Retry guidance (Retry-After headers, backoff hints, documented retry strategy):
  Score 3 if explicit, 1 if implicit, 0 if none.
- Graceful degradation under rapid sequential requests:
  Score 4 if a clear rate-limit response is returned, 2 if degraded but functional,
  0 if crash, timeout, or incoherent response.

Quote all error messages verbatim in the scorecard. Redact credentials.

**Scoring rubric reference:** `scoring-methodology.md`, Dimension 4.

---

## Step 8: Score Latency & Efficiency — max 15 pts (From Task Data + Additional Tests)

Using timing data from Step 5, plus:

- p50 and p95 response times for standard operations (run 10 requests and measure):
  - Score 4 if p95 is sub-500ms, 2 if sub-2s, 0 if above 5s.
- Total task completion time for the full multi-step standardised workflow:
  - Compare against other services in the same category if data exists.
  - Score 4 if fastest quartile, 2 if median, 0 if slowest quartile.
- Pagination and bulk operation support:
  - Retrieve a list that exceeds one page. Test whether bulk/batch endpoints exist.
  - Score 4 if cursor pagination and bulk endpoints are available, 2 if basic pagination
    only, 0 if no pagination support.
- Streaming or webhook support for long-running operations:
  - Score 3 if streaming or webhooks are supported, 1 if polling only, 0 if
    synchronous-only with long waits.

**Scoring rubric reference:** `scoring-methodology.md`, Dimension 5.

---

## Step 9: Score Determinism — max 10 pts (~10 min)

- Send 5 identical GET requests. Compare responses field by field, excluding timestamps
  and request IDs.
  - Score 4 if identical, 2 if minor variance, 0 if significant variance.
- Review documentation for write operations. Are side effects (webhooks triggered,
  related records updated) documented?
  - Score 3 if comprehensive, 1 if partial, 0 if undocumented.
- Compare endpoint behaviour between the start and end of your assessment. Note any
  silent changes, A/B test variation, or undocumented behaviour differences.
  - Score 3 if stable, 1 if minor changes observed, 0 if undocumented changes found.

**Scoring rubric reference:** `scoring-methodology.md`, Dimension 7.

---

## Step 10: Compile and Output Results

### Composite Score

Sum all dimension scores, applying category weights if applicable.

```
AXS = Sum of all weighted dimension scores (out of 100)
```

### Tier Classification

| Tier | Score  | Label                |
|------|--------|----------------------|
| A+   | 90–100 | Agent-Native         |
| A    | 80–89  | Agent-Ready          |
| B    | 65–79  | Agent-Capable        |
| C    | 50–64  | Agent-Tolerant       |
| D    | 35–49  | Agent-Hostile        |
| F    | 0–34   | Agent-Incompatible   |

### Minimum Dimension Floor Check

If ANY dimension scores below 40% of its weighted maximum, the tier is capped at F
regardless of composite score. Note which dimension(s) triggered the cap.

| Dimension            | Default Max | 40% Floor |
|----------------------|-------------|-----------|
| Discoverability      | 15          | 6         |
| Schema Quality       | 15          | 6         |
| Reliability          | 15          | 6         |
| Recoverability       | 15          | 6         |
| Latency & Efficiency | 15          | 6         |
| Auth & Access        | 15          | 6         |
| Determinism          | 10          | 4         |

Recalculate floors if you are using category-specific weights.

### Multi-Pathway Note

If the primary pathway (best-scoring) diverges significantly from other available
pathways, note this explicitly in the summary. Example: "MCP server scores 82; REST
API scores 51. Agents without MCP support will have a materially worse experience."

### Scorecard

Fill out `templates/axs-scorecard.md` completely. Every criterion must have a score
and a specific evidence note. Save the completed file to:

```
benchmarks/[category]/results/[service-name].md
```

Tag your submission `[single-agent]` and `[single-pass]` since this is a single-agent,
single-pass assessment.

### Executive Summary

Write 2–3 paragraphs covering:

1. What worked well for agent interaction and why.
2. What caused friction or failures, with specific examples.
3. Actionable recommendations: what the service could change to improve its AXS score.

---

## Rules to Follow Throughout

- **Score the service, not yourself.** If you struggle because of poor documentation or
  opaque errors, that is a low score for the service. If you struggle due to your own
  limitations, note it but do not penalise the service.
- **Best-pathway scoring.** Score the pathway that produces the highest composite AXS.
  List all available pathways in the scorecard.
- **Protocol neutrality.** No bonuses or penalties for REST vs GraphQL vs MCP vs gRPC.
  Score outcomes, not protocol choices.
- **Evidence over impressions.** Every score must cite a specific, observable finding.
- **Quote errors verbatim.** Redact only credentials (API keys, tokens, passwords).
- **Do not contact support.** The assessment measures unassisted agent experience.
- **Edge-case probing is mandatory.** At least 2 unusual-but-valid requests must be
  attempted to test for genuine quality beyond the happy path.
- **Methodology version.** Record `v0.1` as the methodology version in the scorecard.