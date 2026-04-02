---
title: Contributing
nav_order: 5
---

# Contributing to AXS

Contributions improve this framework. The most valuable contribution is scoring real services and submitting results. Theory without data is worthless here.

## Licence

All contributions are licensed under Apache 2.0. By submitting a PR, you agree to this. No CLAs, no assignment of copyright. You retain authorship; the project retains the right to distribute under Apache 2.0.

---

## Ways to Contribute

### 1. Run a Benchmark (Highest Value)

Score services against the framework and submit results.

**Steps:**

1. Pick a service category from [category-weights.md](framework/category-weights.md) or propose a new one
2. Define 3 standardised tasks if not already defined for that category (see `benchmarks/[category]/tasks.md`)
3. Follow the [test protocol](framework/test-protocol.md)
4. Complete the [scorecard template](templates/axs-scorecard.md)
5. Write a 2–3 paragraph summary per service
6. Submit via PR to `benchmarks/[category]/results/`

**Quality thresholds for benchmark submissions:**

| Requirement | Community submission | Official leaderboard |
|-------------|---------------------|---------------------|
| Agent models tested | 1 minimum | 2 minimum (different providers) |
| Assessment passes | 1 minimum | 2 minimum (14+ days apart) |
| Probe task included | Encouraged | Required (1 per assessment) |
| Edge case requests | Encouraged | Required (2+ per assessment) |
| Tag | `[single-agent]` and/or `[single-pass]` as applicable | No tags required |

Community submissions with tags are welcome and valuable. They surface signal even without the rigour required for the official leaderboard. Do not let the official requirements discourage you from submitting a single-pass, single-agent result. It is still useful data.

**Mandatory fields in every scorecard** (submissions missing any of these will be returned):

- Service name and exact API version
- Primary pathway tested and all available pathways listed
- Agent model(s) and version(s) used
- Date(s) of assessment
- Methodology version (currently v0.1)
- Service tier/plan tested
- Evidence for every score (not impressions; specific observations, quoted error messages, timing data)

### 2. Improve the Framework

- Propose new criteria or refine existing rubrics (with evidence from real agent testing)
- Challenge scoring thresholds (if you can show the current threshold misclassifies a service, open an issue)
- Propose automation for manual review steps
- Propose new service categories with weight rationale

Framework changes follow the change policy in [scoring-methodology.md](framework/scoring-methodology.md):

- Patch (clarifications): 1 reviewer
- Minor (new criteria, weight changes): 1 reviewer + 7-day comment period
- Major (structural): 2 reviewers + 14-day comment period

### 3. Build Tooling

The framework is intentionally manual-first to keep the barrier to entry low. But automation is welcome for:

- Scripts to run standardised API checks (schema validation, error format consistency, response variance)
- Timing harnesses for latency measurement
- Scorecard generators that output the template pre-filled with automated checks
- Dashboards or static site generators for visualising benchmark results
- CI/CD integration for continuous AXS monitoring

Tooling PRs should include documentation and should not change the methodology itself.

### 4. Add Category Weights

To propose weights for a new service category:

1. Define the category and list 5+ example services
2. Propose a weight row (must sum to 100, all dimensions within 7–18 bounds)
3. Write a rationale for each weight adjustment tied to real agent interaction patterns
4. Submit via PR to [category-weights.md](framework/category-weights.md)

---

## Submission Standards

- One PR per service or per category batch (do not mix framework changes with benchmark results)
- All error messages quoted verbatim (redact API keys, tokens, and credentials)
- Timing data must include measurement method
- Assessor name or handle must be included
- Conflict of interest disclosure: if you work for the service being scored, or a direct competitor, state this in your PR description. Your submission is still welcome. Transparency is what matters.

---

## Reporting Issues

**Suspected gaming:** If you believe a service has been optimised specifically for the published task set, report it as a GitHub issue with evidence.

**Score disputes:** If you believe a published score is inaccurate, submit a counter-assessment as a PR with your own scorecard. If two independent assessments diverge by more than 8 points, a third assessment will be sought and the median published.

**Methodology feedback:** Open a GitHub Discussion for broad feedback or questions about the framework. Open an Issue for specific, actionable proposed changes.

---

## Code of Conduct

Be direct. Be constructive. The goal is accurate measurement, not promotion or demotion of any service. Advocacy for or against specific services, protocols, or vendors is not appropriate in this project. Let the scores speak.
