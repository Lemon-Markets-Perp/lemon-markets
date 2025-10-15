# 🍋 Lemon Markets - Decentralized Perpetual Futures Trading Platform

A next-generation decentralized trading platform offering unlimited markets with up to 100x leverage. Trade any asset class from crypto to traditional markets with our innovative perpetual futures protocol.

## 🌟 Features

- **Unlimited Markets**: Trade any asset with perpetual futures contracts
- **High Leverage**: Up to 100x leverage on supported trading pairs
- **Multi-Chain Support**: Supports Base Chain and Binance Smart Chain (BSC)
- **Web3 Integration**: Secure wallet connections with MetaMask, WalletConnect, and more
- **Real-Time Trading**: Live price charts and instant trade execution
- **Position Management**: Advanced position tracking and portfolio management
- **Farcaster Mini App**: Native integration with Farcaster social platform
- **Mobile Optimized**: Responsive design for desktop and mobile trading

## 🏗️ Complete System Architecture

Lemon Markets is a comprehensive decentralized perpetual futures trading ecosystem consisting of multiple interconnected components working together to provide a seamless trading experience.

### 📋 Architecture Overview

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend UI   │    │  Farcaster      │    │  Mobile Apps    │
│   (Next.js)     │    │  Mini App       │    │  (Future)       │
└─────────┬───────┘    └─────────┬───────┘    └─────────┬───────┘
          │                      │                      │
          └──────────────────────┼──────────────────────┘
                                 │
                    ┌─────────────▼───────────────┐
                    │        Web3 Layer           │
                    │   (Wagmi, Viem, RainbowKit) │
                    └─────────────┬───────────────┘
                                 │
          ┌──────────────────────┼──────────────────────┐
          │                      │                      │
   ┌──────▼──────┐    ┌─────────▼─────────┐    ┌──────▼──────┐
   │  Base Chain │    │   BSC Chain       │    │  Future     │
   │  (Primary)  │    │  (Secondary)      │    │  Chains     │
   └──────┬──────┘    └─────────┬─────────┘    └──────┬──────┘
          │                     │                     │
          └─────────────────────┼─────────────────────┘
                                │
                    ┌───────────▼────────────┐
                    │   Smart Contracts      │
                    │  SyntheticPerpetual    │
                    │      + USDC           │
                    └───────────┬────────────┘
                                │
                    ┌───────────▼────────────┐
                    │      Subgraph          │
                    │   (The Graph)          │
                    │  Position Indexing     │
                    └───────────┬────────────┘
                                │
          ┌─────────────────────┼─────────────────────┐
          │                     │                     │
   ┌──────▼──────┐    ┌─────────▼─────────┐    ┌──────▼──────┐
   │   Oracle    │    │  Liquidation      │    │   Price     │
   │   Service   │    │      Bot          │    │  Feeds      │
   │ (LemonAPI)  │    │  (Monitoring)     │    │ (External)  │
   └─────────────┘    └───────────────────┘    └─────────────┘
```

## 🎯 Core Components Deep Dive

### 1. Frontend Application (Next.js)

**Technology Stack:**
- **Framework**: Next.js 15 with App Router
- **Language**: TypeScript for type safety
- **Styling**: Tailwind CSS with custom animations
- **State Management**: React Query + Context API
- **Web3 Integration**: Wagmi v2 + Viem + RainbowKit

**Key Features:**
- Real-time trading interface with live charts
- Multi-chain position management
- Responsive design for desktop and mobile
- Advanced order types and risk management
- Social trading integration

**Core Modules:**
```typescript
src/
├── app/                    # Next.js App Router
│   ├── api/               # Backend API routes
│   │   ├── position/      # Position CRUD operations
│   │   ├── positions/     # Portfolio management
│   │   └── search/        # Token search
│   ├── perp/              # Main trading interface
│   └── positions/         # Position history
├── components/            # React components
│   ├── trading/          # Trading-specific UI
│   │   ├── TradingInterface.tsx
│   │   ├── ChartSection.tsx
│   │   ├── PositionsTable.tsx
│   │   └── TradingPanel.tsx
│   └── ui/               # Reusable components
├── hooks/                # Custom React hooks
│   ├── useUserPositions.ts
│   ├── useMiniAppActions.ts
│   └── useTransactionToast.ts
└── lib/                  # Core utilities
    ├── contracts.ts      # Smart contract ABIs
    ├── position-api.ts   # Position management
    └── token-price-service.ts
```

### 2. Smart Contract Layer

**Primary Contract**: `SyntheticPerpetualContract`
- **Address**: `0xe9Cf759F88b5870cd7EdA3c0Fb11526BA963D8E5`
- **Collateral Token**: USDC (`0x109DAeF13DcA732B32B19b32f45A46E29d320C44`)

**Contract Features:**
```solidity
// Key Functions
function openPosition(
    string memory tokenSymbol,
    uint256 margin,
    uint256 leverage,
    bool isLong,
    uint256 liquidationPrice
) external;

function closePosition(uint256 positionId) external;
function modifyPosition(uint256 positionId, uint256 newMargin) external;
function liquidatePosition(uint256 positionId) external;
```

**Event Emissions:**
- `PositionOpened`: New position creation
- `PositionClosed`: Position termination  
- `PositionModified`: Margin adjustments
- `PvPLiquidation`: Liquidation events

**Safety Features:**
- Liquidation threshold protection
- Admin signature verification (ECDSA)
- Collateral token validation
- Position state management

### 3. Subgraph Infrastructure (The Graph)

**Purpose**: Decentralized indexing of blockchain events for efficient data querying

**GraphQL Schema:**
```graphql
type Position @entity {
  id: ID!
  positionId: String!
  trader: String!
  tokenSymbol: String!
  margin: BigInt!
  leverage: BigInt!
  isLong: Boolean!
  entryPrice: BigInt!
  exitPrice: BigInt
  liquidationPrice: BigInt!
  status: String!
  finalPnl: BigInt
  openedAt: BigInt!
  lastUpdatedAt: BigInt!
  lastTransactionHash: String!
  lastBlockNumber: BigInt!
  lastBlockTimestamp: BigInt!
}

type VirtualMarket @entity {
  id: ID!
  marketId: String!
  realLiquidity: BigInt!
  totalLiquidity: BigInt!
  virtualLiquidity: BigInt!
  exists: Boolean!
  durationFeeRate: BigInt!
  createdTimestamp: BigInt!
}
```

**Query Examples:**
```typescript
// Fetch user positions
const POSITIONS_QUERY = `
  query GetPositions($trader: String!) {
    positions(where: {trader: $trader}) {
      positionId
      tokenSymbol
      margin
      leverage
      isLong
      entryPrice
      liquidationPrice
      status
      finalPnl
    }
  }
`;

// Fetch virtual markets
const VIRTUAL_MARKETS_QUERY = `
  query AllVirtualMarkets {
    virtualMarkets {
      marketId
      realLiquidity
      totalLiquidity
      virtualLiquidity
      durationFeeRate
    }
  }
`;
```

### 4. Liquidation Bot System

**Purpose**: Automated monitoring and liquidation of undercollateralized positions

**Architecture:**
```typescript
interface LiquidationBot {
  // Monitor positions approaching liquidation threshold
  monitorPositions(): Promise<void>;
  
  // Calculate position health
  calculatePositionHealth(position: Position): number;
  
  // Execute liquidation transaction
  liquidatePosition(positionId: string): Promise<TransactionReceipt>;
  
  // Risk management
  assessLiquidationRisk(position: Position): RiskLevel;
}
```

**Key Features:**
- Real-time position monitoring
- Automated liquidation execution
- Gas optimization strategies
- MEV protection mechanisms
- Risk assessment algorithms

**Monitoring Logic:**
```typescript
// Liquidation threshold calculation
const liquidationThreshold = (position.margin * LIQUIDATION_THRESHOLD) / 100;
const currentValue = position.margin + position.unrealizedPnl;

if (currentValue <= liquidationThreshold) {
  await executeLiquidation(position.id);
}
```

### 5. Oracle Service Integration

**Lemon Oracle Client**: Multi-chain price aggregation service

**Features:**
```typescript
class LemonSpotPriceClient {
  // Chain-specific price fetching
  async getPriceBase(symbol: string): Promise<TokenPrice>;
  async getPriceBSC(symbol: string): Promise<TokenPrice>;
  async getPrice(symbol: string, chainId?: number): Promise<TokenPrice>;
  
  // Batch price requests
  async getMultiplePrices(symbols: string[]): Promise<TokenPrice[]>;
  
  // Real-time price streams
  subscribeToPriceUpdates(symbol: string, callback: PriceCallback): void;
}
```

**Price Routing Logic:**
```typescript
// Chain-aware price fetching
export async function getTokenPrice(
  symbol: string, 
  chainId?: number
): Promise<number> {
  switch (chainId) {
    case 8453: // Base
      return await lemonClient.getPriceBase(symbol);
    case 56:   // BSC  
      return await lemonClient.getPriceBSC(symbol);
    default:
      return await lemonClient.getPrice(symbol, chainId);
  }
}
```

## 🚀 Farcaster Mini App Implementation

### Deep Integration Architecture

**1. SDK Integration:**
```typescript
// MiniApp Provider Setup
import { sdk } from "@farcaster/miniapp-sdk";

interface MiniAppContextData {
  user: {
    fid: number;
    username?: string;
    displayName?: string;
    pfpUrl?: string;
  };
  client: {
    platformType?: "web" | "mobile";
    clientFid: number;
    added: boolean;
    safeAreaInsets?: SafeAreaInsets;
  };
}

// Context provider for app-wide Mini App state
export function MiniAppProvider({ children }) {
  const [isMiniApp, setIsMiniApp] = useState(false);
  const [context, setContext] = useState<MiniAppContextData>();
  
  useEffect(() => {
    async function initMiniApp() {
      const inMiniApp = await sdk.isInMiniApp();
      if (inMiniApp) {
        const ctx = await sdk.context;
        setContext(ctx);
        await sdk.actions.ready(); // Signal app ready
      }
    }
    initMiniApp();
  }, []);
}
```

**2. Social Trading Features:**
```typescript
// Trading actions with social integration
export function useMiniAppActions() {
  const { isMiniApp } = useMiniApp();
  
  const shareTradeResult = useCallback(async (trade: TradeResult) => {
    if (!isMiniApp) return;
    
    const castText = `🚀 Just ${trade.type === 'long' ? 'went long' : 'shorted'} ${trade.symbol} 
    💰 PnL: ${trade.pnl > 0 ? '+' : ''}${trade.pnl.toFixed(2)} USDC
    📊 ${trade.leverage}x leverage`;
    
    await sdk.actions.composeCast({
      text: castText,
      embeds: [`${window.location.origin}/positions/${trade.id}`]
    });
  }, [isMiniApp]);
  
  const addToCollection = useCallback(async () => {
    await sdk.actions.addMiniApp();
  }, []);
}
```

**3. Manifest Configuration:**
```json
// public/.well-known/farcaster.json
{
  "schemaVersion": "1.0.0",
  "name": "Lemon Markets",
  "description": "Decentralized perpetual futures trading with unlimited markets",
  "icon": "https://lemon-perp.com/icon.png",
  "homeUrl": "https://lemon-perp.com",
  "embedUrls": [
    "https://lemon-perp.com/positions/*",
    "https://lemon-perp.com/trending",
    "https://lemon-perp.com/perp"
  ],
  "capabilities": [
    "composeCast",
    "addMiniApp"
  ]
}
```

**4. User Experience Enhancements:**
- **Native User Integration**: Access to Farcaster user profile and social graph
- **Seamless Sharing**: One-click trade result sharing to Farcaster timeline
- **Collection Integration**: Users can add Lemon Markets to their Mini App collection
- **Context Awareness**: App behavior adapts based on Mini App vs web context
- **Safe Area Handling**: Proper UI adaptation for mobile Farcaster clients

**5. Development Workflow:**
```bash
# Local development with Mini App testing
npm run dev

# Expose local server for Mini App testing
ngrok http 3000

# Test in Farcaster Mini App preview
# https://farcaster.xyz/~/developers/mini-apps/preview?url=YOUR_NGROK_URL
```

## 🔄 Data Flow Architecture

### Real-time Position Management

1. **Position Creation:**
   ```
   Frontend → Smart Contract → Blockchain → Subgraph → Position API → UI Update
   ```

2. **Price Updates:**
   ```
   Oracle Service → Price Cache → WebSocket → Frontend → Real-time PnL
   ```

3. **Liquidation Flow:**
   ```
   Bot Monitor → Risk Assessment → Liquidation TX → Event Emission → Subgraph Update
   ```

### Multi-Chain Synchronization

- **Chain Detection**: Automatic network identification and switching
- **State Synchronization**: Cross-chain position state management  
- **Price Coordination**: Chain-specific oracle routing
- **Transaction Routing**: Network-aware contract interactions

This comprehensive architecture ensures scalable, reliable, and user-friendly perpetual futures trading across multiple blockchain networks while maintaining decentralization and security principles.

## 🏗️ Technical Architecture
