# Common GraphQL Patterns for Polymarket Analytics

This document provides common patterns and best practices for querying Polymarket subgraphs.

## Filtering Patterns

### Time-based Filtering
```graphql
# Get data from the last 24 hours (assuming current timestamp is 1704153600)
{
  transactions(
    where: { timestamp_gt: "1704067200" }
    orderBy: timestamp
    orderDirection: desc
  ) {
    # fields...
  }
}

# Get data between specific dates
{
  transactions(
    where: {
      timestamp_gte: "1704067200"  # Start date
      timestamp_lt: "1704153600"   # End date
    }
  ) {
    # fields...
  }
}
```

### Amount-based Filtering
```graphql
# Large trades only
{
  transactions(
    where: { tradeAmount_gt: "1000000000" }  # > 1000 USDC (6 decimals)
  ) {
    # fields...
  }
}

# Trades within a range
{
  transactions(
    where: {
      tradeAmount_gte: "100000000"   # >= 100 USDC
      tradeAmount_lte: "1000000000"  # <= 1000 USDC
    }
  ) {
    # fields...
  }
}
```

### User-specific Filtering
```graphql
# All activity for a specific user
{
  account(id: "0x...") {
    transactions {
      # fields...
    }
    marketPositions {
      # fields...
    }
    fpmmPoolMemberships {
      # fields...
    }
  }
}
```

## Pagination Patterns

### Basic Pagination
```graphql
# First page
{
  transactions(first: 100, skip: 0) {
    # fields...
  }
}

# Second page
{
  transactions(first: 100, skip: 100) {
    # fields...
  }
}
```

### Cursor-based Pagination
```graphql
# First query
{
  transactions(first: 100, orderBy: timestamp, orderDirection: desc) {
    id
    timestamp
    # other fields...
  }
}

# Subsequent query using the last timestamp from previous result
{
  transactions(
    first: 100
    where: { timestamp_lt: "LAST_TIMESTAMP" }
    orderBy: timestamp
    orderDirection: desc
  ) {
    # fields...
  }
}
```

## Aggregation Patterns

### Volume Analysis
```graphql
# Top markets by volume
{
  fixedProductMarketMakers(
    first: 10
    orderBy: scaledCollateralVolume
    orderDirection: desc
  ) {
    id
    scaledCollateralVolume
    tradesQuantity
  }
}

# Top users by volume
{
  accounts(
    first: 20
    orderBy: scaledCollateralVolume
    orderDirection: desc
  ) {
    id
    scaledCollateralVolume
    numTrades
  }
}
```

### Profit Analysis
```graphql
# Most profitable positions
{
  userPositions(
    where: { realizedPnl_gt: "0" }
    orderBy: realizedPnl
    orderDirection: desc
    first: 50
  ) {
    user
    tokenId
    realizedPnl
    amount
  }
}

# Biggest losses
{
  userPositions(
    where: { realizedPnl_lt: "0" }
    orderBy: realizedPnl
    orderDirection: asc
    first: 50
  ) {
    user
    tokenId
    realizedPnl
    amount
  }
}
```

## Complex Queries

### Multi-entity Analysis
```graphql
{
  # Global metrics
  globals {
    numTraders
    scaledCollateralVolume
    tradesQuantity
  }
  
  # Top markets
  fixedProductMarketMakers(
    first: 5
    orderBy: scaledCollateralVolume
    orderDirection: desc
  ) {
    id
    scaledCollateralVolume
    outcomeTokenPrices
  }
  
  # Recent activity
  transactions(
    first: 10
    orderBy: timestamp
    orderDirection: desc
  ) {
    type
    tradeAmount
    timestamp
    user { id }
  }
}
```

### Cross-subgraph Analysis
When you need data from multiple subgraphs, execute separate queries:

```javascript
// Query 1: Main subgraph for trading data
const tradingData = await graphqlClient.query({
  query: gql`
    {
      transactions(where: { user: "${userAddress}" }) {
        id
        type
        tradeAmount
        market { id }
      }
    }
  `
});

// Query 2: PnL subgraph for profit data
const pnlData = await graphqlClient.query({
  query: gql`
    {
      userPositions(where: { user: "${userAddress}" }) {
        tokenId
        realizedPnl
        amount
      }
    }
  `
});
```

## Performance Optimization

### Limit Field Selection
```graphql
# Good - only select needed fields
{
  transactions(first: 100) {
    id
    type
    tradeAmount
    timestamp
  }
}

# Avoid - selecting all nested fields
{
  transactions(first: 100) {
    id
    type
    tradeAmount
    timestamp
    user {
      id
      creationTimestamp
      lastSeenTimestamp
      scaledCollateralVolume
      # ... many more fields
    }
  }
}
```

### Use Appropriate Limits
```graphql
# For dashboards - smaller limits
{
  transactions(first: 20, orderBy: timestamp, orderDirection: desc) {
    # fields...
  }
}

# For analysis - larger limits but paginate
{
  transactions(first: 1000, skip: 0) {
    # fields...
  }
}
```

## Common Use Cases

### Market Overview Dashboard
```graphql
{
  globals {
    numConditions
    numTraders
    scaledCollateralVolume
  }
  
  fixedProductMarketMakers(
    first: 10
    orderBy: scaledCollateralVolume
    orderDirection: desc
  ) {
    id
    scaledCollateralVolume
    outcomeTokenPrices
    tradesQuantity
    conditions
  }
}
```

### User Portfolio Analysis
```graphql
{
  account(id: "USER_ADDRESS") {
    scaledCollateralVolume
    numTrades
    scaledProfit
    
    marketPositions(
      orderBy: netValue
      orderDirection: desc
      first: 20
    ) {
      market {
        id
        outcomeIndex
      }
      netQuantity
      netValue
    }
  }
}
```

### Market Activity Feed
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
    tradeAmount
    user { id }
    market {
      id
      conditions
    }
  }
}
```

### Whale Watching
```graphql
{
  transactions(
    where: { tradeAmount_gt: "10000000000" }  # > 10,000 USDC  
    orderBy: tradeAmount
    orderDirection: desc
    first: 20
  ) {
    id
    type
    tradeAmount
    timestamp
    user {
      id
      scaledCollateralVolume
    }
    market { id }
  }
}
```

## Error Handling

### Common Errors and Solutions

1. **Query timeout**: Reduce the `first` parameter or add more specific filters
2. **Invalid field**: Check the schema documentation for available fields
3. **Invalid filter**: Ensure filter values match the field type (String, BigInt, etc.)

### Example Error Handling in JavaScript
```javascript
try {
  const result = await graphqlClient.query({
    query: POLYMARKET_QUERY,
    variables: { userAddress }
  });
  return result.data;
} catch (error) {
  if (error.message.includes('timeout')) {
    // Retry with smaller limit
    console.log('Query timeout, retrying with smaller dataset...');
  } else {
    console.error('GraphQL error:', error);
  }
}
```
