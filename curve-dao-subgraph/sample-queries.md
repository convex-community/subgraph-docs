# Sample Queries

## Retrieve all gauges and their latest weight

```
{  
  gauges(first: 1000) {
  address
    name
    type {
      id
    }
    weight: weights(first: 1 orderBy:timestamp orderDirection:desc) {
      weight
    }
  }
}
```

## Get all of a user's proposals

```
{
  proposals(first: 1000 where: {creator: "0xbe286d574b1ea46f54955bd856821f84dfd20b2e"} orderBy: startDate) {
  voteId
  voteType
  creator
  startDate
  snapshotBlock
  ipfsMetadata
  metadata
  votesFor
  votesAgainst
  voteCount
  supportRequired
  minAcceptQuorum
  executed
  }
}
```

## Retrieve the emissions received by a pool in CRV and USD over the past few weeks

```
{
  emissions (where: {pool: "0x3a283d9c08e8b55966afb64c515f5143cf907611"} 
              orderBy: timestamp 
              orderDirection: desc) {
    gauge {
      address
    }
    pool {
      id
    }
    crvAmount
    value
    timestamp
  }
}
```

