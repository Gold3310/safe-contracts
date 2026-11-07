# Safe Contracts — Hardhat Remote Signer Configuration

**Repository:** `Gold3310/safe-contracts`  
**Baseline:** `main` at `f8fc2f2e75b650d63276aaa12cd3a455a5f5a990`  
**Research branch:** `security-fix/no-remote-default-mnemonic-2026-11-07`

## Finding SC-01 — Public Hardhat test mnemonic assigned to remote networks

**Severity:** High deployment-key risk; conditional on a remote network using the fallback signer and that account receiving funds.

**Affected file:** `hardhat.config.ts`.

The original configuration used the well-known Hardhat test mnemonic `candy maple cake sugar pudding cream honey rich smooth crumble sweet treat` when neither `PK` nor `MNEMONIC` was provided. That shared account configuration was applied to remote `mainnet`, `sepolia`, `gnosis`, `zksync`, and `custom` networks.

If an operator points the configuration at a live chain and funds the derived default account, anyone who knows the public mnemonic can derive the same private key and spend those funds.

**Remediation on this branch:** Remote network accounts are configured only when `PK` or `MNEMONIC` is explicitly provided. The local Hardhat network retains its built-in test accounts.

**Operational effect:** Remote deployment without an explicit signer fails rather than silently selecting a publicly known account.

This is a deployment configuration issue, not a demonstrated vulnerability in deployed Safe contracts and not a bounty submission. No private keys were accessed and no on-chain transactions were sent.
