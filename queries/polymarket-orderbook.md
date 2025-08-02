# Polymarket Orderbook & Market Microstructure Subgraph Queries

**Repository**: [Polymarket-Orders](https://github.com/PaulieB14/Polymarket-Orders)  
**Graph Studio**: [Graph Studio Link](https://api.studio.thegraph.com/query/111767/polymarket-orderbook/version/latest)

## Core Market Data

### Get Markets with Basic Info
```graphql
{
  markets(first: 10, orderBy: totalVolume, orderDirection: desc) {
    id
    conditionId
    marketId
    question
    description
    endDate
    totalVolume
    totalTrades
    lastTradePrice
    lastTradeTimestamp
    createdAt
  }
}
```

### Get Market by Condition ID
```graphql
{
  markets(where: { conditionId: "CONDITION_ID" }) {
    id
    conditionId
    marketId
    question
    description
    endDate
    outcomeSlotCount
    totalVolume
    totalTrades
    lastTradePrice
    outcomes {
      id
      outcomeIndex
      name
      currentBid
      currentAsk
      lastTradePrice
      volume24h
    }
  }
}
```

## Live Orderbook Data

### Get Current Orderbook State
```graphql
{
  orderBooks(first: 10, orderBy: lastUpdate, orderDirection: desc) {
    id
    marketId
    bestBid
    bestAsk
    spread
    spreadPercentage
    totalBidDepth
    totalAskDepth
    bidDepthLevels
    askDepthLevels
    lastUpdate
  }
}
```

### Get Orderbook for Specific Market
```graphql
{
  orderBooks(where: { marketId: "MARKET_ID" }) {
    id
    marketId
    bestBid
    bestAsk
    spread
    spreadPercentage
    totalBidDepth
    totalAskDepth
    bidDepthLevels
    askDepthLevels
    activeOrders(first: 20, orderBy: price, orderDirection: desc) {
      id
      orderHash
      maker
      side
      price
      size
      remainingSize
      status
      createdAt
    }
    lastUpdate
  }
}
```

## Market Depth Analysis

### Get Market Depth Data
```graphql
{
  marketDepths(first: 10, orderBy: lastUpdate, orderDirection: desc) {
    id
    marketId
    totalBidDepth
    totalAskDepth
    depthImbalance
    depthAt1Percent
    depthAt5Percent
    depthAt10Percent
    bidLiquidity
    askLiquidity
    lastUpdate
  }
}
```

### Get Depth for Specific Market
```graphql
{
  marketDepths(where: { marketId: "MARKET_ID" }) {
    id
    marketId
    totalBidDepth
    totalAskDepth
    depthImbalance
    depthAt1Percent
    depthAt5Percent
    depthAt10Percent
    bidLiquidity
    askLiquidity
    lastUpdate
    createdAt
  }
}
```

### Get Price Levels for Market Depth Visualization
```graphql
{
  priceLevels(
    where: { marketId: "MARKET_ID" }
    orderBy: price
    orderDirection: asc
    first: 50
  ) {
    id
    marketId
    price
    side
    totalSize
    orderCount
    lastUpdate
  }
}
```

## Spread Analytics

### Get Current Spreads
```graphql
{
  spreads(first: 10, orderBy: lastUpdate, orderDirection: desc) {
    id
    marketId
    currentSpread
    currentSpreadPercentage
    minSpread
    maxSpread
    avgSpread
    spreadVolatility
    lastUpdate
  }
}
```

### Get Spread History for Market
```graphql
{
  spreads(where: { marketId: "MARKET_ID" }) {
    id
    marketId
    currentSpread
    currentSpreadPercentage
    minSpread
    maxSpread
    avgSpread
    spreadVolatility
    lastUpdate
    createdAt
  }
}
```

### Get Markets with Tight Spreads
```graphql
{
  spreads(
    where: { currentSpreadPercentage_lt: "0.02" }
    orderBy: currentSpreadPercentage
    orderDirection: asc
    first: 20
  ) {
    id
    marketId
    currentSpread
    currentSpreadPercentage
    avgSpread
    lastUpdate
  }
}
```

## Order Flow Analysis

### Get Current Order Flow
```graphql
{
  orderFlows(first: 10, orderBy: lastUpdate, orderDirection: desc) {
    id
    marketId
    buyFlow
    sellFlow
    netFlow
    buySellRatio
    orderFlowImbalance
    flow1Min
    flow5Min
    flow15Min
    lastUpdate
  }
}
```

### Get Order Flow for Specific Market
```graphql
{
  orderFlows(where: { marketId: "MARKET_ID" }) {
    id
    marketId
    buyFlow
    sellFlow
    netFlow
    buySellRatio
    orderFlowImbalance
    flow1Min
    flow5Min
    flow15Min
    lastUpdate
    createdAt
  }
}
```

### Get Markets with High Order Flow Imbalance
```graphql
{
  orderFlows(
    where: { orderFlowImbalance_gt: "0.3" }
    orderBy: orderFlowImbalance
    orderDirection: desc
    first: 20
  ) {
    id
    marketId
    buyFlow
    sellFlow
    netFlow
    orderFlowImbalance
    buySellRatio
    lastUpdate
  }
}
```

## Order Management

### Get Active Orders
```graphql
{
  orders(
    where: { status: ACTIVE }
    orderBy: createdAt
    orderDirection: desc
    first: 50
  ) {
    id
    orderHash
    marketId
    outcomeIndex
    maker
    side
    price
    size
    remainingSize
    status
    createdAt
    expiresAt
  }
}
```

### Get Orders by Maker
```graphql
{
  orders(
    where: { maker: "MAKER_ADDRESS" }
    orderBy: createdAt
    orderDirection: desc
    first: 100
  ) {
    id
    orderHash
    marketId
    outcomeIndex
    taker
    side
    price
    size
    remainingSize
    filledSize
    status
    createdAt
    filledAt
  }
}
```

### Get Large Orders
```graphql
{
  orders(
    where: { size_gt: "1000000000000000000" }
    orderBy: size
    orderDirection: desc
    first: 20
  ) {
    id
    orderHash
    marketId
    maker
    side
    price
    size
    remainingSize
    status
    createdAt
  }
}
```

## Trading Activity

### Get Recent Order Fills
```graphql
{
  orderFills(
    orderBy: timestamp
    orderDirection: desc
    first: 50
  ) {
    id
    marketId
    outcomeIndex
    maker
    taker
    price
    size
    fee
    timestamp
    blockNumber
    transactionHash
  }
}
```

### Get Order Fills for Market
```graphql
{
  orderFills(
    where: { marketId: "MARKET_ID" }
    orderBy: timestamp
    orderDirection: desc
    first: 100
  ) {
    id
    outcomeIndex
    maker
    taker
    price
    size
    fee
    timestamp
    transactionHash
  }
}
```

### Get Large Trades
```graphql
{
  orderFills(
    where: { size_gt: "1000000000000000000" }
    orderBy: size
    orderDirection: desc
    first: 20
  ) {
    id
    marketId
    maker
    taker
    price
    size
    fee
    timestamp
  }
}
```

### Get Trading Activity by User
```graphql
{
  orderFills(
    where: {
      or: [
        { maker: "USER_ADDRESS" }
        { taker: "USER_ADDRESS" }
      ]
    }
    orderBy: timestamp
    orderDirection: desc
    first: 100
  ) {
    id
    marketId
    maker
    taker
    price
    size
    fee
    timestamp
  }
}
```

## Order Cancellations

### Get Recent Cancellations
```graphql
{
  orderCancellations(
    orderBy: timestamp
    orderDirection: desc
    first: 50
  ) {
    id
    marketId
    outcomeIndex
    cancelledSize
    reason
    timestamp
    transactionHash
    order {
      id
      orderHash
      maker
      price
      size
    }
  }
}
```

### Get Cancellations for Market
```graphql
{
  orderCancellations(
    where: { marketId: "MARKET_ID" }
    orderBy: timestamp
    orderDirection: desc
  ) {
    id
    outcomeIndex
    cancelledSize
    reason
    timestamp
    order {
      orderHash
      maker
      price
      size
      side
    }
  }
}
```

## Market Microstructure Events

### Get Recent Market Events
```graphql
{
  marketMicrostructureEvents(
    orderBy: timestamp
    orderDirection: desc
    first: 50
  ) {
    id
    marketId
    eventType
    data
    timestamp
    blockNumber
    transactionHash
  }
}
```

### Get Events by Type
```graphql
{
  marketMicrostructureEvents(
    where: { eventType: SPREAD_WIDENING }
    orderBy: timestamp
    orderDirection: desc
    first: 20
  ) {
    id
    marketId
    eventType
    data
    timestamp
    transactionHash
  }
}
```

### Get Events for Market
```graphql
{
  marketMicrostructureEvents(
    where: { marketId: "MARKET_ID" }
    orderBy: timestamp
    orderDirection: desc
  ) {
    id
    eventType
    data
    timestamp
    blockNumber
  }
}
```

## Advanced Analytics Queries

### Get Complete Market Overview
```graphql
{
  markets(where: { id: "MARKET_ID" }) {
    id
    conditionId
    marketId
    question
    description
    totalVolume
    totalTrades
    lastTradePrice
    outcomes {
      id
      outcomeIndex
      name
      currentBid
      currentAsk
      lastTradePrice
    }
    spread {
      currentSpread
      currentSpreadPercentage
      avgSpread
      spreadVolatility
    }
    marketDepth {
      totalBidDepth
      totalAskDepth
      depthImbalance
      depthAt1Percent
      depthAt5Percent
      depthAt10Percent
    }
    orderFlow {
      buyFlow
      sellFlow
      netFlow
      orderFlowImbalance
      buySellRatio
    }
  }
}
```

### Get Market Liquidity Analysis
```graphql
{
  markets(first: 20, orderBy: totalVolume, orderDirection: desc) {
    id
    marketId
    question
    totalVolume
    spread {
      currentSpreadPercentage
      avgSpread
    }
    marketDepth {
      totalBidDepth
      totalAskDepth
      bidLiquidity
      askLiquidity
      depthAt5Percent
    }
    orderFlow {
      orderFlowImbalance
      buySellRatio
    }
  }
}
```

### Get Trading Intensity Metrics
```graphql
{
  markets(first: 10) {
    id
    marketId
    question
    totalTrades
    orderFlow {
      flow1Min
      flow5Min
      flow15Min
      netFlow
    }
  }
  orderFills(
    where: { timestamp_gt: "TIMESTAMP_1_HOUR_AGO" }
    orderBy: timestamp
    orderDirection: desc
  ) {
    id
    marketId
    size
    timestamp
  }
}
```

## Time-based Analysis

### Get Activity in Time Range
```graphql
{
  orderFills(
    where: {
      timestamp_gte: "START_TIMESTAMP"
      timestamp_lt: "END_TIMESTAMP"
    }
    orderBy: timestamp
    first: 1000
  ) {
    id
    marketId
    maker
    taker
    price
    size
    fee
    timestamp
  }
}
```

### Get Recent Market Activity Summary
```graphql
{
  orderFills(
    where: { timestamp_gt: "RECENT_TIMESTAMP" }
    orderBy: timestamp
    orderDirection: desc
    first: 20
  ) {
    id
    marketId
    price
    size
    timestamp
  }
  orderCancellations(
    where: { timestamp_gt: "RECENT_TIMESTAMP" }
    orderBy: timestamp
    orderDirection: desc
    first: 10
  ) {
    id
    marketId
    cancelledSize
    timestamp
  }
}
```

## Multi-Market Comparison

### Compare Market Liquidity
```graphql
{
  markets(first: 10, orderBy: totalVolume, orderDirection: desc) {
    id
    marketId
    question
    totalVolume
    spread {
      currentSpreadPercentage
    }
    marketDepth {
      totalBidDepth
      totalAskDepth
      bidLiquidity
      askLiquidity
    }
  }
}
```

### Compare Order Flow Across Markets
```graphql
{
  orderFlows(first: 20, orderBy: lastUpdate, orderDirection: desc) {
    id
    marketId
    buyFlow
    sellFlow
    netFlow
    orderFlowImbalance
    buySellRatio
    lastUpdate
  }
}
```

## Real-time Monitoring Queries

### Get Markets Requiring Attention
```graphql
{
  # Markets with wide spreads
  spreads(
    where: { currentSpreadPercentage_gt: "0.05" }
    orderBy: currentSpreadPercentage
    orderDirection: desc
    first: 10
  ) {
    id
    marketId
    currentSpreadPercentage
    lastUpdate
  }
  
  # Markets with high order flow imbalance
  orderFlows(
    where: { orderFlowImbalance_gt: "0.4" }
    orderBy: orderFlowImbalance
    orderDirection: desc
    first: 10
  ) {
    id
    marketId
    orderFlowImbalance
    netFlow
    lastUpdate
  }
}
```

### Get Orderbook Health Check
```graphql
{
  orderBooks(first: 20, orderBy: lastUpdate, orderDirection: desc) {
    id
    marketId
    spread
    spreadPercentage
    totalBidDepth
    totalAskDepth
    bidDepthLevels
    askDepthLevels
    lastUpdate
  }
}
```
