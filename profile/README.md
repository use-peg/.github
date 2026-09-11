<a href="https://usepeg.trade/app"><img src="https://raw.githubusercontent.com/use-peg/.github/main/assets/peg-cover.png" width="100%" alt="Peg — Liquidity for what comes next." /></a>

<div align="center">

[**Launch app ↗**](https://usepeg.trade/app) &nbsp; · &nbsp; [**Explore the contracts**](https://github.com/use-peg/peg-contracts) &nbsp; · &nbsp; [**Follow Peg**](https://x.com/use_peg)

**Tokenized stocks. Connected communities. One onchain workspace.**

</div>

Peg connects stock-token markets, community liquidity and token launches on **Robinhood Chain**. Trade, provide liquidity, stake PEG, or launch a coin with a stock pair — from one physical, monitor-led interface.

## The Peg ecosystem

| | Built for |
| :--- | :--- |
| **Pools & Swap** | Stock-token / USDG markets, liquidity positions and wallet-native trading. |
| **Community Pools** | Bring an ERC-20 address. Create a market. Deposit and withdraw liquidity. |
| **PEG Staking** | Deposit PEG and withdraw your principal from an independent custody vault. |
| **PegPad** | Give your token a stock pair and direct creator fees into your community and Peg. |

## Open the Solidity

[**peg-contracts ↗**](https://github.com/use-peg/peg-contracts) contains three actual Peg primitives, pinned builds and **30 staking tests**.

| Contract | Role |
| :--- | :--- |
| [PegAsset.sol](https://github.com/use-peg/peg-contracts/blob/main/PegAsset.sol) | Fixed-unit stock-token custody wrappers. |
| [PegPool.sol](https://github.com/use-peg/peg-contracts/blob/main/PegPool.sol) | Constant-product pools, ERC-20 LP shares, 30 bps retained swap fees. |
| [PegStakingVault.sol](https://github.com/use-peg/peg-contracts/blob/main/PegStakingVault.sol) | Immutable token and chain bindings; caller-owned principal. |

**Principal out. Directly to its owner.** This is the actual withdrawal entry point in our staking vault:

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

No owner withdrawal or upgrade switch. Exact token transfers and reentrancy protection. Rewards remain separate from principal custody.

## PegPad · A stock pair for your next idea

**Name it. Give it a face. Pick its stock. Launch.**

PegPad connects to [Pons](https://x.com/ponsdotfamily) to launch coins paired with approved tokenized stocks. Accumulated creator-fee proceeds have three destinations:

<table>
<tr>
<td align="center" width="33%"><h2>50%</h2><b>Creator</b><br/><sub>Build your next move.</sub></td>
<td align="center" width="33%"><h2>25%</h2><b>Peg liquidity</b><br/><sub>Deepen the stock market.</sub></td>
<td align="center" width="33%"><h2>25%</h2><b>PEG buyback & burn</b><br/><sub>Return value to the ecosystem.</sub></td>
</tr>
</table>

The split uses available creator proceeds after Pons’ protocol share. PegPad adds no additional creator tax; Pons trading fees still apply. Fees are processed through an explicit **Creator fees** transaction. Post-graduation conversion can depend on the Pons operator.

**Already executed:** [PPTEST / NVDA launch](https://robinhoodchain.blockscout.com/tx/0x286be33e86904c2124409c678e3b1576b6a58971d2bb2ce8248451666fbd17d2) · [Creator payout + liquidity + 4,107.201898 PEG burned](https://robinhoodchain.blockscout.com/tx/0x230cef0b61379e039a337d990a2aa5fb25fd38b0d06df62b419e9f8885c40300).

## Engineered across the stack

**Solidity** + OpenZeppelin for assets and custody. **TypeScript** + ethers for exact transaction plans and receipt recovery. **Next.js** + React for the app. **Three.js** + spring motion for the studio.

Account-bound transaction intent, fresh simulation and canonical receipts connect the interface to onchain outcomes. Public avatars are sanitized, content-addressed images. Analytics distinguish recorded observations from missing data.

<details>
<summary><b>Execution notes</b></summary>

Stock wrappers return their underlying token, not cash or direct legal equity ownership. LP returns depend on fees, inventory and market conditions. The staking vault holds principal; the configured 12% external-reward rate requires a separate reward module. Source publication and historical execution proofs do not constitute an independent security audit. Deployment identities and reproduction steps are documented in the contracts repository.

</details>

---

<div align="center">

**Peg · Assets on air.**

[Application](https://usepeg.trade/app) · [Solidity](https://github.com/use-peg/peg-contracts) · [Docs](https://usepeg.trade/app?tab=docs&screen=open) · [X / use_peg](https://x.com/use_peg)

<sub>Robinhood Chain · 4663 · Independent of Robinhood · September 2026</sub>

</div>
