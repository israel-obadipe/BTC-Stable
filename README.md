# BTC-Stable Protocol Documentation

## Overview

BTC-Stable is a decentralized protocol enabling the creation of USD-pegged stablecoins fully collateralized by Bitcoin. The system combines decentralized finance (DeFi) principles with robust risk management mechanisms to maintain price stability while ensuring capital efficiency.

## Key Features

- **Bitcoin-Backed Stablecoins**: 1:1 USD peg backed by overcollateralized BTC
- **Dynamic Risk Parameters**: Auto-adjusting collateralization ratios
- **Decentralized Oracles**: Secure price feed integration
- **Liquidation Engine**: Automated protection against undercollateralization
- **Governance Ready**: DAO-managed parameter configuration
- **Emergency Protocols**: Circuit-breaker mechanisms for system protection

## Contract Architecture

### Core Components

1. **Vault Management System**

   - User collateral deposits
   - Debt position management
   - Collateral ratio monitoring

2. **Risk Management Framework**

   - Minimum Collateral Ratio (MCR): 150% (configurable)
   - Liquidation Ratio: 120% (configurable)
   - Stability Fees: 2% annual (configurable)

3. **Oracle Network**
   - Decentralized price feeds
   - Multiple authenticated sources
   - Price validation mechanisms

### System Parameters

| Parameter                  | Default   | Range     | Description                           |
| -------------------------- | --------- | --------- | ------------------------------------- |
| `minimum-collateral-ratio` | 150%      | 101-1000% | Minimum collateralization requirement |
| `liquidation-ratio`        | 120%      | 101-1000% | Collateral threshold for liquidation  |
| `stability-fee`            | 2%        | 0-100%    | Annual fee on outstanding debt        |
| `price-validity-window`    | 30 blocks | -         | Maximum acceptable price age          |

## Core Functions

### Vault Operations

1. **Create Vault**

   ```clarity
   (create-vault (collateral-amount uint))
   ```

   - Initializes a new collateral position
   - Transfers STX (Bitcoin placeholder) to contract

2. **Mint Stablecoins**

   ```clarity
   (mint-stablecoin (amount uint))
   ```

   - Generates new stablecoins against collateral
   - Requires maintaining MCR

3. **Repay Debt**

   ```clarity
   (repay-debt (amount uint))
   ```

   - Reduces outstanding stablecoin debt
   - Burns repaid stablecoins

4. **Withdraw Collateral**
   ```clarity
   (withdraw-collateral (amount uint))
   ```
   - Releases collateral from vault
   - Requires maintaining MCR post-withdrawal

### Liquidation System

**Trigger Conditions:**

- Collateral value < (Debt × Liquidation Ratio)
- Price feed validity within parameters

**Liquidation Process:**

```clarity
(liquidate (vault-owner principal))
```

1. Checks collateral health status
2. Transfers remaining collateral to liquidator
3. Closes vault position

## Oracle Management

### Price Feed Updates

```clarity
(update-price (new-price uint))
```

- Authenticated oracles only
- Validates price within $1-$1B range
- Updates timestamped price record

### Oracle Governance

```clarity
(add-oracle (oracle principal))
(remove-oracle (oracle principal))
```

- Owner-managed oracle registry
- Multi-sig capability for critical operations

## Governance Controls

### Parameter Adjustment

```clarity
(set-minimum-collateral-ratio (new-ratio uint))
(set-liquidation-ratio (new-ratio uint))
(set-stability-fee (new-fee uint))
```

- Owner-restricted functions
- Parameter validation safeguards
- Time-lock capability (future implementation)

### Emergency Protocols

```clarity
(trigger-emergency-shutdown)
```

- Global system freeze
- Disables new minting/withdrawals
- Enables orderly liquidation

## Security Model

### Key Protections

1. **Collateral Safeguards**

   - Overcollateralization requirements
   - Withdrawal restrictions
   - Price validity checks

2. **System Integrity**

   - Oracle authentication
   - Liquidation incentives
   - Emergency shutdown capability

3. **Governance Controls**
   - Parameter change safeguards
   - Multi-sig authorization
   - DAO integration readiness

### Error Codes

| Code | Description                    |
| ---- | ------------------------------ |
| u100 | Owner-only function access     |
| u101 | Insufficient collateral        |
| u102 | Below minimum collateral ratio |
| u103 | Contract already initialized   |
| u104 | Contract not initialized       |
| u105 | Insufficient balance           |
| u106 | Invalid price data             |
| u107 | Emergency shutdown active      |
| u108 | Invalid parameter value        |

## Risk Considerations

- **Price Oracle Risk**: Dependence on external price feeds
- **Liquidation Timing**: Market volatility impacts
- **Governance Centralization**: Initial owner controls
- **Smart Contract Risk**: Protocol code vulnerabilities

---

**Note:** This implementation uses Stacks (STX) as a placeholder for Bitcoin collateral. Mainnet deployment would require Bitcoin interoperability solutions such as wrapped BTC or Layer-2 integration.
