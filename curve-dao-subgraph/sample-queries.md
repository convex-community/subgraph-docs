# Sample Queries

## Retrieve daily trading volume data for all markets

```
{
  markets(first: 1000) {
    id
    collateralName
    amm {
      volumeSnapshots(first: 1000 orderBy: timestamp orderDirection: desc where: {period:86400}) {
        swapVolumeUSD
        timestamp
      }
    }
  }
}
```

## Retrieve all the band snapshots for each market

Note that as total band number can go up to 1024, pagination may be needed in some cases to retrieve all bands. (Here only non-empty bands are retrieved)

```
{
  markets(first: 10) {
    id
    collateralName
    snapshots(where: {bandSnapshot:true} orderBy: timestamp orderDirection: desc) {
      timestamp
      bands(first: 1000 orderBy: index orderDirection: asc where: {collateralUsd_gt: 0 stableCoin_gt: 0}){
        index
        collateralUsd
        stableCoin
      } 
    }
  }
}
```

## Retrieve all time total (cumulative) swap volume for each market

```
{
  markets(first: 1000) {
    id
    collateralName
    amm {
      totalSwapVolume
      volumeSnapshots(first: 1000 orderBy: timestamp orderDirection: desc where: {period:86400}) {
        swapVolumeUSD
        timestamp
      }
    }
  }
}
```

