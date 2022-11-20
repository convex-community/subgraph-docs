# Sample Queries

## Retrieve information on all Convex's pools

```
{ pools(first: 1000)
    {
        id
        name
        token
        lpToken
        swap
        gauge
        crvRewardsPool
        isV2
        creationDate
        creationBlock
        tvl
        curveTvlRatio
        baseApr
        crvApr
        cvxApr
        extraRewardsApr
        extraRewards {
            contract
        }
    }
}

```

## Retrieve last snapshot of all convex pools

```
{
    pools(first: 1000) {
        snapshots(first: 1 orderBy: timestamp orderDirection: desc){
            id
            poolid {
                id
            }
            poolName
            withdrawalCount
            withdrawalVolume
            withdrawalValue
            depositCount
            depositVolume
            depositValue
            lpTokenBalance
            lpTokenVirtualPrice
            lpTokenUSDPrice
            tvl
            curveTvlRatio
            baseApr
            crvApr
            cvxApr
            extraRewardsApr
            timestamp
            block
        }
    }
}
```

## Retrieve historical breakdown of Convex revenue from Curve

Note that this does not include bribes and revenue from Frax

```
{ revenueWeeklySnapshots(first: 1000) {
  id
  crvRevenueToCallersAmount
  crvRevenueToPlatformAmount
  crvRevenueToCvxStakersAmount
  crvRevenueToLpProvidersAmount
  crvRevenueToCvxCrvStakersAmount
  totalCrvRevenue
  crvRevenueToCallersAmountCumulative
  crvRevenueToPlatformAmountCumulative
  crvRevenueToCvxStakersAmountCumulative
  crvRevenueToLpProvidersAmountCumulative
  crvRevenueToCvxCrvStakersAmountCumulative
  totalCrvRevenueCumulative
  crvPrice
  timestamp
}}
```

