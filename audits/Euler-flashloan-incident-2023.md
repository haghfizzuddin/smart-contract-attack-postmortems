# Euler Finance Flashloan Exploit (March 2023)

## Summary
- **Victim:** Euler Finance (Lending Protocol)
- **Date:** March 13, 2023
- **Total Loss:** ≈ $197 million USD
- **Primary Root Cause:** Missing liquidity health checks in the `donateToReserves()` function allowed malicious self-liquidation and draining of protocol funds.

---

## Technical Breakdown

### Vulnerability Details
Euler's lending protocol implemented a `donateToReserves()` function without validating the health factor of the borrower account after donation actions.  
This oversight enabled attackers to:
- Manipulate collateral health calculations,
- Execute **self-liquidations** at favorable exchange rates,
- Use flashloans to cycle the process, draining protocol reserves.

### Exploit Path
1. **Borrow assets using flashloan**, leveraging large capital in a single transaction.
2. **Artificially manipulate reserves** via `donateToReserves()` without triggering solvency checks.
3. **Liquidate self-owned positions**, profiting from manipulated collateral ratios.
4. **Repeat until draining ~$197M** from protocol.

---

## Impact Overview
- **Total Loss:** ~$197M across multiple assets (DAI, USDC, WBTC, ETH).
- **Recovery:** ~$190M returned voluntarily by the attacker after on-chain negotiations.
- **Recovered Rate:** ≈ 96% funds recovered — one of the largest successful post-hack recoveries.
- **Chain:** Ethereum mainnet.

---

## Timeline Snapshot
| Time (UTC) | Event |
|-------------|-------|
| March 13 | Attack executed using flashloan manipulations. |
| March 14 | Public confirmation by Euler Labs. |
| March 16–25 | Negotiations with attacker commence via on-chain messages. |
| April 3 | Majority of funds recovered by Euler. |

---

## Mitigation & Lessons Learned

| Area | Actionable Takeaways |
|-------|----------------------|
| **Code Review** | Complex DeFi functions like `donateToReserves()` must include post-action health validations. |
| **Auditing** | Require multiple auditors to focus on **cross-function interactions** affecting solvency. |
| **Flashloan Awareness** | All lending functions should account for **flashloan-powered state manipulations**. |
| **Protocol Design** | Implement **pause mechanisms** and **circuit breakers** for anomalous fund flows. |

---

## References
- [Euler Blog — Full Recovery Announcement](https://www.euler.finance/blog/war-peace-behind-the-scenes-of-eulers-240m-exploit-recovery)
- [Chainalysis Overview of the Exploit](https://www.chainalysis.com/blog/euler-finance-flash-loan-attack/)
- [Hacken Technical Breakdown](https://hacken.io/discover/euler-finance-hack/)
- [Cointelegraph Coverage](https://cointelegraph.com/news/euler-finance-recovers-90-of-stolen-funds)

---

## Quick Summary
**Exploit:** Flashloan abuse via `donateToReserves()`  
**Impact:** $197M drained, $190M recovered  
**Lesson:** Cross-function liquidity manipulation can cause massive financial loss even in “audited” protocols.

---

**Last updated:** July 2025  
**Author:** Hafizzuddin
