---
title: Changelog
nav_order: 6
---

# Changelog

All notable changes to the AXS framework are documented here.

This project follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) conventions and [Semantic Versioning](https://semver.org/spec/v2.0.0.html). Version increments follow the rules defined in `framework/scoring-methodology.md`:

- **Patch** — clarifications, wording fixes, no scoring impact
- **Minor** — new optional dimensions, adjusted category weights, new service categories
- **Major** — scoring formula changes, dimension additions/removals, tier boundary changes

---

## [Unreleased]

---

## [0.1.0] – 2026-03-28

### Added
- Initial publication of the AXS (Agent Experience Score) framework
- Seven scoring dimensions: Discoverability, Schema Quality, Reliability, Recoverability, Latency & Efficiency, Auth & Access, Determinism & Predictability
- Composite scoring formula (0–100 scale) with tier classification: A+, A, B, C, D, F
- Category-specific dimension weights for eight service categories: Default, CRM, Project Management, Communication, Payments, Data & Analytics, Infrastructure, Content Management
- Structured test protocol with step-by-step assessment guide (45–90 min per service)
- Blank AXS scorecard template for recording assessment results
- CONTRIBUTING.md with benchmark submission requirements and community vs. official leaderboard distinction
- Apache 2.0 licence

[Unreleased]: https://github.com/sat3n/axs-framework/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/sat3n/axs-framework/releases/tag/v0.1.0
