# Polymarket Subgraph Analytics

Comprehensive query examples for analyzing Polymarket data using The Graph Protocol subgraphs.

## Available Subgraphs

### 1. Polymarket Main
- **Subgraph ID**: `QmdyCguLEisTtQFveEkvMhTH7UzjyhnrF9kpvhYeG4QX8a`
- **Purpose**: Core Polymarket data including markets, positions, and basic trading activity
- **Network**: Polygon
- **Query Examples**: [queries/polymarket-main.md](queries/polymarket-main.md)

### 2. Polymarket Profit and Loss
- **Subgraph ID**: `QmZAYiMeZiWC7ZjdWepek7hy1jbcW3ngimBF9ibTiTtwQU`
- **Purpose**: User positions, PnL tracking, and realized profits/losses
- **Network**: Polygon
- **Query Examples**: [queries/polymarket-pnl.md](queries/polymarket-pnl.md)

### 3. Polymarket Activity Polygon
- **Subgraph ID**: `Qmf3qPUsfQ8et6E3QNBmuXXKqUJi91mo5zbsaTkQrSnMAP`
- **Purpose**: Split, merge, redemption activities and Neg Risk conversions
- **Network**: Polygon
- **Query Examples**: [queries/polymarket-activity.md](queries/polymarket-activity.md)

### 4. Polymarket Open Interest
- **Subgraph ID**: `QmbxydtB3MF2yNriAHhsrBmqTx44aaw44jjNFwZNWaW7R6`
- **Purpose**: Open interest tracking for markets and global metrics
- **Network**: Polygon
- **Query Examples**: [queries/polymarket-open-interest.md](queries/polymarket-open-interest.md)

### 5. Polymarket Order Filled Events - [Subgraph](https://thegraph.com/explorer/subgraphs/EZCTgSzLPuBSqQcuR3ifeiKHKBnpjHSNbYpty8Mnjm9D?view=Query&chain=arbitrum-one)
- **Subgraph ID**: `Qma5ngHGmgW8qea4nEXmFjNtJb9aXDwV75hWekpTKD1fYf`
- **Purpose**: Order fills, redemptions, and exchange events
- **Network**: Polygon
- **Query Examples**: [queries/polymarket-order-filled.md](queries/polymarket-order-filled.md)

## Quick Start

1. Choose the appropriate subgraph for your analysis needs
2. Browse the query examples in the `queries/` directory
3. Modify queries for your specific use case
4. Execute queries using The Graph's hosted service or decentralized network

## Query Execution

You can execute these queries using:
- [The Graph Explorer](https://thegraph.com/explorer/subgraphs)
- GraphQL clients like Apollo Client
- HTTP requests to subgraph endpoints
- Programming languages with GraphQL support

## Contributing

Feel free to add more query examples or improve existing ones by submitting a pull request.

## Resources

- [The Graph Documentation](https://thegraph.com/docs/)
- [GraphQL Documentation](https://graphql.org/learn/)
- [Polymarket Documentation](https://docs.polymarket.com/)
