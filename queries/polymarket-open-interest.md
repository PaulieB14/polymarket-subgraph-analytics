# Polymarket Open Interest Subgraph Queries

**Subgraph ID**: `QmbxydtB3MF2yNriAHhsrBmqTx44aaw44jjNFwZNWaW7R6`

## Global Open Interest

### Get Global Open Interest
```graphql
{
  globalOpenInterests {
    id
    amount
  }
}
```

### Get Current Global Open Interest
```graphql
{
  globalOpenInterest(id: "") {
    id
    amount
  }
}
```

## Market Open Interest

### Get All Market Open Interest
```graphql
{
  marketOpenInterests(
    orderBy: amount
    orderDirection: desc
    first: 100
  ) {
    id
    amount
  }
}
```

### Get Market Open Interest by Condition ID
```graphql
{
  marketOpenInterest(id: "CONDITION_ID") {
    id
    amount
  }
}
```

### Get Top Markets by Open Interest
```graphql
{
  marketOpenInterests(
    orderBy: amount
    orderDirection: desc
    first: 20
  ) {
    id
    amount
  }
}
```

### Get Markets with Significant Open Interest
```graphql
{
  marketOpenInterests(
    where: { amount_gt: "1000000000000000000" }
    orderBy: amount
    orderDirection: desc
  ) {
    id
    amount
  }
}
```

### Get Markets with Low Open Interest
```graphql
{
  marketOpenInterests(
    where: { amount_lt: "100000000000000000" }
    orderBy: amount
    orderDirection: asc
    first: 50
  ) {
    id
    amount
  }
}
```

## Conditions Analysis

### Get All Conditions
```graphql
{
  conditions {
    id
  }
}
```

### Get Condition by ID
```graphql
{
  condition(id: "CONDITION_ID") {
    id
  }
}
```

## Neg Risk Events Analysis

### Get All Neg Risk Events
```graphql
{
  negRiskEvents {
    id
    feeBps
    questionCount
  }
}
```

### Get Neg Risk Event by ID
```graphql
{
  negRiskEvent(id: "NEG_RISK_MARKET_ID") {
    id
    feeBps
    questionCount
  }
}
```

### Get Neg Risk Events by Fee Structure
```graphql
{
  negRiskEvents(
    where: { feeBps: "200" }
  ) {
    id
    feeBps
    questionCount
  }
}
```

### Get Multi-Question Neg Risk Events
```graphql
{
  negRiskEvents(
    where: { questionCount_gt: 1 }
    orderBy: questionCount
    orderDirection: desc
  ) {
    id
    feeBps
    questionCount
  }
}
```

### Get High Fee Neg Risk Events
```graphql
{
  negRiskEvents(
    where: { feeBps_gt: "100" }
    orderBy: feeBps
    orderDirection: desc
  ) {
    id
    feeBps
    questionCount
  }
}
```

## Combined Analysis

### Get Open Interest Distribution
```graphql
{
  globalOpenInterests {
    id
    amount
  }
  marketOpenInterests(
    orderBy: amount
    orderDirection: desc
    first: 10
  ) {
    id
    amount
  }
}
```

### Get Market Open Interest with Neg Risk Info
```graphql
{
  marketOpenInterests(
    orderBy: amount
    orderDirection: desc
    first: 20
  ) {
    id
    amount
  }
  negRiskEvents {
    id
    feeBps
    questionCount
  }
}
```

### Get Open Interest Summary
```graphql
{
  globalOpenInterests {
    amount
  }
  marketOpenInterests(
    where: { amount_gt: "0" }
  ) {
    id
    amount
  }
}
```

## Open Interest Thresholds

### Get Markets Above Threshold
```graphql
{
  marketOpenInterests(
    where: { amount_gte: "5000000000000000000" }
    orderBy: amount
    orderDirection: desc
  ) {
    id
    amount
  }
}
```

### Get Markets Below Threshold
```graphql
{
  marketOpenInterests(
    where: { amount_lte: "1000000000000000000" }
    orderBy: amount
    orderDirection: asc
  ) {
    id
    amount
  }
}
```

### Get Market Open Interest Range
```graphql
{
  marketOpenInterests(
    where: {
      amount_gte: "1000000000000000000"
      amount_lte: "10000000000000000000"
    }
    orderBy: amount
    orderDirection: desc
  ) {
    id
    amount
  }
}
```
