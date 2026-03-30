# Category-Specific Dimension Weights

## Default Weights

By default, all dimensions are weighted equally (their raw point values). Category-specific weights adjust dimension maximums by ±3 points to reflect what matters most for that service type. Total always remains 100.

## Proposed Categories and Weight Adjustments

| Category | Discoverability | Schema | Reliability | Recoverability | Latency | Auth | Determinism |
|----------|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| **Default** | 15 | 15 | 15 | 15 | 15 | 15 | 10 |
| **CRM / Sales** | 18 | 18 | 15 | 12 | 12 | 15 | 10 |
| **Project Management** | 15 | 15 | 15 | 18 | 12 | 15 | 10 |
| **Communication** | 12 | 12 | 18 | 15 | 15 | 18 | 10 |
| **Payments / Fintech** | 12 | 15 | 18 | 15 | 12 | 15 | 13 |
| **Data / Analytics** | 15 | 18 | 15 | 12 | 18 | 12 | 10 |
| **Cloud Infrastructure** | 15 | 15 | 15 | 18 | 12 | 18 | 7 |
| **Content / CMS** | 18 | 18 | 12 | 12 | 15 | 12 | 13 |
| **E-commerce** | 15 | 15 | 18 | 15 | 15 | 12 | 10 |

## Rationale

Weight adjustments reflect the most critical AX dimensions for each category:

- **CRM / Sales**: Agents need to discover complex data models (many entity types) and work with well-typed schemas across numerous relationship types.
- **Project Management**: Agents frequently encounter edge cases (circular dependencies, permission-based visibility, workflow transitions). Recoverability matters most.
- **Communication**: Messages are time-sensitive (latency) and often involve delegated access (auth). Reliability is critical; a failed message send is worse than a failed data query.
- **Payments / Fintech**: Financial operations must be reliable and deterministic. A payment processed twice is a serious failure.
- **Data / Analytics**: Large data volumes require efficient pagination and fast queries. Schema quality drives query accuracy.
- **Cloud Infrastructure**: Agents managing infrastructure need robust error recovery and strong auth patterns. Misconfigured access is a security risk.
- **Content / CMS**: Content models vary widely. Discoverability and schema quality determine whether an agent can work with the content structure.

## Contributing New Categories

To propose weights for a new category:

1. Define the category and list 5+ example services
2. Explain the rationale for each weight adjustment
3. Weights must sum to 100
4. No dimension may be weighted below 7 or above 18
5. Submit via PR to this file
