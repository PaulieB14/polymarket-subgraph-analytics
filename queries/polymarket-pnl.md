# Polymarket Profit and Loss Subgraph Queries

**Subgraph ID**: `QmZAYiMeZiWC7ZjdWepek7hy1jbcW3ngimBF9ibTiTtwQU`

## User Position Analysis

### Get All User Positions
```graphql
{
  userPositions(
    first: 100
    orderBy: realizedPnl
    orderDirection: desc
  ) {
    id
    user
    tokenId
    amount
    avgPrice
    realizedPnl
    totalBought
  }
}
```

### Get Positions for Specific User
```graphql
{
  userPositions(
    where: { user: "USER_ADDRESS" }
    orderBy: realizedPnl
    orderDirection: desc
  ) {
    id
    tokenId
    amount
    avgPrice
    realizedPnl
    totalBought
  }
}
```

### Get Top Profitable Positions
```graphql
{
  userPositions(
    where: { realizedPnl_gt: "0" }
    orderBy: realizedPnl
    orderDirection: desc
    first: 50
  ) {
    id
    user
    tokenId
    amount
    avgPrice
    realizedPnl
    totalBought
  }
}
```

### Get Biggest Losers
```graphql
{
  userPositions(
    where: { realizedPnl_lt: "0" }
    orderBy: realizedPnl
    orderDirection: asc
    first: 50
  ) {
    id
    user
    tokenId
    amount
    avgPrice
    realizedPnl
    totalBought
  }
}
```

### Get Positions by Token ID
```graphql
{
  userPositions(
    where: { tokenId: "TOKEN_ID" }
    orderBy: amount
    orderDirection: desc
  ) {
    id
    user
    amount
    avgPrice
    realizedPnl
    totalBought
  }
}
```

### Get Large Position Holders
```graphql
{
  userPositions(
    where: { amount_gt: "1000000000000000000" }
    orderBy: amount
    orderDirection: desc
    first: 100
  ) {
    id
    user
    tokenId
    amount
    avgPrice
    realizedPnl
    totalBought
  }
}
```

## Market Condition Analysis

### Get All Conditions with Payouts
```graphql
{
  conditions {
    id
    positionIds
    payoutNumerators
    payoutDenominator
  }
}
```

### Get Condition by ID
```graphql
{
  condition(id: "CONDITION_ID") {
    id
    positionIds
    payoutNumerators
    payoutDenominator
  }
}
```

## FPMM Analysis

### Get All FPMMs
```graphql
{
  fpmms {
    id
    conditionId
  }
}
```

### Get FPMM by Condition
```graphql
{
  fpmms(
    where: { conditionId: "CONDITION_ID" }
  ) {
    id
    conditionId
  }
}
```

## Neg Risk Events

### Get All Neg Risk Events
```graphql
{
  negRiskEvents {
    id
    questionCount
  }
}
```

### Get Neg Risk Event by ID
```graphql
{
  negRiskEvent(id: "NEG_RISK_MARKET_ID") {
    id
    questionCount
  }
}
```

## Combined Analysis Queries

### Get User PnL Summary
```graphql
{
  userPositions(
    where: { user: "USER_ADDRESS" }
  ) {
    id
    tokenId
    amount
    avgPrice
    realizedPnl
    totalBought
  }
}
```

### Get Token Performance Analysis
```graphql
{
  userPositions(
    where: { tokenId: "TOKEN_ID" }
    orderBy: realizedPnl
    orderDirection: desc
  ) {
    id
    user
    amount
    avgPrice
    realizedPnl
    totalBought
  }
}
```

### Get Users with Multiple Positions
```graphql
{
  userPositions(
    where: { totalBought_gt: "1000000000000000000" }
    orderBy: totalBought
    orderDirection: desc
  ) {
    id
    user
    tokenId
    amount
    avgPrice
    realizedPnl
    totalBought
  }
}
```

### Get Average Price Analysis
```graphql
{
  userPositions(
    where: { 
      tokenId: "TOKEN_ID"
      amount_gt: "0"
    }
    orderBy: avgPrice
    orderDirection: asc
  ) {
    id
    user
    amount
    avgPrice
    realizedPnl
  }
}
```

## Profit Distribution Analysis

### Get Profit Distribution by User
```graphql
{
  userPositions(
    where: { user: "USER_ADDRESS" }
    orderBy: realizedPnl
    orderDirection: desc
  ) {
    tokenId
    realizedPnl
    amount
    avgPrice
  }
}
```

### Get Token-wise Profit Leaders
```graphql
{
  userPositions(
    where: { 
      tokenId: "TOKEN_ID"
      realizedPnl_gt: "0" 
    }
    orderBy: realizedPnl
    orderDirection: desc
    first: 10
  ) {
    user
    amount
    avgPrice
    realizedPnl
    totalBought
  }
}
```
