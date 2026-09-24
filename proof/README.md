# Sanitized live proof

These JSON files are sanitized extracts of observed GenLayer CLI receipts and
state reads. They intentionally omit validator private keys, credentials, and
machine-local configuration.

## Current deployment

- Contract: `0x521C5093b2Fb6fE8c282B9BD7E13d57f5f268757`
- Deployment transaction: `0xcdbfed2a17b5744b2600ffcd76bc44b18bff449038c08cd5d7e59f5ccfe53796`
- [Open in Studio](https://studio.genlayer.com/?import-contract=0x521C5093b2Fb6fE8c282B9BD7E13d57f5f268757)
- [View in Explorer](https://explorer-studio.genlayer.com/address/0x521C5093b2Fb6fE8c282B9BD7E13d57f5f268757)

`current-lifecycle.json` is the current contract's partial lifecycle record. It
contains only observed current-contract transactions and state. Current
conflict resolution, priority update, and blocked-rule activation are explicitly
marked `not_yet_executed` or `receipt_unavailable` where applicable.

## Previous deployment lifecycle evidence

The lifecycle JSON files marked `historical: true` belong to the previous
 deployment at
`0x521C5093b2Fb6fE8c282B9BD7E13d57f5f268757`. Their transaction hashes remain
associated with that contract and are not reused for the current deployment.

The older deployment at `0x67a027446838296FcB3022B376c8ff3873a4566C` is
archived under `archive-bd6682d/`.

`deployment-transition.json` records the redeployment boundary between the
previous and current contracts.
