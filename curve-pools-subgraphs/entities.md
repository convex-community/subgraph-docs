# Entities

## Pool

General pool metadata. Variable metrics such as TVL and Volumes are stored in separate snapshot entities.

| Field                    | Type                                                              | Description                                                   |
| ------------------------ | ----------------------------------------------------------------- | ------------------------------------------------------------- |
| id                       | ID                                                                | Entity ID                                                     |
| address                  | Bytes                                                             | Pool contract address                                         |
| platform                 | [Platform](entities.md#platform)                                  | Platform                                                      |
| name                     | String                                                            | Pool name (from registry from LP token)                       |
| symbol                   | String                                                            | LP token symbol                                               |
| metapool                 | Boolean                                                           | Whether the pool is a metapool or a plain pool                |
| lpToken                  | Bytes                                                             | LP Token contract address                                     |
| basePool                 | Bytes                                                             | Base pool (for metapools)                                     |
| coins                    | \[Bytes]                                                          | Pool coins                                                    |
| coinDecimals             | \[BigInt]                                                         | Pool coins decimals (same order as coins)                     |
| coinNames                | \[String]                                                         | Pool coins names (same order as coins)                        |
| assetType                | Int                                                               | Pool asset type (USD: 0, ETH: 1, BTC: 2, Other: 3, Crypto: 4) |
| poolType                 | PoolType                                                          | Pool type (for ABI variations)                                |
| c128                     | Boolean                                                           | Whether the pool uses int128 for coins (legacy contracts)     |
| isV2                     | Boolean                                                           | Whether the pool is a Curve v2 pool                           |
| isRebasing               | Boolean                                                           | Whether one of the pool's assets is rebasing                  |
| cumulativeVolume         | BigDecimal                                                        | Pool cumulative volume since inception                        |
| cumulativeVolumeUSD      | BigDecimal                                                        | Pool cumulative volume since inception (in USD)               |
| cumulativeFeesUSD        | BigDecimal                                                        | Pool cumulative fees since inception (in USD)                 |
| virtualPrice             | BigDecimal                                                        | Latest virtual price                                          |
| baseApr                  | BigDecimal                                                        | Latest base APR (from trading fees)                           |
| creationDate             | BigInt                                                            | Pool creation date                                            |
| creationTx               | Bytes                                                             | Pool creation transaction hash                                |
| creationBlock            | BigInt                                                            | Pool creation block                                           |
| dailyPoolSnapshots       | \[[DailyPoolSnapshot](entities.md#dailypoolsnapshot)]             | Daily pool attribute snapshot                                 |
| candles                  | \[[Candle](entities.md#candle)]                                   | Pool OHLC candle data                                         |
| prices                   | \[[PriceFeed](entities.md#pricefeed)]                             | Pool prices from executed trades (internal use)               |
| swapEvents               | \[[SwapEvent](entities.md#swapevent)]                             | Pool swaps                                                    |
| swapVolumeSnapshots      | \[[SwapVolumeSnapshot](entities.md#swapvolumesnapshot)]           | Pool swap volume snapshots                                    |
| liquidityEvents          | \[[LiquidityEvent](entities.md#liquidityevent)]                   | Pool liquidity events (currently disabled)                    |
| liquidityVolumeSnapshots | \[[LiquidityVolumeSnapshot](entities.md#liquidityvolumesnapshot)] | Pool liquidity volume snapshots (currently disabled)          |

## DailyPoolSnapshot

Snapshot of various pool metrics taken every 24 hours.&#x20;

| Field               | Type          | Description                                                                            |
| ------------------- | ------------- | -------------------------------------------------------------------------------------- |
| id                  | ID            | Entity ID                                                                              |
| pool                | Pool          | Pool ID                                                                                |
| virtualPrice        | BigDecimal    | Pool virtual price                                                                     |
| lpPriceUSD          | BigDecimal    | Pool LP token price (in USD)                                                           |
| tvl                 | BigDecimal    | Pool TVL (in USD)                                                                      |
| fee                 | BigDecimal    | Pool fee value                                                                         |
| adminFee            | BigDecimal    | Pool admin fee value                                                                   |
| offPegFeeMultiplier | BigDecimal    | Off peg fee multiplier (for lending pools)                                             |
| adminFeesUSD        | BigDecimal    | Value of fees paid to the DAO (in USD)                                                 |
| lpFeesUSD           | BigDecimal    | Value of fees paid to the LPs (in USD)                                                 |
| totalDailyFeesUSD   | BigDecimal    | Total value of fees collected (in USD)                                                 |
| reserves            | \[BigInt]     | Total reserves (asset and asset decimals denomination, same order as the pool's coins) |
| normalizedReserves  | \[BigInt]     | Total reserves (asset denomination, 18 decimals, same order as the pool's coins)       |
| reservesUSD         | \[BigDecimal] | Total reserves (USD denomination, same order as the pool's coins)                      |
| A                   | BigInt        | A factor                                                                               |
| xcpProfit           | BigDecimal    | xcpProfit value                                                                        |
| xcpProfitA          | BigDecimal    | xcpProfitA value                                                                       |
| baseApr             | BigDecimal    | base APR from fees (annualized from daily variation)                                   |
| rebaseApr           | BigDecimal    | APR from rebase - if applicable (annualized from daily variation)                      |
| timestamp           | BigInt        | Snapshot timestamp                                                                     |

## SwapVolumeSnapshot

Snapshot of swap volume taken every 24 hours. Volume in token denomination is only meaningful for stable swap pools. Volume figures do not take into account liquidity additions and removals.

| Field           | Type       | Description                                                      |
| --------------- | ---------- | ---------------------------------------------------------------- |
| id              | ID         | Entity ID                                                        |
| pool            | Pool       | Pool ID                                                          |
| period          | BigInt     | Snapshot period- hourly (3600), daily (86400) or weekly (604800) |
| amountSold      | BigDecimal | Amount sold (token denomination, normalized decimals)            |
| amountBought    | BigDecimal | Amount bought (token denomination, normalized decimals)          |
| amountSoldUSD   | BigDecimal | Amount sold (USD denomination, normalized decimals)              |
| amountBoughtUSD | BigDecimal | Amount bought (USD denomination, normalized decimals)            |
| count           | BigInt     | Number of swaps over the period                                  |
| volume          | BigDecimal | Volume (token denomination)                                      |
| volumeUSD       | BigDecimal | Volume (USD denomination)                                        |
| timestamp       | BigInt     | Snapshot timestamp                                               |

## DailyPlatformSnapshot

Daily snapshots containing aggregated metrics for all pool across the platform on that specific chain.

| Field             | Type       | Description                            |
| ----------------- | ---------- | -------------------------------------- |
| id                | ID         | Entity ID                              |
| adminFeesUSD      | BigDecimal | Total admin fees collected (in USD)    |
| lpFeesUSD         | BigDecimal | Total fees distributed to LPs (in USD) |
| totalDailyFeesUSD | BigDecimal | Total fees collected (in USD)          |
| timestamp         | BigInt     | Snapshot timestamp                     |

## SwapEvent

Every swap event generated by a pool is recorded with this entity. Also includes basic metadata about the transaction.&#x20;

| Field           | Type       | Description                      |
| --------------- | ---------- | -------------------------------- |
| pool            | Pool       | Pool ID                          |
| block           | BigInt     | Even block                       |
| tx              | Bytes      | Event transaction hash           |
| gasLimit        | BigInt     | Transaction gas limit            |
| gasUsed         | BigInt     | Transaction gas used             |
| buyer           | Bytes      | Buyer address                    |
| tokenSold       | Bytes      | Sold token address               |
| tokenBought     | Bytes      | Bought token address             |
| amountSold      | BigDecimal | Amount sold                      |
| amountBought    | BigDecimal | Amount bought                    |
| amountSoldUSD   | BigDecimal | Amount sold (USD denomination)   |
| amountBoughtUSD | BigDecimal | Amount bought (USD denomination) |
| timestamp       | BigInt     | Snapshot timestamp               |

## Candle

Price data to generate [candlestick charts](https://www.investopedia.com/terms/o/ohlcchart.asp). Available periodicity is hourly, daily and weekly.



| Field             | Type       | Description                                                    |
| ----------------- | ---------- | -------------------------------------------------------------- |
| id                | ID         | Entity ID (time + period + srcToken + dstToken)                |
| pool              | Pool       | Pool ID                                                        |
| timestamp         | BigInt     | Candle timestamp                                               |
| period            | Int        | Candle period- hourly (3600), daily (86400) or weekly (604800) |
| lastBlock         | BigInt     | Last block included in the candle                              |
| token0            | Bytes      | First asset address                                            |
| token1            | Bytes      | Second asset address                                           |
| txs               | BigInt     | Number of transactions                                         |
| token0TotalAmount | BigDecimal | First asset volume                                             |
| token1TotalAmount | BigDecimal | Second asset volume                                            |
| open              | BigDecimal | Open price                                                     |
| close             | BigDecimal | Close price                                                    |
| low               | BigDecimal | Lowest price                                                   |
| high              | BigDecimal | Highest price                                                  |

## LiquidityVolumeSnapshot

This entity is currently unavailable. Tracks volume from liquidity additions and removals.

| Field         | Type          | Description                                                        |
| ------------- | ------------- | ------------------------------------------------------------------ |
| id            | ID            | Entity ID                                                          |
| pool          | Pool          | Pool ID                                                            |
| period        | BigInt        | Snapshot period- hourly (3600), daily (86400) or weekly (604800)   |
| amountAdded   | \[BigDecimal] | Amounts of assets added to LP (same order as the pool's coins)     |
| amountRemoved | \[BigDecimal] | Amounts of assets removed from LP (same order as the pool's coins) |
| addCount      | BigInt        | Number of liquidity additions                                      |
| removeCount   | BigInt        | Number of liquidity removals                                       |
| volumeUSD     | BigDecimal    | Liquidity based volume (USD denomination)                          |

## LiquidityEvent

This entity is currently unavailable. Tracks all events generated by a pool when liquidity is added or removed.

| Field             | Type      | Description                                               |
| ----------------- | --------- | --------------------------------------------------------- |
| id                | ID        | Entity ID                                                 |
| liquidityProvider | Bytes     | Address of the liquidity provider                         |
| timestamp         | BigInt    | Event timestamp                                           |
| block             | BigInt    | Event block                                               |
| pool              | Pool      | Pool ID                                                   |
| tokenAmounts      | \[BigInt] | Amounts added or removed (same order as the pool's coins) |
| removal           | Boolean   | Whether the event is a liquidity removal or not           |

## PriceFeed

Internal use entity. This tracks the realized price of a pool's assets from the most recent swap event.&#x20;

| Field        | Type       | Description                            |
| ------------ | ---------- | -------------------------------------- |
| id           | ID         | Entity ID (pool + srcToken + dstToken) |
| pool         | Pool       | Pool ID                                |
| lastUpdated  | BigInt     | Last update timestamp                  |
| lastBlock    | BigInt     | Last update block                      |
| fromIndex    | Int        | From token index                       |
| toIndex      | Int        | To token index                         |
| isUnderlying | Boolean    | Was exchange underlying                |
| token0       | Bytes      | First asset address                    |
| token1       | Bytes      | Second asset address                   |
| price        | BigDecimal | Realized price                         |

## TokenSnapshot

Internal use entity. Price snapshots. The pricing of tokens in USD is done based on on-chain DEX or Oracle prices (Uniswap v2, v3, Sushi and Uniswap forks, Curve, Chainlink). Consequently some USD price data may be missing or incorrect for tokens with no liquidity pools outside of Curve.&#x20;

| Field     | Type       | Description        |
| --------- | ---------- | ------------------ |
| id        | ID         | Entity ID          |
| token     | Bytes      | Token address      |
| price     | BigDecimal | Token price        |
| timestamp | BigInt     | Snapshot timestamp |

## BasePool

Internal use entity. Keeps track of base pools linked to metapools.&#x20;

| Field        | Type      | Description             |
| ------------ | --------- | ----------------------- |
| id           | ID        | Entity ID               |
| coins        | \[Bytes]  | Base pool coins         |
| coinDecimals | \[BigInt] | Base pool coin decimals |

## Platform

Internal use entity. Keeps track of all pools.

| Field              | Type     | Description                            |
| ------------------ | -------- | -------------------------------------- |
| id                 | ID       | Entity ID                              |
| pools              | \[Pool]  | List of all pools                      |
| poolAddresses      | \[Bytes] | Addresses of all pools on the platform |
| latestPoolSnapshot | BigInt   | Timestamp of the latest snapshot       |

## Registry

Internal use entity. Keeps track of all registries.

| Field | Type | Description                  |
| ----- | ---- | ---------------------------- |
| id    | ID   | Entity ID (Registry address) |

## Factory

Internal use entity. Keeps track of all factories.



| Field     | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| id        | ID     | Entity ID (Factory address)                      |
| poolCount | BigInt | Number of pools deployed by the Factory contract |
