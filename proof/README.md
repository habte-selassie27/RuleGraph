# Sanitized live proof

These JSON files are sanitized extracts of observed GenLayer CLI receipts and
state reads. They intentionally omit validator private keys, credentials, and
machine-local configuration.

## Current deployment

- Contract: `0x3Cac4957D938f1Ac3E5924F5395e3bd5D0645488`
- Deployment transaction: `0x27eddefef3b87e050a9ecb0e0cda85d7a7548d32a494297d06ba62f1bcd38185`
- Deployer: `0x5B3661C576c7001e6d6279C67F3779705d334c89`
- [Open in Studio](https://studio.genlayer.com/?import-contract=0x3Cac4957D938f1Ac3E5924F5395e3bd5D0645488)
- [View in Explorer](https://explorer-studio.genlayer.com/address/0x3Cac4957D938f1Ac3E5924F5395e3bd5D0645488)

`current-lifecycle.json` is the current contract's partial lifecycle record. It
contains only observed current-contract transactions and state. Current
conflict resolution, priority update, and blocked-rule activation are explicitly
marked `not_yet_executed` or `receipt_unavailable` where applicable.

## Previous deployment lifecycle evidence

The lifecycle JSON files marked `historical: true` belong to superseded
deployments. Their transaction hashes remain associated with those contracts
and are not reused for the current deployment.

- `historical-lifecycle-521C.json` and `historical-deployment-521C.json`
  document the previous deployment at
  `0x521C5093b2Fb6fE8c282B9BD7E13d57f5f268757`
  (deploy tx `0xcdbfed2a17b5744b2600ffcd76bc44b18bff449038c08cd5d7e59f5ccfe53796`).
- The remaining `historical: true` lifecycle files belong to the earlier
  deployment at `0xfA6284e0728A80C80E213eA429aFD800BC1F5E69`.
- Older deployments at `0x6C251947a2b08F8b550b17a17082e2c3e2378136` and
  `0x67a027446838296FcB3022B376c8ff3873a4566C` are also historical; the latter is
  archived under `archive-bd6682d/`.

`deployment-transition.json` records the redeployment boundary between the
previous and current contracts.
