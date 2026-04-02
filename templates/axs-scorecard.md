---
title: Scorecard Template
nav_order: 3
---

# AXS Scorecard: [Service Name]

## Assessment Details

| Field | Value |
|-------|-------|
| Service | [Service Name] |
| Category | [e.g. Project Management] |
| Assessor | [Name] |
| Date | [YYYY-MM-DD] |
| Agent Used | [e.g. Claude Code, Claude Sonnet 4.6] |
| Tier/Plan Tested | [e.g. Free, Developer, Pro] |
| Methodology Version | v0.1 |

---

## Dimension Scores

### 1. Discoverability (0–15)

| Criterion | Max | Score | Evidence |
|-----------|-----|-------|----------|
| Machine-readable API spec | 5 | | |
| Action descriptions clear | 4 | | |
| Actions enumerable | 3 | | |
| Versioning machine-readable | 3 | | |
| **Subtotal** | **15** | **0** | |

### 2. Schema Quality (0–15)

| Criterion | Max | Score | Evidence |
|-----------|-----|-------|----------|
| Typed input/output schemas | 5 | | |
| Internal consistency | 3 | | |
| Response predictability | 4 | | |
| Error format consistency | 3 | | |
| **Subtotal** | **15** | **0** | |

### 3. Reliability (0–15)

| Criterion | Max | Score | Evidence |
|-----------|-----|-------|----------|
| Task completion rate | 5 | | |
| Uptime/availability | 5 | | |
| Rate limit communication | 3 | | |
| Idempotency support | 2 | | |
| **Subtotal** | **15** | **0** | |

### 4. Recoverability (0–15)

| Criterion | Max | Score | Evidence |
|-----------|-----|-------|----------|
| Actionable error messages | 5 | | |
| Partial failure handling | 3 | | |
| Retry guidance | 3 | | |
| Graceful degradation | 4 | | |
| **Subtotal** | **15** | **0** | |

### 5. Latency & Efficiency (0–15)

| Criterion | Max | Score | Evidence |
|-----------|-----|-------|----------|
| Time-to-first-byte | 4 | | |
| Task completion time | 4 | | |
| Pagination/bulk support | 4 | | |
| Streaming support | 3 | | |
| **Subtotal** | **15** | **0** | |

### 6. Auth & Access (0–15)

| Criterion | Max | Score | Evidence |
|-----------|-----|-------|----------|
| M2M auth supported | 5 | | |
| No human-in-the-loop | 4 | | |
| Scoped permissions | 3 | | |
| Token refresh documented | 3 | | |
| **Subtotal** | **15** | **0** | |

### 7. Determinism (0–10)

| Criterion | Max | Score | Evidence |
|-----------|-----|-------|----------|
| Response consistency | 4 | | |
| Side effects documented | 3 | | |
| No undocumented changes | 3 | | |
| **Subtotal** | **10** | **0** | |

---

## Composite Score

| Dimension | Score |
|-----------|-------|
| Discoverability | /15 |
| Schema Quality | /15 |
| Reliability | /15 |
| Recoverability | /15 |
| Latency & Efficiency | /15 |
| Auth & Access | /15 |
| Determinism | /10 |
| **AXS Total** | **/100** |
| **Tier** | |

Minimum dimension floor check (all dimensions ≥40% of max): [ ] Pass / [ ] Fail

---

## Agent Task Results

### Task 1: [Description]

| Attempt | Completed? | Iterations | Human Intervention? | Time | Notes |
|---------|-----------|------------|---------------------|------|-------|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |

### Task 2: [Description]

| Attempt | Completed? | Iterations | Human Intervention? | Time | Notes |
|---------|-----------|------------|---------------------|------|-------|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |

### Task 3: [Description]

| Attempt | Completed? | Iterations | Human Intervention? | Time | Notes |
|---------|-----------|------------|---------------------|------|-------|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |

---

## Error Samples

Record verbatim error messages encountered during testing (redact credentials):

```
[Paste error responses here]
```

---

## Summary

[2–3 paragraphs: what worked well, what broke, specific friction points, recommendations]
