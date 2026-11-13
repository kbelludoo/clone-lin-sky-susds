# clone-lin-sky-susds

Experimental **LIN** clone of Sky Protocol `SUsds.convertToAssets` / `convertToShares` — ERC-4626 share conversion against the savings-rate accumulator `chi` (`assets = shares * chi / RAY`, `shares = assets * RAY / chi`, `previewMint = _divup(shares * chi, RAY)`).

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [sky-ecosystem/sdai](https://github.com/sky-ecosystem/sdai) `dfc7f41cb7599afcb0f0eb1ddaadbf9dd4015dce` branch `susds`
  - `src/SUsds.sol` sha256 `9fe0c713751142e75a1da60ad6c0127d5ad01cb6d24289183ac48b202f3c5d69` blob `efe937675bbbbc7f93bde453fd5648fa885655ab`
  - License AGPL-3.0-or-later. This repository does **not** vendor the Solidity source.
- **LIN kernel:** [`src/lin_sky_susds.lin`](src/lin_sky_susds.lin) (identical to lin-open)
- **C11 oracle copy:** [`test/oracles/sky_susds_c11.c`](test/oracles/sky_susds_c11.c)
- **Transpilable kernel:** [`test/oracles/sky_susds_kernel.c`](test/oracles/sky_susds_kernel.c)
- **Proofs (lin-open only):** `examples/sky_susds/` and `make sky-susds-proof`
- **Class:** EXPERIMENTAL uint64-scale. Not mainnet uint256 ray. Not a production sUSDS replacement.

Named goldens: PAR `chi=1000` shares=500 → assets **500**;
ACCRUED `chi=1100` shares=1000 → assets **1100** / assets=1100 → shares **1000**;
FLOOR 1 share → **1** asset, 1 asset → **0** shares;
CEIL `previewMint(1)` → **2** assets, `previewWithdraw(1)` → **1** share;
ROUNDTRIP 100 assets loses **1**; `chi=0` fail-closes;
convertTo* are not wards-auth; `_rpow` / `block.timestamp` / OZ virtual offset are absent;
OVERFLOW `7e9*3e9` fail-closes while naive i64 wraps.

## Reproduce proofs (in lin-open)

```
make sky-susds-proof
```

## Not claimed

Uint256 ray / mainnet sUSDS / drip `_rpow` parity.
LIN replacing Sky, Maker, sUSDS, or the EVM. TVL or dollar savings. zk soundness.
OZ ERC-4626 virtual shares. `pot.drip`. Hidden `chi`/`rho`/`ssr` storage, `_rpow` assembly, or `block.timestamp`.
