---
description: Sample queries for the Curve subgraphs
---

# Sample Queries

## Retrieve general metadata about a pool

```
{
  pool(id: "0xbebc44782c7db0a1a60cb6fe97d0b483032ff1c7") {
    name
    coins
    coinNames
    assetType
    creationDate
    creationTx
  }
}
```

## Retrieve the most recent TVL value for a pool

```
{
  pools (where: {id: "0xbebc44782c7db0a1a60cb6fe97d0b483032ff1c7"}) {
    dailyPoolSnapshots(first: 1 orderBy: timestamp orderDirection: desc) {
      tvl
      timestamp
    }
  }
}
```

## Retrieve the 5 most recent hourly candles for a pool

```
{
  candles(first: 5 
    orderBy: timestamp
    orderDirection:desc
    where: {period: 3600 
      pool: "0xbebc44782c7db0a1a60cb6fe97d0b483032ff1c7"}) {
    low
    high
    close
    open
    timestamp
  }
}
```

## Retrieve a pool's volume for the past 7 days

```
{
  pool(id: "0xbebc44782c7db0a1a60cb6fe97d0b483032ff1c7") {
    swapVolumeSnapshots(first: 7 orderBy: timestamp orderDirection: desc) {
      volumeUSD
      timestamp
    }
  }
}
```
