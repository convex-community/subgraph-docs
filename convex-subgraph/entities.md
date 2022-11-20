# Entities

### Pool

Details of Convex's staking pools

| Field             | Type                                                  | Description                                                 |
| ----------------- | ----------------------------------------------------- | ----------------------------------------------------------- |
| id                | ID                                                    | Entity ID                                                   |
| poolid            | BigInt                                                | Pool ID in the Convex Booster contract                      |
| platform          | Platform                                              | Platform                                                    |
| name              | String                                                | Pool name                                                   |
| lpToken           | Bytes                                                 | Pool LP token address                                       |
| lpTokenBalance    | BigInt                                                | Pool LP token balance                                       |
| lpTokenUSDPrice   | BigDecimal                                            | Pool LP token price in USD                                  |
| token             | Bytes                                                 | Convex receipt token                                        |
| gauge             | Bytes                                                 | Pool gauge address                                          |
| crvRewardsPool    | Bytes                                                 | CRV Rewards contract                                        |
| stash             | Bytes                                                 | Pool stash reward contract                                  |
| stashVersion      | BigInt                                                | Pool stash reward contract version                          |
| stashMinorVersion | BigInt                                                | Pool stash reward contract minor version                    |
| active            | Boolean                                               | Whether the pool is active                                  |
| isV2              | Boolean                                               | Whether the pool is v2                                      |
| creationDate      | BigInt                                                | Pool creation date                                          |
| creationBlock     | BigInt                                                | Pool creation block                                         |
| tvl               | BigDecimal                                            | Pool TVL in USD                                             |
| curveTvlRatio     | BigDecimal                                            | Percentage of Curve LP token staked on Convex for that pool |
| crvApr            | BigDecimal                                            | Pool CRV APR                                                |
| cvxApr            | BigDecimal                                            | Pool CVX APR                                                |
| extraRewardsApr   | BigDecimal                                            | Pool's extra rewards APR (other tokens)                     |
| baseApr           | BigDecimal                                            | Pool's base APR (from Curve)                                |
| deposits          | \[[Deposit](entities.md#deposit)]                     | Deposits                                                    |
| withdrawals       | \[[Withdrawal](entities.md#withdrawal)]               | Withdrawals                                                 |
| crvRewards        | \[[PoolReward](entities.md#poolreward)]               | CRV Rewards                                                 |
| swap              | Bytes                                                 | Curve pool contract address                                 |
| assetType         | Int                                                   | Underlying asset type (USD, BTC...)                         |
| extras            | \[String]                                             | Extra reward contracts                                      |
| extraRewards      | \[[ExtraReward](entities.md#extrareward)]             | Extra reward contracts                                      |
| coins             | \[Bytes]                                              | Pool coins                                                  |
| snapshots         | \[[DailyPoolSnapshot](entities.md#dailypoolsnapshot)] | Pool snapshots                                              |

### DailyPoolSnapshot

Historical snpashots of Convex's staking pools

| Field               | Type                     | Description                                                 |
| ------------------- | ------------------------ | ----------------------------------------------------------- |
| id                  | ID                       | Entity ID                                                   |
| poolid              | [Pool](entities.md#pool) | Platform                                                    |
| poolName            | String                   | Pool name                                                   |
| withdrawalCount     | BigInt                   | Number of withdrawals                                       |
| depositCount        | BigInt                   | Number of deposits                                          |
| withdrawalVolume    | BigInt                   | Total amount of LP tokens withdrawn                         |
| depositVolume       | BigInt                   | Total amount of LP tokens deposited                         |
| withdrawalValue     | BigDecimal               | Total USD value of withdrawals                              |
| depositValue        | BigDecimal               | Total USD value of deposits                                 |
| lpTokenBalance      | BigInt                   | Pool LP token balance                                       |
| lpTokenVirtualPrice | BigDecimal               | Pool LP token virtual price                                 |
| lpTokenUSDPrice     | BigDecimal               | Pool LP token price in USD                                  |
| xcpProfit           | BigDecimal               | Pool xcpProfit                                              |
| xcpProfitA          | BigDecimal               | Pool xcpProfitA                                             |
| tvl                 | BigDecimal               | Pool TVL in USD                                             |
| curveTvlRatio       | BigDecimal               | Percentage of Curve LP token staked on Convex for that pool |
| crvApr              | BigDecimal               | Pool CRV APR                                                |
| cvxApr              | BigDecimal               | Pool CVX APR                                                |
| extraRewardsApr     | BigDecimal               | Pool's extra rewards APR (other tokens)                     |
| baseApr             | BigDecimal               | Pool's base APR (from Curve)                                |
| timestamp           | BigInt                   | Snapshot timestamp                                          |
| block               | BigInt                   | Snapshot block                                              |

### RevenueWeeklySnapshot

Snapshots of revenue generated by Convex from Curve/CRV

| Field                                     | Type                                                | Description                                                                                    |
| ----------------------------------------- | --------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| crvRevenueToLpProvidersAmount             | BigInt                                              | Weekly revenue distributed to LP Providers in CRV (CRV rewards)                                |
| crvRevenueToCvxCrvStakersAmount           | BigInt                                              | Weekly revenue distributed to CvxCrv Stakers in CRV ('Lock' rewards)                           |
| crvRevenueToCvxStakersAmount              | BigInt                                              | Weekly revenue distributed to CVX stakers                                                      |
| crvRevenueToCallersAmount                 | BigInt                                              | Weekly revenue distributed as caller incentives (for harvests)                                 |
| crvRevenueToPlatformAmount                | BigInt                                              | Weekly revenue going to treasury                                                               |
| totalCrvRevenue                           | BigInt                                              | Total weekly revenue from Curve Pools                                                          |
| crvRevenueToLpProvidersAmountCumulative   | BigInt                                              | Cumulative revenue distributed to LP Providers in CRV                                          |
| crvRevenueToCvxCrvStakersAmountCumulative | BigInt                                              | Cumulative revenue distributed to CvxCrv Stakers in CRV ('Lock' rewards)                       |
| crvRevenueToCvxStakersAmountCumulative    | BigInt                                              | Cumulative revenue distributed to CVX stakers                                                  |
| crvRevenueToCallersAmountCumulative       | BigInt                                              | Cumulative revenue distributed as caller incentives (for harvests)                             |
| crvRevenueToPlatformAmountCumulative      | BigInt                                              | Cumulative revenue going to treasury                                                           |
| totalCrvRevenueCumulative                 | BigInt                                              | Total cumulative revenue                                                                       |
| crvPrice                                  | BigDecimal                                          | crv USD price as (price\_at\_time\_of\_previous\_snapshot + price\_at\_time\_of\_snapshot) / 2 |
| timestamp                                 | BigInt                                              | Snapshot timestamp                                                                             |
| extraTokenRewards                         | \[[ExtraTokenReward](entities.md#extratokenreward)] | Extra token rewards                                                                            |

### FeeRevenue

Internal use. Snapshots of the fee amount

| Field     | Type     | Description        |
| --------- | -------- | ------------------ |
| id        | ID       | Entity ID          |
| platform  | Platform | Platform           |
| amount    | BigInt   | Fee revenue amount |
| timestamp | BigInt   | Snapshot timestamp |

### ExtraTokenReward

| Field           | Type                                                       | Description       |
| --------------- | ---------------------------------------------------------- | ----------------- |
| id              | ID                                                         | Entity ID         |
| revenueSnapshot | [RevenueWeeklySnapshot](entities.md#revenueweeklysnapshot) | Revenue snapshots |
| token           | Bytes                                                      | Token address     |
| amount          | BigInt                                                     | Reward amount     |
| value           | BigDecimal                                                 | Reward USD value  |

### Deposit

| Field     | Type                     | Description                   |
| --------- | ------------------------ | ----------------------------- |
| id        | ID                       | Entity ID                     |
| user      | [User](entities.md#user) | Depositing user address       |
| poolid    | [Pool](entities.md#pool) | Convex booster id of the pool |
| amount    | BigInt                   | Deposit amount                |
| timestamp | BigInt                   | Deposit timestamp             |

### Withdrawal

| Field     | Type                     | Description                   |
| --------- | ------------------------ | ----------------------------- |
| id        | ID                       | Entity ID                     |
| user      | User                     | Withdrawing user address      |
| poolid    | [Pool](entities.md#pool) | Convex booster id of the pool |
| amount    | BigInt                   | Withdrawal amount             |
| timestamp | BigInt                   | Withdrawal timestamp          |

### User

| Field       | Type                                    | Description      |
| ----------- | --------------------------------------- | ---------------- |
| id          | ID                                      | Entity ID        |
| address     | Bytes                                   | User address     |
| withdrawals | \[[Withdrawal](entities.md#withdrawal)] | User withdrawals |
| deposits    | \[[Deposit](entities.md#deposit)]       | User deposits    |

### ExtraReward

| Field    | Type  | Description                                |
| -------- | ----- | ------------------------------------------ |
| id       | ID    | Entity ID                                  |
| poolid   | Pool  | Convex booster id of the pool              |
| contract | Bytes | Contract address handling the extra reward |
| token    | Bytes | Token the extra reward is given in         |

### PoolReward

| Field      | Type   | Description                   |
| ---------- | ------ | ----------------------------- |
| id         | ID     | Entity ID                     |
| poolid     | Pool   | Convex booster id of the pool |
| contract   | Bytes  | Staking contract address      |
| crvRewards | BigInt | CRV rewards amount            |
| timestamp  | BigInt | Timestamp                     |

### Platform

| Field      | Type                                                          | Description              |
| ---------- | ------------------------------------------------------------- | ------------------------ |
| id         | ID                                                            | Entity ID                |
| poolCount  | BigInt                                                        | Number of pools          |
| curvePools | \[[Pool](entities.md#pool)]                                   | Pools                    |
| revenue    | \[[RevenueWeeklySnapshot](entities.md#revenueweeklysnapshot)] | Revenue snapshots        |
| feeRevenue | \[[FeeRevenue](entities.md#feerevenue)]                       | Historical fee snapshots |



