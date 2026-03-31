# AXS: Agent Experience Score

An open-source framework for measuring how well digital services support AI agent interaction.

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

## The Problem

Every agent benchmark measures how good the **agent** is. None of them measure how good the **service** is for agents. AXS inverts the question.

As the [AX community](https://agentexperience.ax) has established, Agent Experience is becoming as foundational as UX and DX. But without a standardised measurement framework, "agent-ready" is a marketing claim, not a verifiable property.

AXS provides objective, reproducible, comparable scoring for service-side Agent Experience.

**Note:** AXS is an independent open-source framework. It is not affiliated with AirShelf's AX Score, Sharpen Technologies' AXS, or any other prior use of the acronym. This framework specifically provides an open, reproducible methodology for scoring service-side AI Agent Experience across all service categories.

## The Framework

AXS scores services across **seven dimensions** on a **0–100 scale**:

| Dimension | Points | What It Measures |
|-----------|--------|-----------------|
| Discoverability | 0–15 | Can an agent find and understand available actions? |
| Schema Quality | 0–15 | Are interfaces well-structured, typed, and consistent? |
| Reliability | 0–15 | Does the service work consistently? |
| Recoverability | 0–15 | How gracefully does it handle errors and edge cases? |
| Latency & Efficiency | 0–15 | How fast can an agent complete tasks? |
| Auth & Access | 0–15 | How painful is agent authentication? |
| Determinism | 0–10 | Same input, same output? |

### Tier Classification

| Tier | Score | Label |
|------|-------|-------|
| A+ | 90–100 | Agent-Native |
| A | 80–89 | Agent-Ready |
| B | 65–79 | Agent-Capable |
| C | 50–64 | Agent-Tolerant |
| D | 35–49 | Agent-Hostile |
| F | 0–34 | Agent-Incompatible |

## Key Design Decisions

- **Objective, not subjective.** Every criterion is measurable through automated testing or structured manual review. No surveys, no opinions.
- **Service-side, not agent-side.** We score the service, not the agent.
- **Protocol-neutral.** No bonuses for MCP, REST, GraphQL, or any specific protocol. The framework measures outcomes. Services using MCP will score higher on dimensions where MCP provides structural advantages, because they genuinely perform better, not because of a label.
- **Best-pathway scoring.** Services with multiple pathways (API + MCP + SDK) are scored on their best-performing pathway. Agents route to the best option; the score reflects reality.
- **Anti-gaming.** Task rotation, multi-agent verification, temporal consistency checks, and edge-case probing prevent benchmark-specific optimisation. See the [scoring methodology](framework/scoring-methodology.md#anti-gaming-policy) for details.
- **Category-aware.** Different service categories have different AX requirements. Category-specific weights adjust priorities.
- **Beyond developer tools.** Built to work across all service categories, not just APIs and dev tools.
- **Open.** Apache 2.0. Methodology, data, and results are all public.

## Documentation

- [`framework/scoring-methodology.md`](framework/scoring-methodology.md): Full scoring rubrics, composite formula, tier classification, anti-gaming policy, version handling, protocol neutrality, multi-pathway rules, and reproducibility requirements
- [`framework/test-protocol.md`](framework/test-protocol.md): Step-by-step guide to running an AXS assessment
- [`framework/category-weights.md`](framework/category-weights.md): Category-specific dimension weighting with rationale
- [`templates/axs-scorecard.md`](templates/axs-scorecard.md): Blank scorecard for recording results
- [`benchmarks/`](benchmarks/): Published benchmark results

## Quick Start: Score a Service

1. Read the [scoring methodology](framework/scoring-methodology.md)
2. Follow the [test protocol](framework/test-protocol.md)
3. Use the [scoring template](templates/axs-scorecard.md) to record results
4. Submit results via PR to `benchmarks/`

## Benchmarks

| Category | Services Scored | Status |
|----------|----------------|--------|
| Project Management | TBD | In progress, first results Q2 2026 |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for full guidelines. The short version:

- **Run a benchmark**: the highest-value contribution. Score services and submit results.
- **Improve the framework**: propose new criteria, challenge rubrics, build automation.
- **Add categories**: define dimension weights for new service types.

Community submissions (single-agent, single-pass) are welcome and carry appropriate tags. Official leaderboard submissions require multi-agent testing and temporal consistency. Both are valuable.

Conflict of interest? Disclose it in your PR. Your submission is still welcome.

## Background

AXS builds on the [Agent Experience (AX)](https://agentexperience.ax) movement initiated by [Mathias Biilmann](https://biilmann.blog/articles/introducing-ax/) in January 2025. The AX community has established principles and practices for building agent-friendly services. AXS contributes the measurement layer: a standardised way to score whether services actually meet those principles.

## Licence

Apache 2.0. See [LICENCE](https://opensource.org/license/Apache-2.0).

## Author

[Satender Kundu](https://satenderkundu.com). Worked on NPS measurement and methodology at Vodafone, with additional background in product GTM strategy and big data engineering across Scholastic EMEA and ITC Hotels. From India, based in the Netherlands.
