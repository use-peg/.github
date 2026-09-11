<a href="https://usepeg.trade/app"><img src="https://raw.githubusercontent.com/use-peg/.github/main/assets/peg-cover-v3.png" width="100%" alt="Peg — Tokenized stocks. Community liquidity." /></a>

<div align="center">

[**Launch app ↗**](https://usepeg.trade/app) &nbsp; · &nbsp; [**Explore Solidity**](https://github.com/use-peg/peg-contracts) &nbsp; · &nbsp; [**Product docs**](https://usepeg.trade/app?tab=docs&screen=open) &nbsp; · &nbsp; [**X / use_peg**](https://x.com/use_peg)

**Stock-token markets, community pools, PEG staking and stock-paired token launches on Robinhood Chain.**

</div>

## What is Peg?

Peg is an onchain application built around **tokenized stocks and the liquidity surrounding them**. It brings market access, liquidity management, community-created pools, PEG principal staking and token launches into one connected workspace.

The idea is practical: a tokenized asset becomes more useful when people can trade it, supply liquidity around it and build new projects connected to its market. Peg provides those entry points through a wallet interface, with the underlying assets and transaction results kept visible.

For liquidity providers, Peg offers market and position controls. For communities, it offers a way to create a token / USDG pool. For creators, PegPad connects a new token to an approved stock-token pair and routes creator-fee proceeds into payouts, Peg liquidity and PEG buybacks.

**Network:** Robinhood Chain · **Chain ID:** `4663` · **Application:** [usepeg.trade](https://usepeg.trade)

### PEG token

**Contract address · Robinhood Chain**

```text
0x7f362d5ef8b02cedf2b5c5f76b9d7aaa667085c8
```

[**View PEG on the explorer ↗**](https://robinhoodchain.blockscout.com/address/0x7f362d5ef8b02cedf2b5c5f76b9d7aaa667085c8)

## Explore the ecosystem

| Product | What it enables | Start here |
| :--- | :--- | :--- |
| **Pools** | Explore supported markets, deposit liquidity and manage LP positions. | [Explore pools](https://usepeg.trade/app?tab=pools&screen=open) |
| **Swap** | Trade supported assets with explicit amounts, approvals and transaction confirmation. | [Open Swap](https://usepeg.trade/app?tab=swap&screen=open) |
| **Community Pools** | Discover a token by contract address and create a token / USDG pool. | [Create a pool](https://usepeg.trade/app?tab=community-pools&screen=open) |
| **PEG Staking** | Deposit PEG and withdraw the principal recorded for your wallet. | [Open Staking](https://usepeg.trade/app?tab=staking&screen=open) |
| **PegPad** | Launch a token paired with an approved tokenized stock through Pons. | [Open PegPad](https://usepeg.trade/app?tab=pegpad&screen=open) |
| **Analytics & Activity** | Inspect positions, settled transactions and recorded market observations. | [Launch the app](https://usepeg.trade/app) |

## Stock tokens and Peg liquidity

### Backed wrappers

`PegAsset` is a fixed-unit custody wrapper. It receives an underlying stock token and issues wrapped units against that asset. New v8 market plans configure zero wrap and unwrap fees; the underlying constructor supports bounded immutable fee settings for other versions.

Unwrapping burns wrapped units and returns the underlying token according to the configured contract. The backing is denominated in token units. It does not depend on the interface inventing a price, and it does not turn the wrapper into a cash-redemption service or direct legal equity ownership.

### Pool assets and LP shares

A Peg stock market pairs its wrapped stock token with **USDG**. Liquidity providers supply the pool assets and receive LP shares. Those shares represent a proportional claim on the pool's reserves. When liquidity is removed, the position returns its share of both assets under the pool's rules.

<img src="https://raw.githubusercontent.com/use-peg/.github/main/assets/peg-market-flow.svg" width="100%" alt="Stock token to Peg wrapper to Peg stock-token/USDG pool to LP shares, with USDG supplying the pool" />

The constant-product pool family retains a **0.30% swap fee** in its reserves. Liquidity providers participate through their LP share of those reserves. Fees, the changing mix of assets and market prices all affect a position's value; a fee rate alone is not an APR or a profit forecast.

Peg's native-entry routing can combine supported ETH or USDG entry steps into one planned operation. Its contract binds the supported markets and venues, spends the current call's balance deltas and returns applicable leftovers. Availability depends on the registered route and current liquidity.

### Swap execution

Trading starts with a specific asset, amount and route. The wallet flow separates required token approvals from the actual swap, simulates the planned operation and checks its receipt before treating it as complete. Minimum output and deadline constraints are part of the transaction, so a displayed quote is not an unlimited authorization to trade.

## Community Pools

**Bring a token address. Create a market around it.**

Community Pools extends pool creation beyond Peg's curated stock markets. A creator supplies an ERC-20 contract address on Robinhood Chain. Peg reads the token identity, prepares its direct pairing with USDG and requests the necessary wallet confirmation.

1. **Identify the asset.** Read the token contract, metadata and decimals from the selected chain.
2. **Create the pool.** Review and confirm the deployment transaction from the connected wallet.
3. **Establish liquidity.** Supply both assets; the first two-sided deposit sets the pool's initial ratio.
4. **Manage the position.** Use the Community view in Pools to access deposit and withdrawal controls.

A confirmed deployment establishes that the pool contract exists. Liquidity is a separate step: an empty contract is not yet a funded market. Likewise, a token name, avatar or RWA category is descriptive metadata, not an endorsement of the issuer.

Community assets do not automatically join the curated stock catalog, native-entry allowlist or operator execution routes. Each part of the system has its own registration and asset requirements.

<img src="https://raw.githubusercontent.com/use-peg/.github/main/assets/peg-coins.png" width="100%" alt="Peg token artwork: ribbed metallic Peg coins on a black background" />

## PEG staking: principal first

The staking interface supports depositing **PEG** into `PegStakingVault` and withdrawing the balance recorded for the caller. The vault permanently binds its token and expected chain at construction.

Its responsibility is deliberately narrow: hold principal, account for each depositor and return that principal when a valid withdrawal is requested. It has no owner withdrawal, upgrade switch, lock period, deposit fee, principal investment or donation sweep. Reward processing does not receive authority over the vault.

The withdrawal entry point below is taken directly from the published contract:

```solidity
function withdraw(uint256 amount) external nonReentrant {
    _checkChain();
    if (amount == 0) revert InvalidAmount();
    if (amount > stakedBalance[msg.sender]) revert InsufficientStake();
    _requireBacking();
    stakedBalance[msg.sender] -= amount;
    totalStaked -= amount;
    _sendExact(msg.sender, amount);
    emit Withdrawn(msg.sender, amount, stakedBalance[msg.sender]);
}
```

OpenZeppelin reentrancy protection and exact sender/recipient balance checks protect the accounting assumptions. Unsupported transfers revert instead of silently crediting a different amount. Underlying-token freezes or changed transfer rules can still prevent a transfer.

The interface's configured **12% external-reward rate** belongs to a separate reward module. It is not yield generated or guaranteed by the principal vault. `Staked` and `Withdrawn` events provide position changes that an external indexer can consume.

[**Read PegStakingVault.sol ↗**](https://github.com/use-peg/peg-contracts/blob/main/PegStakingVault.sol)

<a href="https://usepeg.trade/app?tab=pegpad&screen=open"><img src="https://raw.githubusercontent.com/use-peg/.github/main/assets/pegpad-wordmark.jpg" width="100%" alt="PegPad — the Peg token launchpad" /></a>

## PegPad: launch a token with a stock pair

PegPad is the launchpad inside Peg. It connects to **[Pons](https://x.com/ponsdotfamily)** so a creator can launch a token paired with an approved tokenized stock on Robinhood Chain.

The creation flow starts with a token name, symbol, avatar and stock selection. Uploaded avatars become validated public images so token metadata can resolve beyond the creator's browser. Wallet-confirmed creation is followed by a **Your launches** card containing the confirmed token identity and a copyable contract address.

### Creator fees that support the ecosystem

PegPad's V2 fee router connects available creator-fee proceeds to three destinations:

<table>
<tr>
<td align="center" width="33%"><h2>50%</h2><b>Creator payout</b><br/><sub>Sent to the bound creator recipient.</sub></td>
<td align="center" width="33%"><h2>25%</h2><b>Peg liquidity</b><br/><sub>Provides liquidity to the selected Peg market.</sub></td>
<td align="center" width="33%"><h2>25%</h2><b>PEG buyback & burn</b><br/><sub>Buys PEG and burns the acquired tokens.</sub></td>
</tr>
</table>

The allocation applies to the available net creator proceeds **after Pons' protocol share**, rather than total trading volume. PegPad adds no additional creator tax; Pons' trading fees and opening protections still apply.

Fee processing is an explicit **Creator fees** transaction. The router binds its creator, executor, market and runtime commitments; price-sensitive steps use protected outputs and deadlines. Accumulated fees are not automatically distributed after every trade. Post-graduation conversion can depend on Pons operator execution.

### An executed launch and distribution

The **PegPad Test / PPTEST / NVDA** lifecycle produced real transaction receipts:

| Proof | Recorded outcome |
| :--- | :--- |
| [Token creation](https://robinhoodchain.blockscout.com/tx/0x286be33e86904c2124409c678e3b1576b6a58971d2bb2ce8248451666fbd17d2) | A token launched through the PegPad flow with an NVDA pair. |
| [Creator-fee distribution](https://robinhoodchain.blockscout.com/tx/0x230cef0b61379e039a337d990a2aa5fb25fd38b0d06df62b419e9f8885c40300) | Creator payout, Peg liquidity provision and **4,107.201898 PEG burned** in a settled operation. |

These receipts document completed historical operations. They do not predict future trading volume, fee income or execution conditions.

## Open Solidity, explicit versions

The public [**peg-contracts**](https://github.com/use-peg/peg-contracts) repository contains five selected source contracts, a pinned build and the staking custody test suite.

| Source | Responsibility |
| :--- | :--- |
| [PegAsset.sol](https://github.com/use-peg/peg-contracts/blob/main/PegAsset.sol) | Fixed-unit asset wrapping, bounded immutable fees and exact-transfer checks. |
| [PegPool.sol](https://github.com/use-peg/peg-contracts/blob/main/PegPool.sol) | Foundational constant-product AMM, LP accounting and retained swap fees. |
| [PegStakingVault.sol](https://github.com/use-peg/peg-contracts/blob/main/PegStakingVault.sol) | Caller-owned principal custody bound to an immutable token and chain. |
| [PegNativeEntry.sol](https://github.com/use-peg/peg-contracts/blob/main/PegNativeEntry.sol) | Atomic entry into registered Peg liquidity using ETH or USDG. |
| [PegPadFeeRouterV2.sol](https://github.com/use-peg/peg-contracts/blob/main/PegPadFeeRouterV2.sol) | Pons curve-fee collection and protected 50 / 25 / 25 distribution. |

Source snapshots and deployed versions are distinct. The published `PegPool.sol` is the foundational pool source; current app registries use separately frozen pool artifacts, including the `PegRevenuePool` variant exposed as PegPool. Integrations must match the actual market's artifact and constructor bindings.

The configured staking deployment is [`0x431d61b1a57F8d4fd1C0406dc424d5beEEc07A8F`](https://robinhoodchain.blockscout.com/address/0x431d61b1a57F8d4fd1C0406dc424d5beEEc07A8F). The repository includes its frozen staking artifact and independent reproduction script.

### Build locally

```sh
git clone https://github.com/use-peg/peg-contracts.git
cd peg-contracts
npm ci
npm run build
npm test
```

The package uses Node.js 22+, Solidity **0.8.36**, OpenZeppelin **5.6.1**, optimizer **200** runs and Shanghai EVM. The aggregate build uses IR; the staking reproduction script preserves its separate artifact settings. Local tests require no funded wallet or external RPC.

The **30-test staking suite** covers constructor validation, artifact reproduction, multiple depositors, partial and immediate withdrawals, transfer taxes, rebasing, issuer restrictions, donations, deficit repair, reentrancy and conservation of principal. It is a staking suite, not a complete test suite for every published component.

## From interface to confirmed state

Peg's frontend uses **Next.js, React and TypeScript**, with **Three.js** and spring-based motion for its monitor-led studio. Chain interactions use **ethers**; same-origin server APIs validate inputs with **Zod**.

The transaction lifecycle connects those layers:

| Stage | Responsibility |
| :--- | :--- |
| **Discover** | Load supported assets, registered contracts and saved display observations. |
| **Prepare** | Bind account, chain, asset, amount, route, expiry and transaction intent; obtain fresh execution data. |
| **Confirm** | Present the required operation to the connected wallet. |
| **Reconcile** | Match submitted transactions to canonical receipts and expected contract outcomes. |
| **Recover** | Restore pending intent without silently repeating a financial action. |

Saved display data keeps the interface responsive while background reads refresh it. Transaction-critical checks still need current chain state. Recorded fees, external volume, pool liquidity and position value are separate measurements; missing observations should remain explicit.

## Start with the part you need

- **Liquidity providers:** explore a supported market, inspect its assets and manage your LP position.
- **Communities:** bring an ERC-20 address and create a USDG pool, then establish liquidity.
- **Token creators:** open PegPad, choose a stock pair and inspect the creator-fee route.
- **Developers:** read the Solidity, reproduce the staking artifact and follow the contract boundaries.

Source publication, successful compilation and historical transaction proofs are not an independent security audit. Market conditions, underlying-token restrictions and external venue dependencies remain relevant. Peg is built on Robinhood Chain and is independent of Robinhood.

---

<div align="center">

**Peg · Tokenized stocks. Community liquidity.**

[Application](https://usepeg.trade/app) · [Solidity](https://github.com/use-peg/peg-contracts) · [Documentation](https://usepeg.trade/app?tab=docs&screen=open) · [X / use_peg](https://x.com/use_peg)

<sub>Product and source snapshot · September 2026</sub>

</div>
