<div align="center">

<img src="https://raw.githubusercontent.com/use-peg/.github/main/assets/peg-banner.svg" width="100%" alt="Peg — Real-world assets. Onchain momentum." />

# Peg

**Tokenized stock markets, community liquidity, and a new way to launch.**

Peg brings stock-token liquidity, wallet-native trading, and creator-driven token launches into one workspace on **Robinhood Chain**.

[**Launch app ↗**](https://usepeg.trade/app) · [Explore pools](https://usepeg.trade/app?tab=pools&screen=open) · [Launch with PegPad](https://usepeg.trade/app?tab=pegpad&screen=open) · [Follow @use_peg](https://x.com/use_peg)

<img src="https://img.shields.io/badge/NETWORK-Robinhood_Chain-17151F?style=flat-square&labelColor=292334&color=BCA3E8" alt="Robinhood Chain" />
<img src="https://img.shields.io/badge/CHAIN_ID-4663-17151F?style=flat-square&labelColor=292334&color=BCA3E8" alt="Chain ID 4663" />
<img src="https://img.shields.io/badge/INTERFACE-Live-17151F?style=flat-square&labelColor=292334&color=BCA3E8" alt="Live interface" />

</div>

---

## One workspace. More ways to participate.

Stocks are becoming programmable assets. Peg gives those assets places to trade, liquidity to move through, and a connection to the communities building around them.

| Product | What you can do | Under the hood |
| :--- | :--- | :--- |
| **Peg Pools** | Explore stock-token markets and manage liquidity. | Stock wrappers, USDG pairs, LP shares, and fees retained in pool reserves. |
| **Swap** | Exchange supported assets from your connected wallet. | Fresh transaction plans, simulation, explicit approvals, and receipt verification. |
| **Community Pools** | Bring an ERC-20 contract address and create a token / USDG pool. | A dedicated pool contract, validated token identity, and normal deposit / withdrawal controls. |
| **PEG Staking** | Deposit PEG and withdraw recorded principal. | A dedicated non-upgradeable vault; external rewards are separate from principal custody. |
| **PegPad** | Launch a coin paired with an approved tokenized stock. | Pons launch infrastructure and an immutable creator-fee route into payouts, liquidity, and PEG buybacks. |
| **Analytics & Activity** | Inspect positions, transactions, and recorded market performance. | Onchain counters and recorded observations, with missing data kept explicit. |

## PegPad — give your coin a stock pair.

Pick a name. Upload an avatar. Choose a tokenized stock. Launch through **[Pons](https://x.com/ponsdotfamily)** from the Peg interface.

Your coin's creator fees can do more than accumulate. PegPad directs the amount available to its creator-fee route into three destinations:

<table>
<tr>
<td align="center" width="33%"><h3>50%</h3><b>To the creator</b><br/><sub>Your share of the creator-fee proceeds.</sub></td>
<td align="center" width="33%"><h3>25%</h3><b>To Peg Pools</b><br/><sub>Build liquidity in the selected Peg stock market.</sub></td>
<td align="center" width="33%"><h3>25%</h3><b>To PEG buyback & burn</b><br/><sub>Buy PEG and reduce its supply.</sub></td>
</tr>
</table>

**No additional PegPad creator tax.** The split applies to available creator-fee proceeds after Pons' protocol share, not total trading volume. Pons' own trading fees and opening protections still apply.

The current workflow collects and distributes accumulated fees through an explicit **Creator fees** transaction. It is not an automatic payout after every trade. After a token graduates from its launch curve, fee conversion can depend on Pons operator execution.

[**Open PegPad ↗**](https://usepeg.trade/app?tab=pegpad&screen=open)

---

## Built around explicit assets and explicit execution.

**Backed wrappers.** Peg stock wrappers account for the underlying stock token in raw units. New market plans use zero wrap and unwrap fees. Unwrapping returns the underlying stock token; it is not a promise of cash redemption or direct equity ownership.

**Liquidity that belongs to LPs.** Peg and Community pools use a 0.30% swap fee retained in reserves. LP shares represent a proportional claim on pool assets. Fees, inventory changes, and market prices all affect a position; liquidity provision does not guarantee profit.

**Constrained execution.** The RWA execution path connects an external stock-token venue with a Peg wrapper and pool. Transactions are simulated against their complete route and bounded by configured capital and output constraints. An available route is not a guarantee of a profitable opportunity.

**Wallet-confirmed state changes.** Transaction intent is persisted before sending, bound to its account and exact plan, and reconciled with canonical receipts. Reopening a page recovers pending work without silently replaying a financial action.

**Principal and rewards are separate.** The PEG staking vault holds and returns principal. The interface's configured 12% external-reward rate is not yield generated or guaranteed by that vault; reward payments require a separate module.

## A real lifecycle, with receipts.

The initial **PegPad Test / PPTEST / NVDA** launch and its first fee distribution were completed on Robinhood Chain. These are historical execution proofs, not projections:

| Onchain proof | What it establishes |
| :--- | :--- |
| [Token creation](https://robinhoodchain.blockscout.com/tx/0x286be33e86904c2124409c678e3b1576b6a58971d2bb2ce8248451666fbd17d2) | An actual stock-paired token launched through the PegPad flow. |
| [Creator-fee settlement](https://robinhoodchain.blockscout.com/tx/0x230cef0b61379e039a337d990a2aa5fb25fd38b0d06df62b419e9f8885c40300) | Creator payout, Peg liquidity provision, and **4,107.201898 PEG burned** in one settled operation. |

<details>
<summary><b>Technical stack & integration surface</b></summary>

<br/>

| Layer | Implementation |
| :--- | :--- |
| Contracts | Solidity, ERC-20 assets, dedicated LP and principal-custody contracts, frozen deployment artifacts. |
| Wallet & chain | TypeScript, ethers v6, account-bound transaction plans, canonical receipt reconciliation. |
| Application | Next.js 16, React 19, same-origin server APIs, Zod input validation. |
| Studio | Three.js and spring-based motion for the monitor-led product interface. |
| Metadata | Validated image uploads, sanitized WebP, content-addressed public avatars. |
| Persistence | Confirmed deployment registries and durable transaction journals. |

This organization is the public home for Peg’s project overview and technical documentation.

</details>

## Build with a clear view of the system.

The application is live. Contract verification, a successful transaction, and an independent security audit are different things: **no independent audit is claimed here**. Underlying-token restrictions, liquidity, network conditions, and external execution dependencies still matter.

Explore the app, inspect the receipts, and follow the next releases.

---

<div align="center">

**Peg · Assets on air.**

[usepeg.trade](https://usepeg.trade) · [@use_peg](https://x.com/use_peg) · [Product docs](https://usepeg.trade/app?tab=docs&screen=open)

<sub>Built on Robinhood Chain. Independent of Robinhood. Product and execution details reflect the September 11, 2026 release.</sub>

</div>
