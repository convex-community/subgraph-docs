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

### DailyRevenueSnapshot

Snapshots of revenue generated by Convex from Curve/CRV, Bribes and Frax/FXS. FXS revenue is calculated on a roughly weekly basis from distributions in the FeeDepositor contract.

| Field                           | Type                                                | Description                                                               |
| ------------------------------- | --------------------------------------------------- | ------------------------------------------------------------------------- |
| crvRevenueToLpProvidersAmount   | BigDecimal                                          | Weekly $CRV revenue distributed to LP Providers in CRV (CRV rewards)      |
| crvRevenueToCvxCrvStakersAmount | BigDecimal                                          | Weekly $CRV revenue distributed to CvxCrv Stakers in CRV ('Lock' rewards) |
| crvRevenueToCvxStakersAmount    | BigDecimal                                          | Weekly $CRV revenue distributed to CVX stakers                            |
| crvRevenueToCallersAmount       | BigDecimal                                          | Weekly $CRV revenue distributed as caller incentives (for harvests)       |
| crvRevenueToPlatformAmount      | BigDecimal                                          | Weekly $CRV revenue going to treasury                                     |
| totalCrvRevenue                 | BigDecimal                                          | Total weekly $CRV  revenue                                                |
| crvPrice                        | BigDecimal                                          | $CRV price in USD at end of snapshot                                      |
| cvxRevenueToLpProvidersAmount   | BigDecimal                                          | Daily $CVX distributed to LP Providers in USD (pro-rata CRV rewards)      |
| cvxRevenueToCvxCrvStakersAmount | BigDecimal                                          | Daily $CVX revenue distributed to CvxCrv Stakers in USD                   |
| cvxPrice                        | BigDecimal                                          | $CVX price in USD                                                         |
| fxsRevenueToCvxStakersAmount    | BigDecimal                                          | Daily $FXS revenue distributed to CVX stakers in USD                      |
| fxsRevenueToCvxFxsStakersAmount | BigDecimal                                          | Daily $FXS revenue distributed to CvxFXS Stakers in USD                   |
| fxsRevenueToLpProvidersAmount   | BigDecimal                                          | Daily $FXS distributed to LP Providers in USD                             |
| fxsRevenueToCallersAmount       | BigDecimal                                          | Daily $FXS revenue distributed as caller incentives (for harvests)        |
| fxsRevenueToPlatformAmount      | BigDecimal                                          | Daily $FXS revenue going to treasury                                      |
| totalFxsRevenue                 | BigDecimal                                          | Total Daily revenue from $FXS in USD                                      |
| fxsPrice                        | BigDecimal                                          | Latest $FXS USD price used in snapshot                                    |
| bribeRevenue                    | BigDecimal                                          | Revenue accrued from Votium bribes                                        |
| timestamp                       | BigInt                                              | Snapshot timestamp                                                        |
| extraTokenRewards               | \[[ExtraTokenReward](entities.md#extratokenreward)] | Extra token rewards (currently not in use)                                |

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

| Field                          | Type                                                          | Description                                                            |
| ------------------------------ | ------------------------------------------------------------- | ---------------------------------------------------------------------- |
| id                             | ID                                                            | Entity ID                                                              |
| poolCount                      | BigInt                                                        | Number of pools                                                        |
| curvePools                     | \[[Pool](entities.md#pool)]                                   | Pools                                                                  |
| revenue                        | \[[RevenueWeeklySnapshot](entities.md#revenueweeklysnapshot)] | Revenue snapshots                                                      |
| feeRevenue                     | \[[FeeRevenue](entities.md#feerevenue)]                       | Historical fee snapshots                                               |
| bribeFee                       | BigInt                                                        | Votium latest bribe fee (internal)                                     |
| totalCrvRevenueToLpProviders   | BigDecimal                                                    | Cumulative CRV revenue distributed to LP Providers in USD              |
| totalCvxRevenueToLpProviders   | BigDecimal                                                    | Cumulative CVX revenue distributed to LP Providers in USD              |
| totalFxsRevenueToLpProviders   | BigDecimal                                                    | Cumulative FXS revenue distributed to LP Providers in USD              |
| totalCrvRevenueToCvxCrvStakers | BigDecimal                                                    | Cumulative CRV revenue distributed to CvxCrv Stakers in USD            |
| totalCvxRevenueToCvxCrvStakers | BigDecimal                                                    | Cumulative CVX revenue distributed to CvxCrv Stakers in USD            |
| totalFxsRevenueToCvxFxsStakers | BigDecimal                                                    | Cumulative FXS revenue distributed to CvxFxs Stakers in USD            |
| totalCrvRevenueToCvxStakers    | BigDecimal                                                    | Cumulative CRV revenue distributed to CVX stakers                      |
| totalFxsRevenueToCvxStakers    | BigDecimal                                                    | Cumulative FXS revenue distributed to CVX stakers                      |
| totalCrvRevenueToCallers       | BigDecimal                                                    | Cumulative CRV revenue distributed as caller incentives (for harvests) |
| totalFxsRevenueToCallers       | BigDecimal                                                    | Cumulative FXS revenue distributed as caller incentives (for harvests) |
| totalCrvRevenueToCallers       | BigDecimal                                                    | Cumulative CRV revenue going to treasury                               |
| totalFxsRevenueToCallers       | BigDecimal                                                    | Cumulative FXS revenue going to treasury                               |
| totalCrvRevenueToPlatform      | BigDecimal                                                    | Cumulative CRV revenue going to treasury                               |
| totalFxsRevenueToPlatform      | BigDecimal                                                    | Cumulative FXS revenue going to treasury                               |
| totalCrvRevenue                | BigDecimal                                                    | Total cumulative revenue from CRV                                      |
| totalFxsRevenue                | BigDecimal                                                    | Total cumulative revenue from FXS                                      |
| totalBribeRevenue              | BigDecimal                                                    | Cumulative revenue accrued from Votium bribes                          |
