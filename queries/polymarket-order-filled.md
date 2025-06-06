# Polymarket Order Filled Events Subgraph Queries

**Subgraph ID**: `Qma5ngHGmgW8qea4nEXmFjNtJb9aXDwV75hWekpTKD1fYf`

## Order Filled Events

### Get Recent Order Fills
```graphql
{
  orderFilleds(
    orderBy: blockTimestamp
    orderDirection: desc
    first: 50
  ) {
    id
    orderHash
    maker
    taker
    makerAssetId
    takerAssetId
    makerAmountFilled
    takerAmountFilled
    fee
    blockNumber
    blockTimestamp
    transactionHash
  }
}
```

### Get Order Fills by Maker
```graphql
{
  orderFilleds(
    where: { maker: "MAKER_ADDRESS" }
    orderBy: blockTimestamp
    orderDirection: desc
  ) {
    id
    orderHash
    taker
    makerAssetId
    takerAssetId
    makerAmountFilled
    takerAmountFilled
    fee
    blockTimestamp
    transactionHash
  }
}
```

### Get Order Fills by Taker
```graphql
{
  orderFilleds(
    where: { taker: "TAKER_ADDRESS" }
    orderBy: blockTimestamp
    orderDirection: desc
  ) {
    id
    orderHash
    maker
    makerAssetId
    takerAssetId
    makerAmountFilled
    takerAmountFilled
    fee
    blockTimestamp
    transactionHash
  }
}
```

### Get Order Fills by Asset ID
```graphql
{
  orderFilleds(
    where: { makerAssetId: "ASSET_ID" }
    orderBy: blockTimestamp
    orderDirection: desc
  ) {
    id
    orderHash
    maker
    taker
    takerAssetId
    makerAmountFilled
    takerAmountFilled
    fee
    blockTimestamp
    transactionHash
  }
}
```

### Get Large Order Fills
```graphql
{
  orderFilleds(
    where: { makerAmountFilled_gt: "1000000000000000000" }
    orderBy: makerAmountFilled
    orderDirection: desc
    first: 20
  ) {
    id
    orderHash
    maker
    taker
    makerAssetId
    takerAssetId
    makerAmountFilled
    takerAmountFilled
    fee
    blockTimestamp
  }
}
```

### Get High Fee Order Fills
```graphql
{
  orderFilleds(
    where: { fee_gt: "10000000000000000" }
    orderBy: fee
    orderDirection: desc
    first: 20
  ) {
    id
    orderHash
    maker
    taker
    makerAssetId
    takerAssetId
    makerAmountFilled
    takerAmountFilled
    fee
    blockTimestamp
  }
}
```

## Neg Risk CTF Exchange Order Fills

### Get Recent Neg Risk Order Fills
```graphql
{
  negRiskCtfExchangeOrderFilleds(
    orderBy: blockTimestamp
    orderDirection: desc
    first: 50
  ) {
    id
    orderHash
    maker
    taker
    makerAssetId
    takerAssetId
    makerAmountFilled
    takerAmountFilled
    fee
    blockNumber
    blockTimestamp
    transactionHash
  }
}
```

### Get Neg Risk Order Fills by Maker
```graphql
{
  negRiskCtfExchangeOrderFilleds(
    where: { maker: "MAKER_ADDRESS" }
    orderBy: blockTimestamp
    orderDirection: desc
  ) {
    id
    orderHash
    taker
    makerAssetId
    takerAssetId
    makerAmountFilled
    takerAmountFilled
    fee
    blockTimestamp
  }
}
```

### Get Large Neg Risk Order Fills
```graphql
{
  negRiskCtfExchangeOrderFilleds(
    where: { makerAmountFilled_gt: "1000000000000000000" }
    orderBy: makerAmountFilled
    orderDirection: desc
    first: 20
  ) {
    id
    orderHash
    maker
    taker
    makerAssetId
    takerAssetId
    makerAmountFilled
    takerAmountFilled
    fee
    blockTimestamp
  }
}
```

## Payout Redemptions

### Get Recent Payout Redemptions
```graphql
{
  payoutRedemptions(
    orderBy: blockTimestamp
    orderDirection: desc
    first: 50
  ) {
    id
    redeemer
    collateralToken
    parentCollectionId
    conditionId
    indexSets
    payout
    blockNumber
    blockTimestamp
    transactionHash
  }
}
```

### Get Payout Redemptions by User
```graphql
{
  payoutRedemptions(
    where: { redeemer: "USER_ADDRESS" }
    orderBy: blockTimestamp
    orderDirection: desc
  ) {
    id
    collateralToken
    parentCollectionId
    conditionId
    indexSets
    payout
    blockTimestamp
    transactionHash
  }
}
```

### Get Payout Redemptions by Condition
```graphql
{
  payoutRedemptions(
    where: { conditionId: "CONDITION_ID" }
    orderBy: blockTimestamp
    orderDirection: desc
  ) {
    id
    redeemer
    collateralToken
    parentCollectionId
    indexSets
    payout
    blockTimestamp
  }
}
```

### Get Large Payout Redemptions
```graphql
{
  payoutRedemptions(
    where: { payout_gt: "1000000000000000000" }
    orderBy: payout
    orderDirection: desc
    first: 20
  ) {
    id
    redeemer
    conditionId
    payout
    blockTimestamp
    transactionHash
  }
}
```

## Neg Risk Adapter Payout Redemptions

### Get Recent Neg Risk Adapter Redemptions
```graphql
{
  negRiskAdapterPayoutRedemptions(
    orderBy: blockTimestamp
    orderDirection: desc
    first: 50
  ) {
    id
    redeemer
    conditionId
    amounts
    payout
    blockNumber
    blockTimestamp
    transactionHash
  }
}
```

### Get Neg Risk Adapter Redemptions by User
```graphql
{
  negRiskAdapterPayoutRedemptions(
    where: { redeemer: "USER_ADDRESS" }
    orderBy: blockTimestamp
    orderDirection: desc
  ) {
    id
    conditionId
    amounts
    payout
    blockTimestamp
    transactionHash
  }
}
```

### Get Neg Risk Adapter Redemptions by Condition
```graphql
{
  negRiskAdapterPayoutRedemptions(
    where: { conditionId: "CONDITION_ID" }
    orderBy: blockTimestamp
    orderDirection: desc
  ) {
    id
    redeemer
    amounts
    payout
    blockTimestamp
  }
}
```

### Get Large Neg Risk Adapter Redemptions
```graphql
{
  negRiskAdapterPayoutRedemptions(
    where: { payout_gt: "1000000000000000000" }
    orderBy: payout
    orderDirection: desc
    first: 20
  ) {
    id
    redeemer
    conditionId
    amounts
    payout
    blockTimestamp
  }
}
```

## Time-based Analysis

### Get Activity by Time Range
```graphql
{
  orderFilleds(
    where: {
      blockTimestamp_gte: "1704067200"
      blockTimestamp_lt: "1704153600"
    }
    orderBy: blockTimestamp
    first: 1000
  ) {
    id
    maker
    taker
    makerAssetId
    takerAssetId
    makerAmountFilled
    fee
    blockTimestamp
  }
}
```

### Get Recent Activity Summary
```graphql
{
  orderFilleds(
    where: { blockTimestamp_gt: "1704067200" }
    orderBy: blockTimestamp
    orderDirection: desc
    first: 10
  ) {
    id
    maker
    taker
    makerAmountFilled
    fee
    blockTimestamp
  }
  payoutRedemptions(
    where: { blockTimestamp_gt: "1704067200" }
    orderBy: blockTimestamp
    orderDirection: desc
    first: 10
  ) {
    id
    redeemer
    conditionId
    payout
    blockTimestamp
  }
}
```

## Cross-Market Analysis

### Get User's Complete Activity
```graphql
{
  orderFilleds(
    where: {
      or: [
        { maker: "USER_ADDRESS" }
        { taker: "USER_ADDRESS" }
      ]
    }
    orderBy: blockTimestamp
    orderDirection: desc
  ) {
    id
    orderHash
    maker
    taker
    makerAssetId
    takerAssetId
    makerAmountFilled
    takerAmountFilled
    fee
    blockTimestamp
  }
  payoutRedemptions(
    where: { redeemer: "USER_ADDRESS" }
    orderBy: blockTimestamp
    orderDirection: desc
  ) {
    id
    conditionId
    payout
    blockTimestamp
  }
}
```

### Get Asset Trading Activity
```graphql
{
  orderFilleds(
    where: {
      or: [
        { makerAssetId: "ASSET_ID" }
        { takerAssetId: "ASSET_ID" }
      ]
    }
    orderBy: blockTimestamp
    orderDirection: desc
  ) {
    id
    maker
    taker
    makerAssetId
    takerAssetId
    makerAmountFilled
    takerAmountFilled
    fee
    blockTimestamp
  }
}
```
