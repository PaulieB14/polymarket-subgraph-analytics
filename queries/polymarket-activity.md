# Polymarket Activity Polygon Subgraph Queries

**Subgraph ID**: `Qmf3qPUsfQ8et6E3QNBmuXXKqUJi91mo5zbsaTkQrSnMAP`

## Split Operations

### Get Recent Split Transactions
```graphql
{
  splits(
    orderBy: timestamp
    orderDirection: desc
    first: 50
  ) {
    id
    timestamp
    stakeholder
    condition
    amount
  }
}
```

### Get Splits by User
```graphql
{
  splits(
    where: { stakeholder: "USER_ADDRESS" }
    orderBy: timestamp
    orderDirection: desc
  ) {
    id
    timestamp
    condition
    amount
  }
}
```

### Get Splits by Condition
```graphql
{
  splits(
    where: { condition: "CONDITION_ID" }
    orderBy: timestamp
    orderDirection: desc
  ) {
    id
    timestamp
    stakeholder
    amount
  }
}
```

### Get Large Split Operations
```graphql
{
  splits(
    where: { amount_gt: "1000000000000000000" }
    orderBy: amount
    orderDirection: desc
    first: 20
  ) {
    id
    timestamp
    stakeholder
    condition
    amount
  }
}
```

## Merge Operations

### Get Recent Merge Transactions
```graphql
{
  merges(
    orderBy: timestamp
    orderDirection: desc
    first: 50
  ) {
    id
    timestamp
    stakeholder
    condition
    amount
  }
}
```

### Get Merges by User
```graphql
{
  merges(
    where: { stakeholder: "USER_ADDRESS" }
    orderBy: timestamp
    orderDirection: desc
  ) {
    id
    timestamp
    condition
    amount
  }
}
```

### Get Merges by Condition
```graphql
{
  merges(
    where: { condition: "CONDITION_ID" }
    orderBy: timestamp
    orderDirection: desc
  ) {
    id
    timestamp
    stakeholder
    amount
  }
}
```

### Get Large Merge Operations
```graphql
{
  merges(
    where: { amount_gt: "1000000000000000000" }
    orderBy: amount
    orderDirection: desc
    first: 20
  ) {
    id
    timestamp
    stakeholder
    condition
    amount
  }
}
```

## Redemption Operations

### Get Recent Redemptions
```graphql
{
  redemptions(
    orderBy: timestamp
    orderDirection: desc
    first: 50
  ) {
    id
    timestamp
    redeemer
    condition
    indexSets
    payout
  }
}
```

### Get Redemptions by User
```graphql
{
  redemptions(
    where: { redeemer: "USER_ADDRESS" }
    orderBy: timestamp
    orderDirection: desc
  ) {
    id
    timestamp
    condition
    indexSets
    payout
  }
}
```

### Get Redemptions by Condition
```graphql
{
  redemptions(
    where: { condition: "CONDITION_ID" }
    orderBy: timestamp
    orderDirection: desc
  ) {
    id
    timestamp
    redeemer
    indexSets
    payout
  }
}
```

### Get Large Redemptions
```graphql
{
  redemptions(
    where: { payout_gt: "1000000000000000000" }
    orderBy: payout
    orderDirection: desc
    first: 20
  ) {
    id
    timestamp
    redeemer
    condition
    indexSets
    payout
  }
}
```

## Neg Risk Conversions

### Get Recent Neg Risk Conversions
```graphql
{
  negRiskConversions(
    orderBy: timestamp
    orderDirection: desc
    first: 50
  ) {
    id
    timestamp
    stakeholder
    negRiskMarketId
    amount
    indexSet
    questionCount
  }
}
```

### Get Neg Risk Conversions by User
```graphql
{
  negRiskConversions(
    where: { stakeholder: "USER_ADDRESS" }
    orderBy: timestamp
    orderDirection: desc
  ) {
    id
    timestamp
    negRiskMarketId
    amount
    indexSet
    questionCount
  }
}
```

### Get Neg Risk Conversions by Market
```graphql
{
  negRiskConversions(
    where: { negRiskMarketId: "NEG_RISK_MARKET_ID" }
    orderBy: timestamp
    orderDirection: desc
  ) {
    id
    timestamp
    stakeholder
    amount
    indexSet
    questionCount
  }
}
```

### Get Large Neg Risk Conversions
```graphql
{
  negRiskConversions(
    where: { amount_gt: "1000000000000000000" }
    orderBy: amount
    orderDirection: desc
    first: 20
  ) {
    id
    timestamp
    stakeholder
    negRiskMarketId
    amount
    indexSet
    questionCount
  }
}
```

## Market & Position Analysis

### Get All Positions
```graphql
{
  positions {
    id
    condition
    outcomeIndex
  }
}
```

### Get Position by Token ID
```graphql
{
  position(id: "TOKEN_ID") {
    id
    condition
    outcomeIndex
  }
}
```

### Get Positions by Condition
```graphql
{
  positions(
    where: { condition: "CONDITION_ID" }
  ) {
    id
    outcomeIndex
  }
}
```

### Get All Conditions
```graphql
{
  conditions {
    id
  }
}
```

### Get All Fixed Product Market Makers
```graphql
{
  fixedProductMarketMakers {
    id
  }
}
```

### Get All Neg Risk Events
```graphql
{
  negRiskEvents {
    id
    questionCount
  }
}
```

## Combined Activity Analysis

### Get All Activity for a User
```graphql
{
  splits(where: { stakeholder: "USER_ADDRESS" }) {
    id
    timestamp
    condition
    amount
  }
  merges(where: { stakeholder: "USER_ADDRESS" }) {
    id
    timestamp
    condition
    amount
  }
  redemptions(where: { redeemer: "USER_ADDRESS" }) {
    id
    timestamp
    condition
    payout
  }
  negRiskConversions(where: { stakeholder: "USER_ADDRESS" }) {
    id
    timestamp
    negRiskMarketId
    amount
  }
}
```

### Get Activity by Time Range
```graphql
{
  splits(
    where: {
      timestamp_gte: "1704067200"
      timestamp_lt: "1704153600"
    }
    orderBy: timestamp
  ) {
    id
    timestamp
    stakeholder
    condition
    amount
  }
  merges(
    where: {
      timestamp_gte: "1704067200"
      timestamp_lt: "1704153600"
    }
    orderBy: timestamp
  ) {
    id
    timestamp
    stakeholder
    condition
    amount
  }
}
```

### Get High-Value Activities
```graphql
{
  splits(where: { amount_gt: "10000000000000000000" }) {
    id
    timestamp
    stakeholder
    condition
    amount
  }
  merges(where: { amount_gt: "10000000000000000000" }) {
    id
    timestamp
    stakeholder
    condition
    amount
  }
  redemptions(where: { payout_gt: "10000000000000000000" }) {
    id
    timestamp
    redeemer
    condition
    payout
  }
}
```
