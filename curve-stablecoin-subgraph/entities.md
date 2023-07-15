# Entities

### Market

The "Market" entity represents a market in the crvUSD ecosystem (corresponding to a "Controller" contract), recording information about the collateral, LLAMMA, monetary policy, as well as tracking events such as borrows, repayments, liquidations, and fee collections within this market.

| Field               | Type            | Description                                                                         |
| ------------------- | --------------- | ----------------------------------------------------------------------------------- |
| id                  | Bytes           | Entity ID                                                                           |
| collateral          | Bytes           | The address of the collateral token used in this market                             |
| collateralPrecision | BigInt          | The number of decimals used by the collateral token                                 |
| collateralName      | String          | The name of the collateral used, obtained from the corresponding ERC20 contract     |
| controller          | Bytes           | The address of the controller contract for this market                              |
| amm                 | Amm             | Address of llamma contract associated with this market                              |
| monetaryPolicy      | MonetaryPolicy  | Address of monetary policy contract                                                 |
| index               | BigInt          | The index of the market in the controller factory                                   |
| blockNumber         | BigInt          | The block number where this market contract was deployed                            |
| blockTimestamp      | BigInt          | The timestamp of the block where the market contract was deployed                   |
| transactionHash     | Bytes           | The hash of the transaction deploying the market contract                           |
| borrows             | \[Borrow]       | The list of borrow events occurring in this market                                  |
| repayments          | \[Repayment]    | The list of repayment events occurring in this market                               |
| removals            | \[Removal]      | The list of collateral removal events occurring in this market                      |
| liquidations        | \[Liquidation]  | The list of liquidation events occurring in this market                             |
| snapshots           | \[Snapshot]     | The list of snapshots recording the state of the market at different points in time |
| collectedFees       | \[CollectedFee] | The list of events capturing the collection of fees in this market                  |

### Snapshot

\
The `Snapshot` entity records the state of a `Market` at a specific block, including key data like orrow rate, liquidation discount, and total crvUSD supply. The snapshots are taken on an hourly basis, except for band snapshots which are taken daily.

| Field               | Type           | Description                                                                                                                                                 |
| ------------------- | -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                  | String         | Entity ID                                                                                                                                                   |
| market              | Market         | The market for which this snapshot was taken                                                                                                                |
| llamma              | Amm            | Llamma for the market snapshotted                                                                                                                           |
| policy              | MonetaryPolicy | Monetary policy for the market snapshotted                                                                                                                  |
| A                   | BigInt         | Amplification coefficient                                                                                                                                   |
| rate                | BigDecimal     | Market borrow rate (annualized)                                                                                                                             |
| futureRate          | BigDecimal     | The future policy rate of the market, annualized, at the time this snapshot was taken                                                                       |
| liquidationDiscount | BigDecimal     | The liquidation discount applied in the market at the time this snapshot was taken                                                                          |
| loanDiscount        | BigDecimal     | The loan discount applied in the market at the time this snapshot was taken                                                                                 |
| minted              | BigDecimal     | The total amount of crvUSD minted in the market up until the time this snapshot was taken                                                                   |
| redeemed            | BigDecimal     | The total amount of debt redeemed in the market up until the time this snapshot was taken                                                                   |
| totalKeeperDebt     | BigDecimal     | The total debt of all Keepers globally, not specific to this market, at the time this snapshot was taken                                                    |
| totalCollateral     | BigDecimal     | The controller's collateral balance (token balance - collateral denominated admin fees) at the time this snapshot was taken                                 |
| totalCollateralUsd  | BigDecimal     | The USD value of the controller's collateral balance at the time this snapshot was taken                                                                    |
| totalSupply         | BigDecimal     | The total crvUSD supply for this market (global crvUSD supply is the sum of this for each market + total keepers' debt) at the time this snapshot was taken |
| totalStableCoin     | BigDecimal     | The controller's stablecoin balance (crvUSD balance - crvUSD denominated admin fees) at the time this snapshot was taken                                    |
| totalDebt           | BigDecimal     | The total amount of debt in the market at the time this snapshot was taken                                                                                  |
| nLoans              | BigInt         | Number of outstanding loans on the market                                                                                                                   |
| crvUsdAdminFees     | BigDecimal     | crvUSD denominated fees from Llamma waiting to be collected                                                                                                 |
| collateralAdminFees | BigDecimal     | Collateral denominated fees from Llamma waiting to be collected                                                                                             |
| adminBorrowingFees  | BigDecimal     | Borrowing fees                                                                                                                                              |
| fee                 | BigDecimal     | Llamma fee                                                                                                                                                  |
| adminFee            | BigDecimal     | Llamma admin fee                                                                                                                                            |
| ammPrice            | BigDecimal     | Llamma collateral market price                                                                                                                              |
| oraclePrice         | BigDecimal     | Llamma oracle price                                                                                                                                         |
| basePrice           | BigDecimal     | Llamma brice                                                                                                                                                |
| activeBand          | BigInt         | Active band at snapshot time                                                                                                                                |
| minBand             | BigInt         | Min band at snapshot time                                                                                                                                   |
| maxBand             | BigInt         | Max band at snapshot time                                                                                                                                   |
| bandSnapshot        | Boolean        | Whether the (hourly) snapshot includes a (daily) band snapshot                                                                                              |
| bands               | \[Band]        | State of all bands at snapshot time                                                                                                                         |
| blockNumber         | BigInt         | Snapshot block                                                                                                                                              |
| timestamp           | BigInt         | Timestamp of the snapshot's block                                                                                                                           |

### CollectedFee

The `CollectedFee` entity records the details of fees collected from a `Market`, including borrowing fees and fees collected from the Llamma in USD and collateral terms.

| Field                | Type       | Description                                                 |
| -------------------- | ---------- | ----------------------------------------------------------- |
| id                   | Bytes      | Entity ID                                                   |
| market               | Market     | Specifies the market from which the fees were collected     |
| borrowingFees        | BigDecimal | The total borrowing fees collected in terms of USD          |
| ammCollateralFees    | BigDecimal | Collateral denominated fees collected from the Llamma       |
| ammCollateralFeesUsd | BigDecimal | USD denominated collateral fees collected from the Llamma   |
| ammBorrowingFees     | BigDecimal | USD (stablecoin) fees collected from the Llamma             |
| blockNumber          | BigInt     | The block number at which the fee collection event occurred |
| blockTimestamp       | BigInt     | Block timestamp                                             |
| transactionHash      | Bytes      | Transaction Hash                                            |

### Band

The `Band` entity captures the state of a market band at a snapshot time, detailing stablecoin and collateral amounts, collateral USD value, and upper and lower price ranges.

| Field           | Type       | Description                                                          |
| --------------- | ---------- | -------------------------------------------------------------------- |
| id              | String     | Entity ID                                                            |
| index           | BigInt     | Band index                                                           |
| snapshot        | Snapshot   | Market snapshot where the band snapshot was taken                    |
| stableCoin      | BigDecimal | bands\_x - The amount of stablecoin in the band at the snapshot time |
| collateral      | BigDecimal | bands\_y - The amount of collateral in the band at the snapshot time |
| collateralUsd   | BigDecimal | The USD value of the collateral in the band at the snapshot time     |
| priceOracleUp   | BigDecimal | Band higher price range                                              |
| priceOracleDown | BigDecimal | Band lower price range                                               |

### Mint

The `Mint` entity represents an event where stablecoin is minted, recording the receiving controller's address, the minted amount, the block number, timestamp, and transaction hash of the minting event.

| Field           | Type   | Description                                                       |
| --------------- | ------ | ----------------------------------------------------------------- |
| id              | Bytes  | Entity ID                                                         |
| addr            | Bytes  | The address of the controller that received the minted stablecoin |
| amount          | BigInt | Amount of stablecoin minted                                       |
| blockNumber     | BigInt | Block at which the mint happened                                  |
| blockTimestamp  | BigInt | Block timestamp                                                   |
| transactionHash | Bytes  | Transaction hash                                                  |

### Burn

The `Burn` entity denotes a burn event, detailing the address of the controller for which the burn occurred, the amount burned, the block number, timestamp, and transaction hash of the burn event.

| Field           | Type   | Description                                                  |
| --------------- | ------ | ------------------------------------------------------------ |
| id              | Bytes  | Entity ID                                                    |
| addr            | Bytes  | Address of the controller for which the stablecoin is burned |
| amount          | BigInt | Amount burned/removed                                        |
| blockNumber     | BigInt | Block at which the mint happened                             |
| blockTimestamp  | BigInt | Block timestamp                                              |
| transactionHash | Bytes  | Transaction hash                                             |

### Amm

The `Amm` entity represents a LLAMMA associated to a specific market, including details about the traded tokens, fees, and historical records related to trading volume, rates, fees, and liquidity operations.

| Field           | Type                | Description                                                       |
| --------------- | ------------------- | ----------------------------------------------------------------- |
| id              | Bytes               | Entity ID                                                         |
| market          | Market              | Market/Controller the Llamma is associated with                   |
| A               | BigInt              | Amplification parameter (A) that dictates the trading curve       |
| coins           | \[Bytes]            | Stores the addresses of the traded coins                          |
| coinNames       | \[String]           | Stores the names of the traded coins                              |
| coinDecimals    | \[BigInt]           | Stores the decimals of the traded coins for precision             |
| basePrice       | BigInt              | The price of the collateral at the time of market launch          |
| priceOracle     | Bytes               | Oracle contract address                                           |
| fee             | BigInt              | Amm fee                                                           |
| adminFee        | BigInt              | Amm admin fee                                                     |
| totalSwapVolume | BigDecimal          | The total trading volume accumulated over the lifetime of the Amm |
| volumeSnapshots | \[VolumeSnapshot]   | Historical snapshots of the Amm trading volume                    |
| rates           | \[LlammaRate]       | Historical records of the Llamma's rates                          |
| fees            | \[LlammaFee]        | Historical records of the Llamma's fees                           |
| exchanges       | \[TokenExchange]    | Records of all trades executed via the Llamma                     |
| withdrawals     | \[LlammaWithdrawal] | Records of all liquidity withdrawals from the Llamma              |
| deposits        | \[LlammaDeposit]    | Records of all liquidity deposits to the Llamma                   |

### VolumeSnapshot

The `VolumeSnapshot` entity records a period (hourly and daily) snapshot of the AMM's trading volume, detailing the total value of tokens sold, bought, deposited, withdrawn in USD, the total number of swaps, the duration of the snapshot period, and the timestamps

| Field              | Type       | Description                                                            |
| ------------------ | ---------- | ---------------------------------------------------------------------- |
| id                 | ID         | Entity ID                                                              |
| llamma             | Amm        | Llamma the snapshot is for                                             |
| amountSoldUSD      | BigDecimal | Total value of tokens sold in USD during the snapshot period           |
| amountBoughtUSD    | BigDecimal | Total value of tokens bought in USD during the snapshot period         |
| swapVolumeUSD      | BigDecimal | Swap volume (as average of sold & bought amounts) in USD               |
| amountDepositedUSD | BigDecimal | Total value of liquidity deposits in USD during the snapshot period    |
| amountWithdrawnUSD | BigDecimal | Total value of liquidity withdrawals in USD during the snapshot period |
| count              | BigInt     | Total number of swaps that occurred during the snapshot period         |
| period             | BigInt     | Duration of the snapshot period in seconds                             |
| roundedTimestamp   | BigInt     | Timestamp the snapshot was initiated at, rounded to period             |
| timestamp          | BigInt     | Actual timestamp snapshot was initiated at                             |

### LlammaRate

The `LlammaRate` entity records a change in rate for a LLAMMA, detailing the new rate, the rate multiplier, the block number and timestamp of the change, and the transaction hash.

| Field           | Type   | Description                               |
| --------------- | ------ | ----------------------------------------- |
| id              | Bytes  | Entity ID                                 |
| llamma          | Amm    | Llamma whose rate was changed             |
| rate            | BigInt | New rate                                  |
| rateMul         | BigInt | Rate multiplier, 1 + integral(rate \* dt) |
| blockNumber     | BigInt | Block number at which rate was changed    |
| blockTimestamp  | BigInt | Block timestamp                           |
| transactionHash | Bytes  | Transaction hash                          |

### LlammaFee

The `LlammaFee` entity logs adjustments to fees for a specific LLAMMA, including both the admin fee and the general fee. It provides the block number, timestamp, and transaction hash of the event.

| Field           | Type   | Description                           |
| --------------- | ------ | ------------------------------------- |
| id              | Bytes  | Entity ID                             |
| llamma          | Amm    | Llamma whose rate was changed         |
| adminFee        | BigInt | New admin fee                         |
| fee             | BigInt | New fee                               |
| blockNumber     | BigInt | Block number at which fee was changed |
| blockTimestamp  | BigInt | Block timestamp                       |
| transactionHash | Bytes  | Transaction hash                      |

### LlammaDeposit

The `LlammaDeposit` entity records instances of users depositing into a specific LLAMMA, capturing deposit amount, band range, and event metadata like block number, timestamp, and transaction hash.

| Field           | Type   | Description                        |
| --------------- | ------ | ---------------------------------- |
| id              | Bytes  | Entity ID                          |
| llamma          | Amm    | Llamma where deposit occured       |
| provider        | User   | Depositing user address            |
| amount          | BigInt | Amount deposited                   |
| n1              | BigInt | Lower band in the deposit range    |
| n2              | BigInt | Upper band in the deposit range    |
| blockNumber     | BigInt | Block at which the deposit occured |
| blockTimestamp  | BigInt | Block timestamp                    |
| transactionHash | Bytes  | Transaction hash                   |

### LlammaWithdrawal

The `LlammaWithdrawal` entity records instances of withdrawals from a specific LLAMMA. It captures the withdrawing user address, amounts of stablecoin and collateral withdrawn, and metadata like block number, timestamp, and transaction hash.

| Field            | Type   | Description                           |
| ---------------- | ------ | ------------------------------------- |
| id               | Bytes  | Entity ID                             |
| llamma           | Amm    | Llamma where withdrawal occured       |
| provider         | User   | Withdrawing user address              |
| amountBorrowed   | BigInt | Amount of stablecoin withdrawn        |
| amountCollateral | BigInt | Amount of collateral withdrawn        |
| blockNumber      | BigInt | Block at which the withdrawal occured |
| blockTimestamp   | BigInt | Block timestamp                       |
| transactionHash  | Bytes  | Transaction hash                      |

### TokenExchange

The `TokenExchange` entity tracks token exchange events in a specific LLAMMA. It captures details such as the buyer's address, indices of tokens sold and bought, their quantities and values in USD, and metadata like block number, timestamp, and transaction hash.

| Field           | Type       | Description                             |
| --------------- | ---------- | --------------------------------------- |
| id              | Bytes      | Entity ID                               |
| buyer           | User       | Buyer address                           |
| llamma          | Amm        | Llamma where exchange occured           |
| soldId          | BigInt     | Index of token sold                     |
| tokensSold      | BigInt     | Amount of token sold (token decimals)   |
| tokensSoldUSD   | BigDecimal | USD value of tokens sold                |
| boughtId        | BigInt     | Index of token bought                   |
| tokensBought    | BigInt     | Amount of token bought (token decimals) |
| tokensBoughtUSD | BigDecimal | USD value of tokens bought              |
| blockNumber     | BigInt     | Block at which the exchange occured     |
| blockTimestamp  | BigInt     | Block timestamp                         |
| transactionHash | Bytes      | Transaction hash                        |

### MonetaryPolicy

The `MonetaryPolicy` entity encapsulates the monetary policy parameters associated with a particular market. It includes details like the address of the price oracle, list of keepers addresses, as well as historical policy rates, benchmark rates, and debt fractions.

| Field          | Type               | Description                                          |
| -------------- | ------------------ | ---------------------------------------------------- |
| id             | Bytes              | Entity ID                                            |
| priceOracle    | Bytes              | Address of price oracle used by the policy           |
| keepers        | \[Bytes]           | List of keepers addresses (used for internal loops)  |
| pegKeepers     | \[PolicyPegKeeper] | Peg Keepers associated with this monetary policy     |
| rates          | \[PolicyRate]      | Relationship to rates determined by policy           |
| benchmarkRates | \[BenchmarkRate]   | Relationship to benchmark rates determined by policy |
| debtFractions  | \[DebtFraction]    | Relationship to debt fractions determined by policy  |

### PolicyPegKeeper

Internally used entity to create a many-to-many relationship between peg keepers and monetary policies.

| Field     | Type           | Description                                  |
| --------- | -------------- | -------------------------------------------- |
| id        | Bytes          | Entity ID (Policy ID + PegKeeper ID)         |
| pegKeeper | PegKeeper      | PegKeeper used for many to many relationship |
| policy    | MonetaryPolicy | Policy used for many to many relationship    |

### BenchmarkRate

The `BenchmarkRate` entity tracks historical benchmark rate change events for a specific monetary policy.&#x20;

| Field           | Type           | Description                     |
| --------------- | -------------- | ------------------------------- |
| id              | Bytes          | Entity ID                       |
| policy          | MonetaryPolicy | Monetary policy for the rate    |
| rate            | BigInt         | Rate value                      |
| blockNumber     | BigInt         | Block at which the rate was set |
| blockTimestamp  | BigInt         | Block timestamp                 |
| transactionHash | Bytes          | Transaction hash                |

### DebtFraction

The `DebtFraction` entity tracks historical changes to the target debt fraction for a specific monetary policy.

| Field           | Type           | Description                              |
| --------------- | -------------- | ---------------------------------------- |
| id              | Bytes          | Entity ID                                |
| policy          | MonetaryPolicy | Monetary policy for the debt fraction    |
| target          | BigInt         | Fraction value                           |
| blockNumber     | BigInt         | Block at which the debt fraction was set |
| blockTimestamp  | BigInt         | Block timestamp                          |
| transactionHash | Bytes          | Transaction hash                         |

### PolicyRate

The `PolicyRate` entity tracks adjustements to the policy rate of a specific monetary policy.&#x20;

| Field           | Type           | Description                     |
| --------------- | -------------- | ------------------------------- |
| id              | Bytes          | Entity ID                       |
| policy          | MonetaryPolicy | Monetary policy for the rate    |
| rate            | BigInt         | Rate value                      |
| blockNumber     | BigInt         | Block at which the rate was set |
| blockTimestamp  | BigInt         | Block timestamp                 |
| transactionHash | Bytes          | Transaction hash                |

### PegKeeper

The `PegKeeper` entity represents a peg keeper contract which are used for maintaining the peg of the crvUSD stablecoin. The entity includes a list of associated monetary policies, an activity status, associated stablecoin pool details, debt amount, total coins provided and withdrawn, and the keeper's total profit. It also has relationships to events related to providing, withdrawing, and profit withdrawal in the pool.

| Field          | Type               | Description                                                      |
| -------------- | ------------------ | ---------------------------------------------------------------- |
| id             | Bytes              | Entity ID                                                        |
| policies       | \[PolicyPegKeeper] | Monetary policies associated with the keeper                     |
| active         | Boolean            | True if the keeper is still active, False if it has been removed |
| pool           | Bytes              | CrvUSD pool that the keeper is associated with                   |
| debt           | BigInt             | Amount of debt for that particular keeper                        |
| totalProvided  | BigInt             | Amount of coins provided to the pool                             |
| totalWithdrawn | BigInt             | Amount of coins withdrawn from the pool                          |
| totalProfit    | BigInt             | Keeper's profit                                                  |
| provides       | \[Provide]         | Relationship to all events for providing in the pool             |
| withdrawals    | \[Withdraw]        | Relationship to all events for withdrawing from the pool         |
| profits        | \[Profit]          | Relationship to all profit withdrawal events                     |

### Provide

The `Provide` entity represents a deposit event performed by a PegKeeper.

| Field           | Type      | Description                         |
| --------------- | --------- | ----------------------------------- |
| id              | Bytes     | Entity ID                           |
| keeper          | PegKeeper | Keeper that initiated the deposit   |
| amount          | BigInt    | Amount provided                     |
| debt            | BigInt    | Debt amount after deposit           |
| blockNumber     | BigInt    | Block at which the deposit occurred |
| blockTimestamp  | BigInt    | Block timestamp                     |
| transactionHash | Bytes     | Transaction hash                    |

### Withdraw

The `Withdraw` entity is a record of a withdrawal event performed by a PegKeeper.

| Field           | Type      | Description                            |
| --------------- | --------- | -------------------------------------- |
| id              | Bytes     | Entity ID                              |
| keeper          | PegKeeper | Keeper that initiated the withdrawal   |
| amount          | BigInt    | Amount withdrawn                       |
| debt            | BigInt    | Debt amount after withdrawal           |
| blockNumber     | BigInt    | Block at which the withdrawal occurred |
| blockTimestamp  | BigInt    | Block timestamp                        |
| transactionHash | Bytes     | Transaction hash                       |

### Profit

The `Profit` entity records the event of a PegKeeper claiming profits.

| Field           | Type      | Description                            |
| --------------- | --------- | -------------------------------------- |
| id              | Bytes     | Entity ID                              |
| keeper          | PegKeeper | Keeper from which profits were claimed |
| amount          | BigInt    | Amount of profits claimed              |
| blockNumber     | BigInt    | Block at which the profit was claimed  |
| blockTimestamp  | BigInt    | Block timestamp                        |
| transactionHash | Bytes     | Transaction hash                       |

### DebtCeiling

The `DebtCeiling` entity represents an update to the debt ceiling of a specific controller.

| Field           | Type   | Description                                 |
| --------------- | ------ | ------------------------------------------- |
| id              | Bytes  | Entity ID                                   |
| addr            | Bytes  | Controller affected by new ceiling          |
| debtCeiling     | BigInt | New debt ceiling value                      |
| blockNumber     | BigInt | Block at which the debt ceiling was updated |
| blockTimestamp  | BigInt | Block timestamp                             |
| transactionHash | Bytes  | Transaction hash                            |

### Borrow

The `Borrow` entity represents a borrowing event in a specific market.

| Field              | Type   | Description                             |
| ------------------ | ------ | --------------------------------------- |
| id                 | Bytes  | Entity ID                               |
| user               | User   | Borrower address                        |
| market             | Market | Market/controller where borrow happened |
| collateralIncrease | BigInt | Amount of collateral increase           |
| loanIncrease       | BigInt | Amount of loan increase                 |
| blockNumber        | BigInt | Block at which borrow occured           |
| blockTimestamp     | BigInt | Block timestamp                         |
| transactionHash    | Bytes  | Transaction hash                        |

### Repayment

The `Repayment` entity records the event of a user making a repayment in a specific market.

| Field              | Type   | Description                                             |
| ------------------ | ------ | ------------------------------------------------------- |
| id                 | Bytes  | Entity ID                                               |
| user               | User   | The user who made the repayment                         |
| market             | Market | The market/controller where the repayment was made      |
| collateralDecrease | BigInt | The decrease in collateral as a result of the repayment |
| loanDecrease       | BigInt | The decrease in loan as a result of the repayment       |
| blockNumber        | BigInt | The block number at which the repayment occurred        |
| blockTimestamp     | BigInt | Block timestamp                                         |
| transactionHash    | Bytes  | Transaction hash                                        |

### Removal

The `Removal` entity represents a user removing collateral from a specific market.

| Field              | Type   | Description                                                 |
| ------------------ | ------ | ----------------------------------------------------------- |
| id                 | Bytes  | Entity ID                                                   |
| user               | User   | The user who removed the collateral                         |
| market             | Market | The market/controller from which the collateral was removed |
| collateralDecrease | BigInt | The amount of collateral removed by the user                |
| blockNumber        | BigInt | The block number at which the removal occurred              |
| blockTimestamp     | BigInt | Block timestamp                                             |
| transactionHash    | Bytes  | Transaction hash                                            |

### Liquidation

The `Liquidation` entity records a liquidation event in a specific market.

| Field              | Type   | Description                                          |
| ------------------ | ------ | ---------------------------------------------------- |
| id                 | Bytes  | Entity ID                                            |
| user               | User   | The user whose debt was liquidated                   |
| liquidator         | User   | The user who performed the liquidation               |
| market             | Market | The market/controller where the liquidation occurred |
| collateralReceived | BigInt | The amount of collateral received by the liquidator  |
| stablecoinReceived | BigInt | The amount of stablecoin received by the liquidator  |
| debt               | BigInt | The amount of the user's debt that was liquidated    |
| blockNumber        | BigInt | The block number at which the liquidation occurred   |
| blockTimestamp     | BigInt | Block timestamp                                      |
| transactionHash    | Bytes  | Transaction hash                                     |

### UserState

The `UserState` entity records the state of a user in a particular market at a given block. This can include data like the amount of collateral they've provided, the bounds of their borrow range (`n1` and `n2`), their total debt, the liquidation discount applicable to them, and the block number, timestamp, and transaction hash of the event that updated the user's state.

| Field               | Type   | Description                                            |
| ------------------- | ------ | ------------------------------------------------------ |
| id                  | Bytes  | Entity ID                                              |
| user                | User   | The user whose state is being represented              |
| market              | Market | The market/controller where the state change happened  |
| collateral          | BigInt | The amount of collateral the user has provided         |
| n1                  | BigInt | Lower band in the user's borrow range                  |
| n2                  | BigInt | Upper band in the user's borrow range                  |
| debt                | BigInt | Total debt amount of the user                          |
| liquidationDiscount | BigInt | Liquidation discount applicable to the user            |
| blockNumber         | BigInt | The block number at which the user's state was updated |
| blockTimestamp      | BigInt | Block timestamp                                        |
| transactionHash     | Bytes  | Transaction hash                                       |

### User

Used to track all of unique wallet address' actions in the system.

| Field                 | Type           | Description                                                         |
| --------------------- | -------------- | ------------------------------------------------------------------- |
| id                    | Bytes          | Entity ID                                                           |
| borrows               | \[Borrow]      | All borrow events associated with this user                         |
| repayments            | \[Repayment]   | All repayment events associated with this user                      |
| removals              | \[Removal]     | All removal events associated with this user                        |
| liquidations          | \[Liquidation] | All liquidation events where this user was liquidated               |
| initiatedLiquidations | \[Liquidation] | All liquidation events where this user was liquidating another user |
| states                | \[UserState]   | All user states associated with this user                           |

### Platform

Internal use entity for global variables

| Field          | Type     | Description                              |
| -------------- | -------- | ---------------------------------------- |
| id             | ID       | Platform ID                              |
| ammAddresses   | \[Bytes] | List of amm addresses for internal use   |
| latestSnapshot | BigInt   | Last snapshot timestamp for internal use |
