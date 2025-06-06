# Polymarket Main Subgraph Queries

**Subgraph ID**: `QmdyCguLEisTtQFveEkvMhTH7UzjyhnrF9kpvhYeG4QX8a`

## Global Statistics

### Get Global Trading Metrics
```graphql
{
  globals {
    id
    numConditions
    numOpenConditions
    numClosedConditions
    numTraders
    tradesQuantity
    buysQuantity
    sellsQuantity
    scaledCollateralVolume
    scaledCollateralFees
    scaledCollateralBuyVolume
    scaledCollateralSellVolume
  }
}
```

## Market Analysis

### Get Top Markets by Volume
```graphql
{
  fixedProductMarketMakers(
    first: 10
    orderBy: scaledCollateralVolume
    orderDirection: desc
  ) {
    id
    creator
    creationTimestamp
    scaledCollateralVolume
    scaledFeeVolume
    tradesQuantity
    outcomeTokenAmounts
    outcomeTokenPrices
    conditions
    fee
  }
}
```

### Get Market with Detailed Information
```graphql
{
  fixedProductMarketMaker(id: "MARKET_ADDRESS") {
    id
    creator
    creationTimestamp
    collateralToken {
      id
      name
      symbol
      decimals
    }
    conditions
    fee
    scaledCollateralVolume
    scaledFeeVolume
    outcomeTokenAmounts
    outcomeTokenPrices
    outcomeSlotCount
    tradesQuantity
    buysQuantity
    sellsQuantity
    lastActiveDay
    totalSupply
    poolMembers {
      funder {
        id
        scaledCollateralVolume
      }
      amount
    }
  }
}
```

### Get Recent Market Activity
```graphql
{
  fixedProductMarketMakers(
    first: 20
    orderBy: lastActiveDay
    orderDirection: desc
    where: { lastActiveDay_gt: "1640995200" }
  ) {
    id
    scaledCollateralVolume
    tradesQuantity
    lastActiveDay
    outcomeTokenPrices
    conditions
  }
}
```

## Trading Activity

### Get Recent Transactions
```graphql
{
  transactions(
    first: 50
    orderBy: timestamp
    orderDirection: desc
  ) {
    id
    type
    timestamp
    user {
      id
      scaledCollateralVolume
      numTrades
    }
    market {
      id
      scaledCollateralVolume
    }
    tradeAmount
    feeAmount
    outcomeIndex
    outcomeTokensAmount
  }
}
```

### Get User's Trading History
```graphql
{
  transactions(
    where: { user: "USER_ADDRESS" }
    orderBy: timestamp
    orderDirection: desc
    first: 100
  ) {
    id
    type
    timestamp
    market {
      id
      conditions
      outcomeTokenPrices
    }
    tradeAmount
    feeAmount
    outcomeIndex
    outcomeTokensAmount
  }
}
```

### Get Large Trades (Whales)
```graphql
{
  transactions(
    where: { tradeAmount_gt: "10000000000" }
    orderBy: tradeAmount
    orderDirection: desc
    first: 20
  ) {
    id
    type
    timestamp
    user {
      id
      scaledCollateralVolume
    }
    market {
      id
      conditions
    }
    tradeAmount
    outcomeIndex
    outcomeTokensAmount
  }
}
```

## User Analysis

### Get Top Traders by Volume
```graphql
{
  accounts(
    first: 20
    orderBy: scaledCollateralVolume
    orderDirection: desc
  ) {
    id
    creationTimestamp
    lastSeenTimestamp
    scaledCollateralVolume
    scaledProfit
    numTrades
    lastTradedTimestamp
  }
}
```

### Get User Profile with Positions
```graphql
{
  account(id: "USER_ADDRESS") {
    id
    creationTimestamp
    lastSeenTimestamp
    scaledCollateralVolume
    scaledProfit
    numTrades
    marketPositions {
      market {
        id
        condition {
          id
        }
        outcomeIndex
      }
      quantityBought
      quantitySold
      netQuantity
      valueBought
      valueSold
      netValue
      feesPaid
    }
    fpmmPoolMemberships {
      pool {
        id
        scaledCollateralVolume
      }
      amount
    }
  }
}
```

### Get Active Users (Recent Activity)
```graphql
{
  accounts(
    where: { lastSeenTimestamp_gt: "1704067200" }
    orderBy: lastSeenTimestamp
    orderDirection: desc
    first: 50
  ) {
    id
    lastSeenTimestamp
    scaledCollateralVolume
    numTrades
    scaledProfit
  }
}
```

## Market Positions

### Get Positions for Specific Market
```graphql
{
  marketPositions(
    where: { market: "MARKET_TOKEN_ID" }
    orderBy: netQuantity
    orderDirection: desc
    first: 50
  ) {
    id
    user {
      id
      scaledCollateralVolume
    }
    market {
      id
      priceOrderbook
    }
    quantityBought
    quantitySold
    netQuantity
    valueBought
    valueSold
    netValue
    feesPaid
  }
}
```

### Get User's Largest Positions
```graphql
{
  marketPositions(
    where: { user: "USER_ADDRESS" }
    orderBy: netValue
    orderDirection: desc
    first: 20
  ) {
    id
    market {
      id
      condition {
        id
        oracle
        questionId
        resolutionTimestamp
        payouts
      }
      outcomeIndex
      priceOrderbook
    }
    netQuantity
    netValue
    feesPaid
  }
}
```

## Conditions & Market Resolution

### Get Resolved Markets
```graphql
{
  conditions(
    where: { resolutionTimestamp_not: null }
    orderBy: resolutionTimestamp
    orderDirection: desc
    first: 20
  ) {
    id
    oracle
    questionId
    outcomeSlotCount
    resolutionTimestamp
    payouts
    payoutNumerators
    payoutDenominator
    resolutionHash
    fixedProductMarketMakers {
      id
      scaledCollateralVolume
      tradesQuantity
    }
  }
}
```

### Get Pending Markets (Unresolved)
```graphql
{
  conditions(
    where: { resolutionTimestamp: null }
    orderBy: id
    first: 50
  ) {
    id
    oracle
    questionId
    outcomeSlotCount
    fixedProductMarketMakers {
      id
      scaledCollateralVolume
      tradesQuantity
      lastActiveDay
      outcomeTokenPrices
    }
  }
}
```

## Liquidity Analysis

### Get Liquidity Providers
```graphql
{
  fpmmPoolMemberships(
    orderBy: amount
    orderDirection: desc
    first: 50
  ) {
    id
    pool {
      id
      scaledCollateralVolume
      scaledFeeVolume
      conditions
    }
    funder {
      id
      scaledCollateralVolume
    }
    amount
  }
}
```

### Get Funding Events
```graphql
{
  fpmmFundingAdditions(
    orderBy: timestamp
    orderDirection: desc
    first: 20
  ) {
    id
    timestamp
    fpmm {
      id
      conditions
    }
    funder {
      id
    }
    amountsAdded
    sharesMinted
  }
}
```

## Order Book Analysis

### Get Order Book Activity
```graphql
{
  orderbooks(
    orderBy: scaledCollateralVolume
    orderDirection: desc
    first: 20
  ) {
    id
    tradesQuantity
    buysQuantity
    sellsQuantity
    scaledCollateralVolume
    scaledCollateralBuyVolume
    scaledCollateralSellVolume
    lastActiveDay
  }
}
```

### Get Enriched Order Fills
```graphql
{
  enrichedOrderFilleds(
    orderBy: timestamp
    orderDirection: desc
    first: 50
  ) {
    id
    timestamp
    maker {
      id
    }
    taker {
      id
    }
    market {
      id
      scaledCollateralVolume
    }
    side
    size
    price
    orderHash
  }
}
```

## Time-based Analytics

### Get Trading Activity by Time Period
```graphql
{
  transactions(
    where: {
      timestamp_gte: "1704067200"
      timestamp_lt: "1704153600"
    }
    orderBy: timestamp
    first: 1000
  ) {
    id
    timestamp
    type
    tradeAmount
    market {
      id
    }
    user {
      id
    }
  }
}
```

### Get Market Creation Timeline
```graphql
{
  fixedProductMarketMakers(
    orderBy: creationTimestamp
    orderDirection: desc
    first: 100
  ) {
    id
    creator
    creationTimestamp
    conditions
    scaledCollateralVolume
    tradesQuantity
  }
}
```
