# Polymarket Names Subgraph Queries

**Subgraph ID**: `22CoTbEtpv6fURB6moTNfJPWNUPXtiFGRA8h1zajMha3`  
**IPFS Hash**: `QmP6hMoYTYx4dFGs2dYiNnUDsRZ4ybhH9N6C6G19tHQxku`  
**Network**: Arbitrum One  
**Purpose**: Human-readable market titles and questions for Polymarket analytics

## Overview

The Polymarket Names subgraph provides the crucial missing piece for building user-friendly Polymarket applications: **human-readable market titles and questions**. Instead of displaying cryptic questionIDs, you can now show users exactly what each market is about.

### Key Entities

- **Market**: Core market information with human-readable questions
- **QuestionInitialized**: Event data when markets are created  
- **QuestionResolved**: Resolution data with payouts
- **PriceRequest**: Oracle price request information

## Basic Queries

### 1. Get Recent Markets with Human-Readable Titles

```graphql
query GetRecentMarkets {
  markets(first: 10, orderBy: timestamp, orderDirection: desc) {
    id
    questionID
    question
    creator
    timestamp
    blockNumber
    rewardToken
    reward
  }
}
```

### 2. Find Market by Question ID

```graphql
query GetMarketByQuestionID($questionID: Bytes!) {
  markets(where: { questionID: $questionID }) {
    id
    questionID
    question
    creator
    timestamp
    ancillaryData
    rewardToken
    reward
    proposalBond
    liveness
  }
}
```

### 3. Search Markets by Keyword

```graphql
query SearchMarkets($keyword: String!) {
  markets(where: { question_contains_nocase: $keyword }, first: 20) {
    id
    questionID
    question
    creator
    timestamp
  }
}
```

## Advanced Queries

### 4. Get Markets by Creator

```graphql
query GetMarketsByCreator($creator: Bytes!) {
  markets(where: { creator: $creator }, orderBy: timestamp, orderDirection: desc) {
    id
    questionID
    question
    timestamp
    reward
    proposalBond
  }
}
```

### 5. Get Question Initialization Events

```graphql
query GetQuestionInitializations {
  questionInitializeds(first: 20, orderBy: blockTimestamp, orderDirection: desc) {
    id
    questionID
    question
    creator
    requestTimestamp
    blockTimestamp
    rewardToken
    reward
    proposalBond
    ancillaryData
  }
}
```

### 6. Get Resolved Markets

```graphql
query GetResolvedMarkets {
  questionResolveds(first: 10, orderBy: blockTimestamp, orderDirection: desc) {
    id
    questionID
    settledPrice
    payouts
    blockTimestamp
    transactionHash
  }
}
```

### 7. Get Markets with High Rewards

```graphql
query GetHighRewardMarkets($minReward: BigInt!) {
  markets(where: { reward_gte: $minReward }, orderBy: reward, orderDirection: desc) {
    id
    questionID
    question
    creator
    reward
    timestamp
  }
}
```

## Cross-Subgraph Integration

### 8. Combine with Main Subgraph Data

```javascript
// First query the main subgraph for market data
const mainSubgraphQuery = `
  query GetTopMarkets {
    fixedProductMarketMakers(first: 10, orderBy: scaledCollateralVolume, orderDirection: desc) {
      id
      conditions
      scaledCollateralVolume
      outcomeTokenPrices
      tradesQuantity
    }
  }
`;

// Then query the names subgraph for titles
const namesSubgraphQuery = `
  query GetMarketNames($questionIDs: [Bytes!]) {
    markets(where: { questionID_in: $questionIDs }) {
      questionID
      question
      creator
      timestamp
    }
  }
`;

// Combine the results
function combineMarketData(mainData, namesData) {
  return mainData.fixedProductMarketMakers.map(market => {
    const nameInfo = namesData.markets.find(n => 
      n.questionID.toLowerCase() === market.conditions[0].toLowerCase()
    );
    
    return {
      id: market.id,
      question: nameInfo?.question || "Unknown Market",
      volume: market.scaledCollateralVolume,
      probability: market.outcomeTokenPrices[0],
      trades: market.tradesQuantity,
      creator: nameInfo?.creator,
      createdAt: nameInfo?.timestamp
    };
  });
}
```

### 9. Market Analytics with Names

```graphql
query GetBitcoinMarkets {
  markets(where: { question_contains_nocase: "bitcoin" }, first: 50) {
    id
    questionID
    question
    creator
    timestamp
    reward
  }
  
  questionInitializeds(where: { question_contains_nocase: "bitcoin" }, first: 20) {
    questionID
    question
    blockTimestamp
    rewardToken
    reward
  }
}
```

### 10. Creator Analytics

```graphql
query GetCreatorStats($creator: Bytes!) {
  markets(where: { creator: $creator }) {
    id
    questionID
    question
    timestamp
    reward
  }
  
  questionInitializeds(where: { creator: $creator }) {
    id
    questionID
    question
    blockTimestamp
    reward
  }
}
```

## Utility Queries

### 11. Get Question IDs for External Matching

```graphql
query GetQuestionIDs {
  markets(first: 1000) {
    questionID
    question
  }
}
```

### 12. Recent Activity with Names

```graphql
query GetRecentActivity {
  questionInitializeds(first: 10, orderBy: blockTimestamp, orderDirection: desc) {
    id
    questionID
    question
    creator
    blockTimestamp
    transactionHash
  }
  
  questionResolveds(first: 5, orderBy: blockTimestamp, orderDirection: desc) {
    id
    questionID
    settledPrice
    payouts
    blockTimestamp
  }
}
```

### 13. Market Events Timeline

```graphql
query GetMarketTimeline($questionID: Bytes!) {
  questionInitializeds(where: { questionID: $questionID }) {
    id
    question
    creator
    blockTimestamp
    rewardToken
    reward
  }
  
  questionResolveds(where: { questionID: $questionID }) {
    id
    settledPrice
    payouts
    blockTimestamp
  }
  
  ancillaryDataUpdateds(where: { questionID: $questionID }) {
    id
    owner
    update
    blockTimestamp
  }
}
```

## Dashboard Integration Examples

### 14. Market Card Data

```javascript
// Perfect for populating market cards in a dashboard
const marketCardQuery = `
  query GetMarketCardData($questionIDs: [Bytes!]) {
    markets(where: { questionID_in: $questionIDs }) {
      questionID
      question
      creator
      timestamp
      reward
    }
  }
`;
```

### 15. Live Market Feed

```javascript
// For real-time market updates
const liveMarketFeed = `
  query GetLiveMarkets($lastTimestamp: BigInt!) {
    questionInitializeds(
      where: { blockTimestamp_gt: $lastTimestamp }
      orderBy: blockTimestamp
      orderDirection: desc
      first: 50
    ) {
      id
      questionID
      question
      creator
      blockTimestamp
      reward
    }
  }
`;
```

## Variables Examples

```javascript
// For searching Bitcoin markets
const variables = {
  keyword: "bitcoin"
};

// For specific creator
const variables = {
  creator: "0x91430cad2d3975766499717fa0d66a78d814e5c5"
};

// For specific question ID
const variables = {
  questionID: "0x71f8dbd344d612f63a2cdfab1549dc5dd69d8153ac122d8640fa505b24726e2c"
};

// For high reward markets (minimum 1000 tokens)
const variables = {
  minReward: "1000000000000000000000" // 1000 * 10^18
};
```

## Key Benefits

1. **Human-Readable Titles**: No more cryptic questionIDs in your UI
2. **Real-time Data**: Indexed directly from blockchain events
3. **Cross-Subgraph Linking**: Connect questionIDs across all Polymarket subgraphs
4. **Search Functionality**: Find markets by keywords in titles
5. **Creator Analytics**: Track market creation by specific addresses
6. **Event Tracking**: Full lifecycle from initialization to resolution

## Best Practices

- **Cache Market Names**: They don't change once created, so cache aggressively
- **Batch Queries**: Use `questionID_in` to get multiple market names at once
- **Handle Missing Data**: Not all questionIDs may be indexed yet
- **Use Variables**: Parameterize queries for reusability
- **Error Handling**: Implement fallbacks for when the subgraph is unavailable

## Common Patterns

```javascript
// Pattern 1: Get market data then enrich with names
const enrichMarketsWithNames = async (markets) => {
  const questionIDs = markets.map(m => m.conditions[0]);
  const names = await queryNamesSubgraph(questionIDs);
  return markets.map(market => ({
    ...market,
    question: names.find(n => n.questionID === market.conditions[0])?.question
  }));
};

// Pattern 2: Search and display
const searchAndDisplay = async (keyword) => {
  const results = await querySubgraph(`
    query($keyword: String!) {
      markets(where: { question_contains_nocase: $keyword }) {
        questionID
        question
        creator
        timestamp
      }
    }
  `, { keyword });
  
  return results.markets;
};
```

This subgraph is essential for building user-friendly Polymarket applications that display actual market questions instead of technical IDs!
