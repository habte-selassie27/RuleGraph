# Rule_Graph Deployment

## Current repository status

The exact source commit below is deployed and finalized on GenLayer StudioNet.
The live lifecycle evidence was collected from the active CLI account and is
recorded here so the deployment can be independently audited.

| Field | Value |
|---|---|
| Source commit | `d63eb90585369597d7e66f143264c94254ff42ad` |
| Network | GenLayer StudioNet |
| CLI | `genlayer@0.39.2` |
| Deployer | `0x04e0353B7218b66D6803725ce7342E6e1225DB1b` |
| Deployment transaction | `0xcdbfed2a17b5744b2600ffcd76bc44b18bff449038c08cd5d7e59f5ccfe53796` |
| Contract address | `0x521C5093b2Fb6fE8c282B9BD7E13d57f5f268757` |
| Studio | [Open contract](https://studio.genlayer.com/?import-contract=0x521C5093b2Fb6fE8c282B9BD7E13d57f5f268757) |
| Explorer | [View contract](https://explorer-studio.genlayer.com/address/0x521C5093b2Fb6fE8c282B9BD7E13d57f5f268757) |
| Deployment receipt | `FINALIZED`, `MAJORITY_AGREE`; execution `SUCCESS` |

The deployment schema was verified through the CLI, and the deployment transaction is finalized on StudioNet. The CLI returned the contract address above; block number and gas values are not included because they were not independently captured.
Previous deployments `0x67a027446838296FcB3022B376c8ff3873a4566C`
(commit `bd6682d...`) and `0xf529EDf5291B7fB78f0ba3922b9162A593972020`
are historical because the contract source changed after those deployments.

## Requirements

- Node.js and npm
- GenLayer CLI
- a funded active account for the selected network

Current official CLI documentation lists direct deployment as:

```bash
genlayer deploy --contract <contractPath>
```

Rule_Graph has no constructor arguments.

## Install CLI

```bash
npm install -g genlayer
genlayer --version
```

## Select StudioNet

```bash
genlayer network set studionet
genlayer config get network
genlayer account show
```

Use an existing active account if one is already configured. Do not commit private keys or passwords to this repository.

## Deploy

```bash
genlayer deploy --contract contracts/rule_graph.py
```

Record only real output:

```text
Network: studionet
Contract address: <finalized address>
Deployment transaction: <transaction hash>
Deployment status: <accepted/finalized>
CLI version: <version>
```

## Runtime smoke sequence

After deployment, use the Studio or CLI to execute the lifecycle in `examples/treasury_rulebook.md`.

Minimum proof should include:

1. rulebook creation;
2. first active rule;
3. equal-priority conflicting rule blocked in strict mode;
4. relation edge showing `CONFLICT` + `UNRESOLVED`;
5. blocked-rule priority update;
6. same relation edge showing deterministic precedence;
7. blocked rule activation;
8. new rule_graph hash;
9. successful `is_consistent_for` call using that exact hash.

## Current deployment lifecycle

The authoritative current lifecycle record is [`proof/current-lifecycle.json`](proof/current-lifecycle.json). The redeployment boundary is recorded in [`proof/deployment-transition.json`](proof/deployment-transition.json).

The current contract has a finalized deployment and a partial observed lifecycle: rulebook `1` has two CLEAR ACTIVE rules, `COHERENT` rule_graph version `2`, and no resolved or unresolved conflicts. The current conflicting-rule submission has no verified receipt; priority update and blocked-rule activation are `not_yet_executed`. No current conflict-resolution or activation evidence is claimed.

## Historical lifecycle evidence (previous deployment)

The complete lifecycle table below is retained historical evidence for the previous deployment at `0x521C5093b2Fb6fE8c282B9BD7E13d57f5f268757`. Its transaction hashes remain associated with that contract.

| Operation | Transaction | Observed result |
|---|---|---|
| Create proof rulebook 1 | `0x378b2be1b204883998e3a7a515419bfd65d499ce7fd69dce404f9637d9577747` | Finalized; strict; rulebook 1 `Treasury Constitution` |
| Propose rule 1 (prohibition, priority 100) | `0x87d19fef4a9ae286aa242ba29bf4f0efa01c09072a83f11ecfdfd94319e521eb` | Finalized; CLEAR; ACTIVE; rule_graph v1 |
| Propose rule 2 (emergency permission, priority 100) | `0xf6ab8cfa8e984d1c28a8b54977a495d7372e9649b7a8fe28e064a624705a5d9b` | Finalized; CLEAR; BLOCKED; relation `CONFLICT`/`UNRESOLVED` |
| Propose duplicate rules 3 and 5 (retry storm) | `0x65c3dcb541d28e299ffcdace9b6a95b1000d496826f508b56aa015c92eac4878`, `0xebc2a09eb896e493b5dccac7f1b8864fd69697db404e8bcb17e85705ed1ee628` | Finalized; later repealed |
| Repeal rules 3, 4, 5 | `0x9d1a00828d08876623c3e2a7221c80430f040c27e14ee9bc84de18a9d71db75a`, `0x83963d55b3931ac770b0ec8d0c5c248cc2e98bf76bc1db9e9f35335b24358c00`, `0x87e3a27cec1ef4ea85b79ab5d8e5b1cd0fb66622c993920c3ecc453684592225` | Finalized; rules `REPEALED` |
| Set rule 2 priority to 200 | `0x5ecf8b6c86899366cd6184882a771378e4511c4735c455f59e5fa23116b0fd38` | Finalized; relation 1 `RIGHT_PREVAILS`; blocking reason empty |
| Activate rule 2 | `0x7e17fcce3aab9305ccbc2ed267ef038f894141906c23d17df63da2f6dd1bd051` | Finalized; rule 2 ACTIVE; rule_graph v2 `3cb1e710938e267223b14d142cab63f087629ad0dcab231cb8c9916688441e27` |

A premature activation attempt `0x9a1ecd210bc7d8ae62a5f5e336e302340098bf8f82732267b68464d786dbcb85` ran before the priority receipt landed and rolled back with no state change.

Final readback: relation `1`, rules `1 -> 2`, kind `CONFLICT`, conflict type `EXCEPTION`, reason `EXCEPTION_OVERRIDE`, resolution `RIGHT_PREVAILS`. Rulebook: `rule_count=5` (3 repealed), `active_count=2`, `blocked_count=0`, `relation_count=10`, `revision=11`, `resolved_conflicts=1`, `unresolved_conflicts=0`, `rule_graph_status=RESOLVED_CONFLICTS`, `consistent=true`, `rule_graph_version=2`. Exact `is_consistent_for` returned `true`; a zero hash returned `false`.

Sanitized machine-readable artifacts are in [`proof/`](proof/): deployment,
rulebook, semantic rule, conflict, repeal, priority-update, activation, and
final-state records. They contain no private keys or credentials.

## Historical live lifecycle

The following sequential transactions were finalized against the previous
deployment at `0x67a027446838296FcB3022B376c8ff3873a4566C` (commit
`bd6682d81afa7063d6b595dcdab04d220aed8bbb`). Full sanitized extracts are in
`proof/archive-bd6682d/`.

## Test before/after deployment

Local Direct Mode:

```bash
python -m pip install -r requirements-dev.txt
gltest tests/test_rule_graph.py -v -s
```

Offline repository preflight:

```bash
python scripts/preflight.py
```

Hosted network integration can be run against the finalized current contract address listed above.
