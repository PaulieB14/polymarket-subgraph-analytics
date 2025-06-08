# Polymarket Subgraph Analytics

Comprehensive query examples for analyzing Polymarket data using The Graph Protocol subgraphs.

## Available Subgraphs

### 1. Polymarket Main - [Subgraph](https://thegraph.com/explorer/subgraphs/81Dm16JjuFSrqz813HysXoUPvzTwE7fsfPk2RTf66nyC?view=Query&chain=arbitrum-one)
- **Subgraph ID**: `QmdyCguLEisTtQFveEkvMhTH7UzjyhnrF9kpvhYeG4QX8a`
- **Purpose**: Core Polymarket data including markets, positions, and basic trading activity
- **Network**: Polygon
- **Query Examples**: [queries/polymarket-main.md](queries/polymarket-main.md)

### 2. Polymarket Profit and Loss - [Subgraph](https://thegraph.com/explorer/subgraphs/6c58N5U4MtQE2Y8njfVrrAfRykzfqajMGeTMEvMmskVz?view=Query&chain=arbitrum-one)
- **Subgraph ID**: `QmZAYiMeZiWC7ZjdWepek7hy1jbcW3ngimBF9ibTiTtwQU`
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

### 5. Polymarket Order Filled Events - [Subgraph](https://thegraph.com/explorer/subgraphs/EZCTgSzLPuBSqQcuR3ifeiKHKBnpjHSNbYpty8Mnjm9D?view=Query&chain=arbitrum-one)
- **Subgraph ID**: `Qma5ngHGmgW8qea4nEXmFjNtJb9aXDwV75hWekpTKD1fYf`
- **Purpose**: Order fills, redemptions, and exchange events
- **Network**: Polygon
- **Query Examples**: [queries/polymarket-order-filled.md](queries/polymarket-order-filled.md)

### 6. Polymarket Names - [Subgraph](https://thegraph.com/explorer/subgraphs/22CoTbEtpv6fURB6moTNfJPWNUPXtiFGRA8h1zajMha3?view=Curators&chain=arbitrum-one) 🆕
- **Subgraph ID**: `22CoTbEtpv6fURB6moTNfJPWNUPXtiFGRA8h1zajMha3`
- **IPFS Hash**: `QmP6hMoYTYx4dFGs2dYiNnUDsRZ4ybhH9N6C6G19tHQxku`
- **Purpose**: **Human-readable market titles and questions** - Provides the missing link between questionIDs and actual market descriptions
- **Network**: Arbitrum One
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

### Enhanced Data Transformation with Human-Readable Names

**NEW**: With the Polymarket Names subgraph, you can now easily get human-readable market titles without external API calls!

#### Market Card Example (Enhanced)
```javascript
// Enhanced market data transformation with readable names
function formatMarketDataWithNames(rawMarket, marketNames) {
  // Find matching market name from Names subgraph
  const marketName = marketNames.find(m => 
    m.questionID.toLowerCase() === rawMarket.conditions[0].toLowerCase()
  );
  
  return {
    id: rawMarket.id,
    volume: formatCurrency(rawMarket.scaledCollateralVolume),
    probability: formatProbability(rawMarket.outcomeTokenPrices),
    trades: rawMarket.tradesQuantity,
    status: getMarketStatus(rawMarket),
    // Now we have the actual market question!
    title: marketName?.question || "Market title not found",
    questionID: rawMarket.conditions[0]
  };
}

// Combined query to get markets with names
const combinedMarketQuery = `
  query GetMarketsWithNames {
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

// Helper functions remain the same
function formatCurrency(scaledAmount) {
  return `$${(scaledAmount / 1e6).toLocaleString()}`;
}

function formatProbability(prices) {
  if (!prices || prices.length === 0) return "N/A";
  const yesPrice = parseFloat(prices[0]);
  return `${(yesPrice * 100).toFixed(1)}%`;
}

function getMarketStatus(market) {
  if (market.resolutionTimestamp) return "Resolved";
  if (market.lastActiveDay > Date.now() / 1000 - 86400) return "Active";
  return "Inactive";
}
```

#### Cross-Subgraph Market Analytics
```javascript
// Example: Get top markets with full details including names
async function getTopMarketsWithDetails() {
  // 1. Get market data from main subgraph
  const marketsData = await querySubgraph(POLYMARKET_MAIN_SUBGRAPH, `
    query {
      fixedProductMarketMakers(first: 20, orderBy: scaledCollateralVolume, orderDirection: desc) {
        id
        conditions
        scaledCollateralVolume
        outcomeTokenPrices
        tradesQuantity
        lastActiveDay
      }
    }
  `);
  
  // 2. Extract questionIDs
  const questionIDs = marketsData.fixedProductMarketMakers
    .map(market => market.conditions[0]);
  
  // 3. Get human-readable names from Names subgraph
  const namesData = await querySubgraph(POLYMARKET_NAMES_SUBGRAPH, `
    query($questionIDs: [Bytes!]) {
      markets(where: { questionID_in: $questionIDs }) {
        questionID
        question
        creator
        timestamp
      }
    }
  `, { questionIDs });
  
  // 4. Combine the data
  return marketsData.fixedProductMarketMakers.map(market => {
    const nameInfo = namesData.markets.find(n => 
      n.questionID.toLowerCase() === market.conditions[0].toLowerCase()
    );
    
    return {
      ...market,
      question: nameInfo?.question || "Unknown Market",
      creator: nameInfo?.creator,
      createdAt: nameInfo?.timestamp
    };
  });
}
```

### Real-time Market Monitoring Dashboard
```javascript
// Example: Real-time dashboard with live market names
function MarketDashboard() {
  const [markets, setMarkets] = useState([]);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    async function loadMarkets() {
      try {
        const marketsWithNames = await getTopMarketsWithDetails();
        setMarkets(marketsWithNames);
      } catch (error) {
        console.error('Failed to load markets:', error);
      } finally {
        setLoading(false);
      }
    }
    
    loadMarkets();
    
    // Refresh every 5 minutes
    const interval = setInterval(loadMarkets, 5 * 60 * 1000);
    return () => clearInterval(interval);
  }, []);
  
  if (loading) return <div>Loading markets...</div>;
  
  return (
    <div className="market-dashboard">
      <h1>Top Polymarket Markets</h1>
      <div className="market-grid">
        {markets.map(market => (
          <div key={market.id} className="market-card">
            <h3>{market.question}</h3>
            <div className="market-stats">
              <span>Volume: {formatCurrency(market.scaledCollateralVolume)}</span>
              <span>Probability: {formatProbability(market.outcomeTokenPrices)}</span>
              <span>Trades: {market.tradesQuantity}</span>
            </div>
            <div className="market-meta">
              <small>Created: {new Date(market.createdAt * 1000).toLocaleDateString()}</small>
            </div>
          </div>
        ))}
      </div>
    </div>
  );
}
```

### Multi-Subgraph Analytics Queries
```javascript
// Example: Comprehensive market analysis using multiple subgraphs
async function getCompleteMarketAnalysis(questionID) {
  const [marketDetails, pnlData, activityData, openInterest, marketName] = await Promise.all([
    // Main market data
    querySubgraph(POLYMARKET_MAIN_SUBGRAPH, `
      query($condition: Bytes!) {
        fixedProductMarketMakers(where: { conditions_contains: [$condition] }) {
          id
          scaledCollateralVolume
          outcomeTokenPrices
          tradesQuantity
        }
      }
    `, { condition: questionID }),
    
    // PnL data
    querySubgraph(POLYMARKET_PNL_SUBGRAPH, `
      query($condition: Bytes!) {
        marketPositions(where: { market_: { conditions_contains: [$condition] } }) {
          user { id }
          realizedProfit
          netQuantity
        }
      }
    `, { condition: questionID }),
    
    // Activity data  
    querySubgraph(POLYMARKET_ACTIVITY_SUBGRAPH, `
      query($condition: Bytes!) {
        splitPositions(where: { condition: $condition }, first: 100, orderBy: timestamp, orderDirection: desc) {
          user
          amount
          timestamp
        }
      }
    `, { condition: questionID }),
    
    // Open interest
    querySubgraph(POLYMARKET_OPEN_INTEREST_SUBGRAPH, `
      query($condition: Bytes!) {
        marketDayData(where: { market_: { conditions_contains: [$condition] } }, orderBy: date, orderDirection: desc, first: 30) {
          date
          volume
          openInterest
        }
      }
    `, { condition: questionID }),
    
    // Human-readable name
    querySubgraph(POLYMARKET_NAMES_SUBGRAPH, `
      query($questionID: Bytes!) {
        markets(where: { questionID: $questionID }) {
          question
          creator
          timestamp
        }
      }
    `, { questionID })
  ]);
  
  return {
    question: marketName.markets[0]?.question || "Unknown Market",
    marketDetails: marketDetails.fixedProductMarketMakers[0],
    topTraders: pnlData.marketPositions
      .sort((a, b) => parseFloat(b.realizedProfit) - parseFloat(a.realizedProfit))
      .slice(0, 10),
    recentActivity: activityData.splitPositions,
    volumeHistory: openInterest.marketDayData,
    creator: marketName.markets[0]?.creator,
    createdAt: marketName.markets[0]?.timestamp
  };
}
```

### Key Benefits of the Names Subgraph Integration

- **No External API Dependencies**: Get market titles directly from The Graph
- **Real-time Updates**: Market names are indexed as they're created
- **Cross-subgraph Linking**: Use questionIDs to connect data across all Polymarket subgraphs
- **Enhanced User Experience**: Display actual market questions instead of cryptic IDs
- **Reduced Complexity**: Single GraphQL query instead of multiple API calls

### Tips for Using Multiple Subgraphs

- **Batch Queries**: Use GraphQL variables to query multiple markets at once
- **Cache Strategically**: Market names don't change once created, so cache them aggressively
- **Handle Missing Data**: Not all questionIDs may be present in the Names subgraph
- **Use Promise.all()**: Query multiple subgraphs in parallel for better performance
- **Error Handling**: Implement fallbacks when subgraphs are temporarily unavailable

## Contributing

Feel free to add more query examples or improve existing ones by submitting a pull request.

## Resources

- [The Graph Documentation](https://thegraph.com/docs/)
- [GraphQL Documentation](https://graphql.org/learn/)
- [Polymarket Documentation](https://docs.polymarket.com/)
- [Polymarket Names Subgraph](https://thegraph.com/explorer/subgraphs/22CoTbEtpv6fURB6moTNfJPWNUPXtiFGRA8h1zajMha3?view=Curators&chain=arbitrum-one)
