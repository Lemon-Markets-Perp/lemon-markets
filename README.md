# 🍋 Lemon Perp - Decentralized Perpetual Futures Trading Platform

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

## 🏗️ Technical Architecture

### Frontend Stack
- **Next.js 15**: Modern React framework with App Router
- **TypeScript**: Type-safe development
- **Tailwind CSS**: Utility-first styling with custom animations
- **Framer Motion**: Smooth animations and transitions
- **Radix UI**: Accessible component primitives
- **Lightweight Charts**: Professional trading charts

### Web3 Integration
- **Wagmi v2**: React hooks for Ethereum interactions
- **RainbowKit**: Beautiful wallet connection UX
- **Viem**: TypeScript interface for Ethereum
- **Multi-chain Support**: Base Chain (8453) and BSC (56)

### Key Libraries
- **@farcaster/miniapp-sdk**: Farcaster Mini App integration
- **@tanstack/react-query**: Server state management
- **Recharts**: Additional charting capabilities
- **Ethers.js**: Ethereum library for blockchain interactions

### API & Services
- **Lemon Oracle**: Custom price oracle for multi-chain asset pricing
- **Position Management**: RESTful APIs for trade lifecycle
- **Real-time Updates**: Live price feeds and position updates

## 🚀 Getting Started

### Prerequisites

- **Node.js** 18+ or **Bun** (recommended)
- A Web3 wallet (MetaMask, WalletConnect, etc.)
- Basic understanding of DeFi and perpetual futures

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd lemon-looper-main
   ```

2. **Install dependencies**
   ```bash
   # Using Bun (recommended)
   bun install
   
   # Or using npm
   npm install
   ```

3. **Set up environment variables**
   Create a `.env.local` file in the root directory:
   ```env
   NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID=your_walletconnect_project_id
   NEXT_PUBLIC_APP_URL=http://localhost:3000
   ```

4. **Start the development server**
   ```bash
   # Using Bun
   bun dev
   
   # Or using npm
   npm run dev
   ```

5. **Open your browser**
   Navigate to [http://localhost:3000](http://localhost:3000)

### Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID` | WalletConnect project ID for wallet connections | Yes |
| `NEXT_PUBLIC_APP_URL` | Base URL of your application | Yes |

## 🛠️ Development

### Available Scripts

```bash
# Start development server
bun dev

# Build for production
bun run build

# Start production server
bun start

# Run linting
bun run lint
```

### Project Structure

```
src/
├── app/                    # Next.js App Router pages
│   ├── api/               # API routes
│   ├── perp/              # Trading interface
│   ├── positions/         # Position management
│   └── trending/          # Market trends
├── components/            # React components
│   ├── layout/           # Layout components
│   ├── trading/          # Trading-specific components
│   └── ui/               # Reusable UI components
├── hooks/                # Custom React hooks
├── lib/                  # Utility libraries
└── contexts/             # React contexts
```

### Key Components

- **TradingInterface**: Main trading UI with charts and order forms
- **PositionsTable**: Real-time position tracking
- **ChainSelector**: Multi-chain network switching
- **WalletProvider**: Web3 wallet integration
- **MiniAppProvider**: Farcaster Mini App support

## 🌐 Multi-Chain Support

The platform supports multiple blockchain networks:

### Supported Chains
- **Base Chain** (Chain ID: 8453) - Primary network
- **Binance Smart Chain** (Chain ID: 56) - Secondary network

### Chain Features
- Automatic chain detection and switching
- Chain-specific price oracles
- Cross-chain position management
- Network-specific transaction handling

## 📱 Farcaster Mini App Integration

Lemon Perp works as both a standalone web application and a Farcaster Mini App:

### Features
- Native Farcaster user integration
- Social trading and sharing
- Cast composition for trade sharing
- User context awareness

### Testing as Mini App
```bash
# 1. Start development server
bun dev

# 2. Expose locally with ngrok
ngrok http 3000

# 3. Test with Farcaster preview tool
# https://farcaster.xyz/~/developers/mini-apps/preview?url=YOUR_NGROK_URL
```

## 🐳 Docker Deployment

The project includes Docker support for containerized deployment:

```bash
# Build Docker image
docker build -t lemon-perp .

# Run container
docker run -p 3000:3000 lemon-perp
```

## 🔧 Configuration

### Trading Configuration
- Maximum leverage: 100x
- Supported asset classes: Crypto, Traditional Markets, Commodities
- Minimum position size: Varies by asset
- Trading fees: Dynamic based on market conditions

### Network Configuration
- Development: Supports testnets (Sepolia, etc.)
- Production: Mainnet chains only
- Automatic fallback for network issues

## 📊 Trading Features

### Position Types
- **Long Positions**: Profit from price increases
- **Short Positions**: Profit from price decreases
- **Leveraged Trading**: Amplify gains (and risks) up to 100x

### Risk Management
- Stop-loss orders
- Take-profit levels
- Position size limits
- Real-time P&L tracking

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## ⚠️ Risk Disclaimer

Trading perpetual futures involves substantial risk of loss and is not suitable for all investors. Past performance is not indicative of future results. Please trade responsibly and only with funds you can afford to lose.

## 🔗 Links

- **Documentation**: See `QUICKSTART.md` for Farcaster Mini App setup
- **Base Chain Integration**: See `BASE_CHAIN_INTEGRATION.md` for technical details
- **Support**: Open an issue for bug reports or feature requests

## 🏆 Acknowledgments

- Built with Next.js and the React ecosystem
- Powered by Wagmi and Viem for Web3 integration
- Charts by Lightweight Charts (TradingView)
- UI components by Radix UI
- Styled with Tailwind CSS
