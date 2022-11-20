# Entities

### Platform

List of gauges and pools. General metrics regarding CRV, veCRV.

| Field          | Type                                        | Description                |
| -------------- | ------------------------------------------- | -------------------------- |
| id             | ID                                          | Entity ID                  |
| totalVeCrv     | BigInt                                      | Total supply of veCRV      |
| totalCrvLocked | BigInt                                      | Total amount of CRV locked |
| crvSupply      | BigInt                                      | Total supply of CRV        |
| poolCount      | BigInt                                      | Pool count (internal use)  |
| pools          | \[[Pool](entities.md#pool)]                 | All pools                  |
| gauges         | \[[Gauge](entities.md#gauge)]               | All gauges                 |
| gaugeIds       | \[String]                                   | Gauge IDs (internal use)   |
| snapshotTimes  | \[[SnapshotTime](entities.md#snapshottime)] | Snapshot times             |

### PowerSnapshot

Snapshots of veCrv and Crv supplies at each new lock.

| Field          | Type   | Description                |
| -------------- | ------ | -------------------------- |
| block          | BigInt | Entity ID                  |
| timestamp      | BigInt | Snapshot timestamp         |
| totalVeCrv     | BigInt | Total supply of veCRV      |
| totalCrvLocked | BigInt | Total amount of CRV locked |
| crvSupply      | BigInt | Total supply of CRV        |

### User

Tracks the votes and locks of a user

| Field            | Type                                              | Description             |
| ---------------- | ------------------------------------------------- | ----------------------- |
| id               | ID                                                | Entity ID               |
| balance          | [UserBalance](entities.md#userbalance)            | User balance            |
| locks            | \[[Lock](entities.md#lock)]                       | Locks                   |
| proposals        | \[[Proposal](entities.md#proposal)]               | Proposals posted        |
| votes            | \[[Vote](entities.md#vote)]                       | Votes participated in   |
| gaugeWeightVotes | \[[GaugeWeightVote](entities.md#gaugeweightvote)] | Gauge weight votes cast |

### UserBalance

Tracks the changes to a user's voting power and locking status

| Field      | Type                     | Description        |
| ---------- | ------------------------ | ------------------ |
| id         | ID                       | Entity ID          |
| startTx    | Bytes                    | First lock tx      |
| user       | [User](entities.md#user) | User address       |
| crvLocked  | BigInt                   | CRV locked by user |
| lockStart  | BigInt                   | Lock start date    |
| unlockTime | BigInt                   | Unlock date        |

### Lock

Details about each lock event

| Field               | Type                     | Description                                                                         |
| ------------------- | ------------------------ | ----------------------------------------------------------------------------------- |
| id                  | ID                       | Entity ID                                                                           |
| user                | [User](entities.md#user) | Address of the locking user                                                         |
| tx                  | Bytes                    | Transaction hash                                                                    |
| value               | BigInt                   | Amount of CRV locked                                                                |
| lockStart           | BigInt                   | Lock start date                                                                     |
| unlockTime          | BigInt                   | Unlock date                                                                         |
| previousUnlockTime  | BigInt                   | Previous unlock date (for relocks)                                                  |
| totalLocked         | BigInt                   | Total amount locked                                                                 |
| previousTotalLocked | BigInt                   | Total locked by user prior to this lock                                             |
| type                | LockType                 | Lock type- First lock, Increase lock time, Increase lock amount, Withdraw, Lock for |
| timestamp           | BigInt                   | Lock timestamp                                                                      |

### Proposal

Details about each proposal posted (both ownership and parameter votes). The subgraph will \*try\* to retrieve the proposal content from the IPFS, however that process is still nondeterministic and prone to failures. As such, there is a high likelihood that the decoded metadata will be missing. The proposal's call data needs to be decoded as well.

| Field              | Type                               | Description                                      |
| ------------------ | ---------------------------------- | ------------------------------------------------ |
| id                 | ID                                 | Entity ID                                        |
| tx                 | Bytes                              | Transaction hash                                 |
| voteId             | BigInt                             | Vote ID                                          |
| voteType           | VoteType                           | Vote type (Ownership or Parameter)               |
| creator            | [User](entities.md#user)           | User initiating the vote                         |
| startDate          | BigInt                             | Start date                                       |
| snapshotBlock      | BigInt                             | Block used for the snapshot                      |
| ipfsMetadata       | String                             | IPFS address of the proposal's metadata          |
| metadata           | String                             | Decoding of the proposal's metadata (Unreliable) |
| minBalance         | BigInt                             | Minimum balance required                         |
| minTime            | BigInt                             | Minimum amount of lock time                      |
| totalSupply        | BigInt                             | Total supply at moment of proposal               |
| creatorVotingPower | BigInt                             | Creator voting power                             |
| votesFor           | BigInt                             | Votes in favor (in voting power)                 |
| votesAgainst       | BigInt                             | Votes against (in voting power)                  |
| votes              | \[[Vote](entities.md#vote)]        | List of votes                                    |
| voteCount          | BigInt                             | Number of individual votes cast                  |
| supportRequired    | BigInt                             | Support required                                 |
| minAcceptQuorum    | BigInt                             | Quorum value                                     |
| executed           | Boolean                            | Whether the proposal has been executed or not    |
| execution          | [Execution](entities.md#execution) | Execution tx details                             |
| script             | Bytes                              | Proposal calldata                                |

### Vote

Details of a vote cast on a proposal.

| Field     | Type                             | Description                                     |
| --------- | -------------------------------- | ----------------------------------------------- |
| id        | ID                               | Entity ID                                       |
| tx        | Bytes                            | Transaction hash                                |
| voteId    | BigInt                           | Vote ID                                         |
| proposal  | [Proposal](entities.md#proposal) | Proposal details                                |
| voter     | [User](entities.md#user)         | Voter address and metadata                      |
| supports  | Boolean                          | Whether the vote is for or against the proposal |
| stake     | BigInt                           | User voting power                               |
| timestamp | BigInt                           | Vote timestamp                                  |

### Execution

Tracks all proposal executions.

| Field    | Type                             | Description       |
| -------- | -------------------------------- | ----------------- |
| id       | ID                               | Entity ID         |
| tx       | Bytes                            | Transaction hash  |
| voteId   | BigInt                           | Vote ID           |
| proposal | [Proposal](entities.md#proposal) | Proposal metadata |

### GaugeLiquidity

Tracks updates to gauge liquidity limits

| Field           | Type                       | Description                                           |
| --------------- | -------------------------- | ----------------------------------------------------- |
| id              | ID                         | Entity ID                                             |
| user            | Bytes                      | User updating the limit                               |
| gauge           | [Gauge](entities.md#gauge) | Gauge metadata                                        |
| originalBalance | BigInt                     | Original user balance                                 |
| originalSupply  | BigInt                     | Original total supply                                 |
| workingBalance  | BigInt                     | Balance after boost has been applied                  |
| workingSupply   | BigInt                     | Total LP token amounts after boosts have been applied |
| timestamp       | BigInt                     | Event timestamp                                       |
| block           | BigInt                     | Event block                                           |
| tx              | Bytes                      | Transaction hash                                      |

### GaugeDeposit

Tracks all deposits to a gauge

| Field    | Type                       | Description       |
| -------- | -------------------------- | ----------------- |
| id       | ID                         | Entity ID         |
| gauge    | [Gauge](entities.md#gauge) | Gauge metadata    |
| provider | Bytes                      | Depositor address |
| value    | BigInt                     | Amount deposited  |

### GaugeWithdraw

Tracks all withdrawal from a gauge

| Field    | Type                       | Description         |
| -------- | -------------------------- | ------------------- |
| id       | ID                         | Entity ID           |
| gauge    | [Gauge](entities.md#gauge) | Gauge metadata      |
| provider | Bytes                      | Withdrawing address |
| value    | BigInt                     | Amount withdrawn    |

### Gauge

Metadata about gauges. Because not all gauges have an associated pool, the Pool field may be empty.&#x20;

| Field     | Type                                      | Description                      |
| --------- | ----------------------------------------- | -------------------------------- |
| id        | ID                                        | Entity ID                        |
| address   | Bytes                                     | Gauge address                    |
| name      | String                                    | Gauge name                       |
| type      | [GaugeType](entities.md#gauge)            | Gauge type                       |
| platform  | [Platform](entities.md#platform)          | Platform                         |
| pool      | [Pool](entities.md#pool)                  | Pool address (if can be matched) |
| isKilled  | Boolean                                   | Whether the gauge is inactive    |
| created   | BigInt                                    | Creation date                    |
| block     | BigInt                                    | Creation block                   |
| tx        | Bytes                                     | Transaction hash                 |
| weights   | \[[GaugeWeight](entities.md#gaugeweight)] | Gauge weight history             |
| emissions | \[[Emission](entities.md#emission)]       | Gauge emission history           |

### Pool

Pools associated to a certain gauge.

| Field     | Type                                | Description        |
| --------- | ----------------------------------- | ------------------ |
| id        | ID                                  | Entity ID          |
| lpToken   | Bytes                               | LP Token address   |
| address   | Bytes                               | Pool address       |
| name      | String                              | Pool name          |
| platform  | [Platform](entities.md#platform)    | Platform           |
| gauges    | \[[Gauge](entities.md#gauge)]       | Gauges associated  |
| emissions | \[[Emission](entities.md#emission)] | Emissions received |

### SnapshotTime

Internal use. Date of last saved snapshot.

| Field    | Type                             | Description |
| -------- | -------------------------------- | ----------- |
| id       | ID                               | Entity ID   |
| platform | [Platform](entities.md#platform) | Platform    |

### GaugeWeight

Tracks all increase to a gauge's weight as a result of a vote.

| Field     | Type                       | Description     |
| --------- | -------------------------- | --------------- |
| id        | ID                         | Entity ID       |
| gauge     | [Gauge](entities.md#gauge) | Gauge metadata  |
| timestamp | BigInt                     | Event timestamp |
| block     | BigInt                     | Event block     |
| weight    | BigDecimal                 | Weight added    |

### Emission

Tracks CRV emissions received by a gauge over a weekly period.

| Field     | Type                       | Description                      |
| --------- | -------------------------- | -------------------------------- |
| id        | ID                         | Entity ID                        |
| gauge     | [Gauge](entities.md#gauge) | Gauge metadata                   |
| pool      | [Pool](entities.md#pool)   | Pool address                     |
| timestamp | BigInt                     | Week timestamp                   |
| crvAmount | BigDecimal                 | CRV emissions received that week |
| value     | BigDecimal                 | USD value of CRV emissions       |

### GaugeType

Types of gauges

| Field      | Type                          | Description                   |
| ---------- | ----------------------------- | ----------------------------- |
| id         | ID                            | Entity ID                     |
| name       | String                        | Gauge type name               |
| weight     | BigDecimal                    | Gauge type weight             |
| gaugeCount | BigInt                        | Number of gauges of this type |
| gauges     | \[[Gauge](entities.md#gauge)] | Gauges of this type           |

### GaugeTotalWeight

Internal use. Tracks the historical total weight of all gauges.

| Field     | Type       | Description                             |
| --------- | ---------- | --------------------------------------- |
| id        | ID         | Entity ID                               |
| timestamp | BigInt     | Timestamp for the total weight snapshot |
| block     | BigInt     | Block of the snapshot                   |
| weight    | BigDecimal | Total weight at time of snapshot        |

### GaugeWeightVote

Tracks metadata from all gauge weight vote events.

| Field     | Type       | Description         |
| --------- | ---------- | ------------------- |
| id        | ID         | Entity ID           |
| tx        | Bytes      | Transaction hash    |
| timestamp | BigInt     | Vote timestamp      |
| user      | User       | Voting user address |
| weight    | BigDecimal | Weight added        |
| gauge     | Bytes      | Gauge address       |
