# Polymarket Subgraph Analytics

Comprehensive query examples for analyzing Polymarket data using The Graph Protocol subgraphs.

## Available Subgraphs

### 1. Polymarket Main - [Subgraph](https://thegraph.com/explorer/subgraphs/81Dm16JjuFSrqz813HysXoUPvzTwE7fsfPk2RTf66nyC?view=Query&chain=arbitrum-one)
- **Subgraph ID**: `QmdyCguLEisTtQFveEkvMhTH7UzjyhnrF9kpvhYeG4QX8a`
- **Purpose**: Core Polymarket data including markets, positions, and basic trading activity
- **Network**: Polygon
- **Query Examples**: [queries/polymarket-main.md](queries/polymarket-main.md)

### 2. Polymarket Profit and Loss - [Subgraph](https://thegraph.com/explorer/subgraphs/6c58N5U4MtQE2Y8njfVrrAfRykzfqajMGeTMEvMmskVz?view=Query&chain=arbitrum-one)
- **Subgraph ID**: `QmZAYiMeZiWC7ZjdWepek7hy1jbcW3ngimBF9kpvhYeG4QX8a`
- **Purpose**: User positions, PnL tracking, and realized profits/losses
- **Network**: Polygon
- **Query Examples**: [queries/polymarket-pnl.md](queries/polymarket-pnl.md)

### 3. Polymarket Activity Polygon - [Subgraph](https://thegraph.com/explorer/subgraphs/Bx1W4S7kDVxs9gC3s2G6DS8kdNBJNVhMviCtin2DiBp?view=Query&chain=arbitrum-one)
- **Subgraph ID**: `Qmf3qPUsfQ8et6E3QNBmuXXKqUJi91mo5zbsaTkQrSnMAP`
- **Purpose**: Split, merge, redemption activities and Neg Risk conversions
- **Network**: Polygon
- **Query Examples**: [queries/polymarket-activity.md](queries/polymarket-activity.md)

### 4. Polymarket Open Interest - [Subgraph](https://thegraph.com/explorer/subgraphs/ELaW6RtkbmYNmMMU6hEPsghG9Ko3EXSmiRkH855M4qfF?view=Query&chain=arbitrum-one)
- **Subgraph ID**: `QmbxydtB3MF2yNriAHhsrBmqTx44aaw44jjNFwZNWaW7R6`
- **Purpose**: Open interest tracking for markets and global metrics
- **Network**: Polygon
- **Query Examples**: [queries/polymarket-open-interest.md](queries/polymarket-open-interest.md)

### 5. Polymarket Orderbook & Market Microstructure 🆕
- **Repository**: [Polymarket-Orders](https://github.com/PaulieB14/Polymarket-Orders)
- **Graph Studio**: [Graph Studio Link](https://api.studio.thegraph.com/query/111767/polymarket-orderbook/version/latest)
- **Purpose**: **Advanced orderbook analytics and market microstructure data** - Real-time orderbook depth, bid-ask spreads, liquidity analysis, and trading flow metrics
- **Network**: Polygon
- **Key Features**:
  - Real-time orderbook state and depth tracking
  - Market microstructure metrics (spreads, liquidity, order flow)
  - Trading activity analysis with price impact metrics
  - Multi-level market depth visualization
  - Order execution and cancellation tracking
  - Buy/sell flow imbalance detection
- **Query Examples**: [queries/polymarket-orderbook.md](queries/polymarket-orderbook.md)

### 6. Polymarket Names - [Subgraph](https://thegraph.com/explorer/subgraphs/22CoTbEtpv6fURB6moTNfJPWNUPXtiFGRA8h1zajMha3?view=Curators&chain=arbitrum-one)
- **Subgraph ID**: `22CoTbEtpv6fURB6moTNfJPWNUPXtiFGRA8h1zajMha3`
- **IPFS Hash**: `QmP6hMoYTYx4dFGs2dYiNnUDsRZ4ybhH9N6C6G19tHQxku`
- **Purpose**: **Human-readable market titles and questions** - Provides the missing link between questionIDs and actual market descriptions
- **Network**: Polygon
- **Key Features**:
  - Market questions in plain English
  - Question initialization and resolution tracking
  - Links questionIDs from other subgraphs to readable titles
  - Real-time market metadata updates
- **Query Examples**: [queries/polymarket-names.md](queries/polymarket-names.md)

## Quick Start

1. Choose the appropriate subgraph for your analysis needs
2. Browse the query examples in the `queries/` directory
3. Modify queries for your specific use case
4. Execute queries using The Graph's decentralized network

## Query Execution

You can execute these queries using:
- [The Graph Explorer](https://thegraph.com/explorer/subgraphs)
- GraphQL clients like Apollo Client
- HTTP requests to subgraph endpoints
- Programming languages with GraphQL support

## Dashboard Integration

### Enhanced Data Transformation with Human-Readable Names and Orderbook Data

**NEW**: With the Polymarket Names subgraph and the advanced Orderbook & Market Microstructure subgraph, you can now build comprehensive trading dashboards with human-readable market titles and real-time market depth data!

#### Advanced Market Card with Orderbook Data
```javascript
// Enhanced market data transformation with names and orderbook depth
function formatAdvancedMarketData(rawMarket, marketNames, orderbookData) {
  // Find matching market name from Names subgraph
  const marketName = marketNames.find(m => 
    m.questionID.toLowerCase() === rawMarket.conditions[0].toLowerCase()
  );
  
  // Find matching orderbook data
  const orderbook = orderbookData.find(ob => 
    ob.marketId === rawMarket.conditions[0]
  );
  
  return {
    id: rawMarket.id,
    volume: formatCurrency(rawMarket.scaledCollateralVolume),
    probability: formatProbability(rawMarket.outcomeTokenPrices),
    trades: rawMarket.tradesQuantity,
    status: getMarketStatus(rawMarket),
    title: marketName?.question || "Market title not found",
    questionID: rawMarket.conditions[0],
    // New orderbook metrics
    spread: orderbook?.currentSpread || "N/A",
    spreadPercentage: orderbook?.currentSpreadPercentage || "N/A",
    bidDepth: orderbook?.totalBidDepth || "0",
    askDepth: orderbook?.totalAskDepth || "0",
    lastTradePrice: orderbook?.lastTradePrice || "N/A"
  };
}

// Multi-subgraph query for comprehensive market data
const comprehensiveMarketQuery = `
  query GetCompleteMarketData {
    # From Polymarket Main subgraph
    fixedProductMarketMakers(first: 10, orderBy: scaledCollateralVolume, orderDirection: desc) {
      id
      conditions
      scaledCollateralVolume
      outcomeTokenPrices
      tradesQuantity
    }
  }
`;

const orderbookQuery = `
  query GetOrderbookData($marketIds: [String!]) {
    orderBooks(where: { marketId_in: $marketIds }) {
      id
      marketId
      totalBidDepth
      totalAskDepth
      bidDepthLevels
      askDepthLevels
      lastUpdate
    }
    spreads(where: { marketId_in: $marketIds }) {
      id
      marketId
      currentSpread
      currentSpreadPercentage
      avgSpread
      lastUpdate
    }
    orderFills(
      where: { marketId_in: $marketIds }
      orderBy: timestamp
      orderDirection: desc
      first: 1
    ) {
      marketId
      price
      timestamp
    }
  }
`;

const namesQuery = `
  query GetMarketNames($questionIDs: [Bytes!]) {
    markets(where: { questionID_in: $questionIDs }) {
      questionID
      question
      creator
      timestamp
    }
  }
`;
```

### Key Benefits of the Advanced Orderbook Subgraph Integration

- **Real-time Market Depth**: Monitor bid-ask depth and liquidity in real-time
- **Market Microstructure Analysis**: Advanced metrics for spread analysis and order flow
- **Trading Strategy Insights**: Price impact analysis and liquidity scoring
- **Professional Trading Tools**: Order flow imbalance detection and market efficiency metrics
- **Enhanced User Experience**: Complete market picture with names, prices, and depth data
- **Cross-subgraph Analytics**: Combine fundamental data with advanced trading metrics

### Tips for Using the Orderbook Subgraph

- **Real-time Updates**: Query frequently for orderbook data as it changes rapidly
- **Depth Analysis**: Use market depth queries for impact analysis before large trades
- **Flow Monitoring**: Track order flow imbalances for potential price movement signals
- **Spread Tracking**: Monitor spreads for market efficiency and trading cost analysis
- **Liquidity Scoring**: Use liquidity scores to assess market quality and tradability
- **Historical Analysis**: Combine with other subgraphs for comprehensive market research

## Contributing

Feel free to add more query examples or improve existing ones by submitting a pull request.

## Resources

- [The Graph Documentation](https://thegraph.com/docs/)
- [GraphQL Documentation](https://graphql.org/learn/)
- [Polymarket Documentation](https://docs.polymarket.com/)
- [Polymarket Names Subgraph](https://thegraph.com/explorer/subgraphs/22CoTbEtpv6fURB6moTNfJPWNUPXtiFGRA8h1zajMha3?view=Curators&chain=arbitrum-one)
- [Polymarket Orderbook Repository](https://github.com/PaulieB14/Polymarket-Orders)
