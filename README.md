# zivana-sdk

Official SDK libraries for building on the [Zivana Protocol](https://github.com/zivana-labs).

Available for TypeScript, JavaScript, and Python. All three libraries 
expose the same five ZVN namespaces — Identity, Trust, Covenant, 
Distribution, and Governance — so developers can build on Zivana 
using whichever language fits their stack.

---

## Available Packages

| Package | Language | Install | Registry |
|---|---|---|---|
| `@zivana-labs/sdk-ts` | TypeScript | `npm install @zivana-labs/sdk-ts` | npm |
| `@zivana-labs/sdk-js` | JavaScript | `npm install @zivana-labs/sdk-js` | npm |
| `zivana-sdk` | Python | `pip install zivana-sdk` | PyPI |

---

## Five Namespaces — Three Languages

Every package exposes the same five namespaces:

### TypeScript / JavaScript

```typescript
import { ZVN } from '@zivana-labs/sdk-ts';

const client = new ZVN.Client({
  network: 'preprod',
  cardanoProvider: your_wallet_provider,
  midnightNode: 'https://your-midnight-node',
});

// Identity
const credential = await client.Identity.issue({
  did: participant_did,
  role: 'SME',
  jurisdiction: 'NG',
});

// Trust
const proof = await client.Trust.proveThreshold({
  did: sme_did,
  requiredScore: 600,
});

// Covenant
const covenant = await client.Covenant.create({
  schema: 'catalyst-profit-share-v1',
  sme: sme_did,
  terms: covenant_terms,
});

// Distribution
const distribution = await client.Distribution.attest({
  covenantId: covenant.id,
  revenue: 1000000, // in kobo
});

// Governance
const proposal = await client.Governance.propose({
  type: 'parameter-change',
  parameter: 'community-access-fund-threshold',
  value: 500000,
});
```

### Python

```python
from zivana import ZVNClient

client = ZVNClient(
    network="preprod",
    cardano_provider=your_provider,
    midnight_node="https://your-midnight-node"
)

# Identity
credential = await client.identity.issue(
    did=participant_did,
    role="SME",
    jurisdiction="NG"
)

# Trust
proof = await client.trust.prove_threshold(
    did=sme_did,
    required_score=600
)

# Covenant
covenant = await client.covenant.create(
    schema="catalyst-profit-share-v1",
    sme=sme_did,
    terms=covenant_terms
)

# Distribution
distribution = await client.distribution.attest(
    covenant_id=covenant.id,
    revenue=1000000  # in kobo
)

# Governance
proposal = await client.governance.propose(
    type="parameter-change",
    parameter="community-access-fund-threshold",
    value=500000
)
```

---

## Repository Structure

This is a monorepo. Each SDK package lives in its own directory 
under `packages/` and is published independently.

zivana-sdk/

├── packages/

│   ├── sdk-typescript/   ← @zivana-labs/sdk-ts

│   ├── sdk-python/       ← zivana-sdk on PyPI

│   └── sdk-javascript/   ← @zivana-labs/sdk-js

├── docs/                 ← language-specific guides

├── examples/             ← working examples in all three languages

└── README.md

---

## Design Principles

**Consistent across languages** — the same five namespaces, the same 
method names (translated to each language's convention), the same 
error types. A developer who knows the TypeScript SDK can read 
Python SDK code without confusion.

**Idiomatic within each language** — TypeScript uses classes and 
interfaces. Python uses async/await with snake_case naming and type 
hints. JavaScript provides CommonJS and ESM exports. Each SDK feels 
native to its community.

**Honest about what is implemented** — methods that are not yet 
backed by live protocol contracts are marked clearly as 
`@coming-in-phase-2` in TypeScript and `# Phase 2` in Python. 
No silent stubs that appear to work but do nothing.

**Test coverage minimum 85%** across all three packages. The SDK 
is the interface every external developer uses — it has the highest 
quality bar in the organisation.

---

## Development Status

> Phase 2 — SDK implementation begins after core primitives are 
> verified in Phase 1

The API surface documented above represents the target interface. 
Method signatures are being finalised during Phase 0 and Phase 1 
alongside the primitive implementations.

Package structure and monorepo configuration are being established 
now so that development begins on a solid foundation in Phase 2.

---

## Getting Started for Contributors

### TypeScript / JavaScript

```bash
git clone https://github.com/zivana-labs/zivana-sdk.git
cd zivana-sdk

# Install all workspace dependencies
npm install

# Build all packages
npm run build --workspaces

# Run all tests
npm test --workspaces
```

### Python

```bash
cd packages/sdk-python

# Create virtual environment
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Install dependencies
pip install -e ".[dev]"

# Run tests
pytest
```

---

## Contributing

Read the [contributing guidelines](https://github.com/zivana-labs/.github/blob/main/CONTRIBUTING.md) 
before opening a pull request. All PRs target the `develop` branch.

The SDK has the highest documentation and test standard of any 
repository in the organisation:

- Every public method must have documentation with a usage example
- Minimum 85% test coverage required on all PRs
- Breaking changes require a major version bump and migration guide
- New methods must be implemented in all three languages simultaneously 
  — no language falls behind the others

## Licence

MIT — see [LICENSE](./LICENSE)

---

*Part of [Zivana Labs](https://github.com/zivana-labs) — 
built for Africans, open to the world.*
