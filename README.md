# 🌟 Pay-Per-Call on Stellar Network

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Built with Soroban](https://img.shields.io/badge/Built%20with-Soroban-blue)](https://soroban.stellar.org)
[![WebRTC](https://img.shields.io/badge/WebRTC-Enabled-green)](https://webrtc.org)

> **Decentralized, blockchain-based voice calls with micropayments per second**  
> Pay for conversations in real-time using Stellar (XLM) - No intermediaries, transparent billing, instant settlements.

---

## 📋 Table of Contents
- [🌟 Features](#-features)
- [🚀 Quick Start](#-quick-start)
- [🏗️ Architecture](#-architecture)
- [🔧 Installation](#-installation)
- [💻 Usage](#-usage)
- [📁 Project Structure](#-project-structure)
- [🛠️ Smart Contract](#-smart-contract)
- [🎨 Frontend](#-frontend)
- [📡 Signaling Server](#-signaling-server)
- [🚢 Deployment](#-deployment)
- [🔒 Security](#-security)
- [🧪 Testing](#-testing)
- [🤝 Contributing](#-contributing)
- [📄 License](#-license)

---

## 🌟 Features

### 💰 **Blockchain-Powered Payments**
- **Pay-per-second billing** - Only pay for actual conversation time
- **Stellar XLM payments** - Ultra-low transaction fees (~0.00001 XLM)
- **Smart contract escrow** - Funds held securely until call completion
- **Real-time balance tracking** - Live updates of remaining balance
- **Auto-refund** - Unused funds returned automatically

### 📞 **Voice Communication**
- **WebRTC peer-to-peer** - Direct, encrypted audio calls
- **No central servers** - Privacy-focused architecture
- **Crystal clear audio** - Modern codec support (Opus)
- **Low latency** - Optimized for real-time conversation

### 🔐 **Security & Privacy**
- **End-to-end encryption** - Your conversations stay private
- **Wallet authentication** - No passwords, just crypto wallets
- **On-chain transparency** - All payments verifiable on blockchain
- **Pseudonymous** - Use Stellar addresses instead of personal info

### 🎯 **User Experience**
- **Simple wallet connection** - One-click with Freighter extension
- **Live call dashboard** - Timer, balance, and controls in one view
- **Rate customization** - Set your own price per second
- **Responsive design** - Works on desktop and mobile

---

## 🚀 Quick Start

### Prerequisites
- **Node.js 18+** and npm
- **Rust 1.70+** (for smart contracts)
- **Freighter Wallet** extension
- **Stellar Testnet account** (get free XLM from friendbot)

### 5-Minute Setup

```bash
# 1. Clone the repository
git clone https://github.com/yourusername/stellar-pay-per-call.git
cd stellar-pay-per-call

# 2. Install dependencies
npm install

# 3. Set up environment variables
cp .env.example .env.local
# Edit .env.local with your details

# 4. Start the development servers
npm run dev:all

# 5. Open your browser
# Navigate to http://localhost:3000
```

### Get Test XLM
```bash
# Use Stellar Friendbot to get test XLM
curl "https://friendbot.stellar.org?addr=YOUR_PUBLIC_KEY"
```

---

## 🏗️ Architecture

### System Overview
```
┌─────────────────────────────────────────────────────────────┐
│                    Frontend (Next.js)                       │
│  • Wallet Connection (Freighter)                           │
│  • Call Interface                                          │
│  • Real-time Updates                                       │
└───────────────┬─────────────────────────────────────────────┘
                │ HTTP/WebSocket
┌───────────────▼─────────────────────────────────────────────┐
│               Signaling Server (Node.js)                    │
│  • WebRTC SDP/ICE Exchange                                 │
│  • Call Session Management                                 │
└───────────────┬─────────────────────────────────────────────┘
                │ Stellar Transactions
┌───────────────▼─────────────────────────────────────────────┐
│            Stellar Blockchain (Soroban)                     │
│  • PayPerCall Smart Contract                               │
│  • Escrow & Payment Logic                                  │
│  • Event Emission                                          │
└─────────────────────────────────────────────────────────────┘
```

### Data Flow
1. **Call Initiation**: Caller deposits XLM into smart contract escrow
2. **WebRTC Connection**: Peer-to-peer audio established via signaling server
3. **Real-time Billing**: Smart contract tracks seconds and deducts balance
4. **Call Completion**: Funds distributed, unused amount refunded

---

## 🔧 Installation

### Detailed Setup Instructions

#### 1. Smart Contract Setup
```bash
# Install Soroban CLI
curl -sSL https://soroban.stellar.org/install.sh | bash

# Build the contract
cd contracts/paypercall
cargo build --target wasm32-unknown-unknown --release

# Optimize WASM size
soroban contract optimize \
  --wasm target/wasm32-unknown-unknown/release/paypercall.wasm
```

#### 2. Frontend Setup
```bash
# Install frontend dependencies
cd frontend
npm install

# Install required packages
npm install @stellar/freighter-api @stellar/stellar-sdk webrtc-adapter
```

#### 3. Signaling Server Setup
```bash
# Install signaling server dependencies
cd signaling-server
npm install

# Start the server
node server.js
```

---

## 💻 Usage

### For Callers
1. **Connect Wallet**: Click "Connect Freighter Wallet"
2. **Fund Account**: Deposit XLM (use Testnet for development)
3. **Start Call**: Enter callee's Stellar address and deposit amount
4. **Talk**: Communicate while watching your balance update in real-time
5. **End Call**: Hang up to receive refund of unused funds

### For Callees
1. **Set Your Rate**: Configure your price per second in the profile page
2. **Receive Calls**: Accept incoming call requests
3. **Earn**: Get paid automatically when calls end
4. **Withdraw**: Transfer earnings to your wallet

### Call Interface
```
┌─────────────────────────────────────────┐
│           ACTIVE CALL                   │
│                                         │
│  ⏱️  Time: 03:45                       │
│  💰  Balance: 12.5 XLM                 │
│  📞  Rate: 0.05 XLM/sec                │
│                                         │
│  [🎤 Mute]    [📞 End Call]            │
│                                         │
│  Progress: ███████████░░░░ 65%         │
└─────────────────────────────────────────┘
```

---

## 📁 Project Structure

```
stellar-pay-per-call/
├── contracts/
│   └── paypercall/
│       ├── src/
│       │   ├── lib.rs              # Main smart contract
│       │   └── test.rs             # Contract tests
│       ├── Cargo.toml
│       └── target/                 # Built WASM files
├── frontend/
│   ├── app/
│   │   ├── (auth)/
│   │   │   ├── connect/
│   │   │   └── profile/
│   │   ├── call/[id]/
│   │   ├── api/webrtc/
│   │   └── layout.tsx
│   ├── components/
│   │   ├── WalletConnector.tsx
│   │   ├── CallInterface.tsx
│   │   └── BalanceDisplay.tsx
│   ├── lib/
│   │   ├── stellar.ts
│   │   ├── webrtc.ts
│   │   └── contract-client.ts
│   └── public/
├── signaling-server/
│   ├── server.js
│   ├── package.json
│   └── README.md
├── docker/
│   ├── Dockerfile.frontend
│   ├── Dockerfile.server
│   └── docker-compose.yml
├── scripts/
│   ├── deploy-contract.sh
│   ├── fund-accounts.sh
│   └── test-call.sh
├── .env.example
├── package.json
└── README.md
```

---

## 🛠️ Smart Contract

### Contract Functions

| Function | Description | Auth Required |
|----------|-------------|---------------|
| `initialize_call` | Start new call with escrow | Caller |
| `update_usage` | Update amount used (called periodically) | Caller |
| `end_call` | End call and distribute funds | Both parties |
| `set_rate` | Set per-second rate | User |
| `get_session` | View call details | Anyone |
| `emergency_cancel` | Cancel within 5 minutes | Caller |

### Key Design Decisions
1. **Escrow-based**: Funds locked until call completion
2. **Per-second billing**: Granular time tracking
3. **Auto-refund**: Unused funds returned automatically
4. **Emergency exit**: Safety mechanism for failed calls
5. **Event emission**: Frontend can track state changes

### Contract Deployment
```bash
# Deploy to Stellar Testnet
soroban contract deploy \
  --wasm target/wasm32-unknown-unknown/release/paypercall.wasm \
  --source-account YOUR_SECRET_KEY \
  --network testnet

# Initialize contract
soroban contract invoke \
  --id CONTRACT_ID \
  --source-account YOUR_SECRET_KEY \
  --network testnet \
  -- \
  initialize
```

---

## 🎨 Frontend

### Technology Stack
- **Next.js 14** - React framework with App Router
- **TypeScript** - Type-safe development
- **Tailwind CSS** - Utility-first styling
- **Freighter API** - Stellar wallet integration
- **WebRTC** - Peer-to-peer communication

### Key Components

#### WalletConnector.tsx
Handles Freighter wallet connection and balance display.

#### CallInterface.tsx
Main call interface with controls, timer, and balance display.

#### BalanceDisplay.tsx
Real-time balance and progress visualization.

### Environment Variables
```env
# .env.local
NEXT_PUBLIC_CONTRACT_ID=YOUR_CONTRACT_ID
NEXT_PUBLIC_NETWORK=testnet
NEXT_PUBLIC_HORIZON_URL=https://horizon-testnet.stellar.org
NEXT_PUBLIC_SIGNALING_SERVER=ws://localhost:8080
NEXT_PUBLIC_RPC_URL=https://soroban-testnet.stellar.io
```

---

## 📡 Signaling Server

### Purpose
The signaling server facilitates WebRTC connection establishment without handling any payment logic or call content.

### Features
- **WebSocket-based** - Real-time signaling
- **Session management** - Track active calls
- **SDP exchange** - Handle offer/answer negotiation
- **ICE candidate relay** - NAT traversal assistance

### Running the Server
```bash
cd signaling-server
npm start
# Server runs on http://localhost:8080
```

### API Endpoints
- `GET /health` - Server health check
- `WS /signaling?callId=X&userId=Y` - WebSocket signaling

---

## 🚢 Deployment

### Docker Deployment
```bash
# Build and run all services
docker-compose up --build

# Or run individually
docker build -t paypercall-frontend -f docker/Dockerfile.frontend .
docker build -t paypercall-server -f docker/Dockerfile.server .
```

### Manual Deployment

#### 1. Smart Contract
```bash
# Build optimized WASM
soroban contract optimize --wasm contract.wasm

# Deploy to mainnet (use with caution!)
soroban contract deploy \
  --wasm contract.optimized.wasm \
  --source-account MAINNET_SECRET_KEY \
  --network mainnet \
  --fee 10000000
```

#### 2. Frontend (Vercel)
```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel --prod
```

#### 3. Signaling Server (Railway/Render)
```bash
# Railway deployment
railway up
```

### Production Environment
```env
# Production .env
NEXT_PUBLIC_NETWORK=mainnet
NEXT_PUBLIC_HORIZON_URL=https://horizon.stellar.org
NEXT_PUBLIC_SIGNALING_SERVER=wss://your-domain.com
NEXT_PUBLIC_RPC_URL=https://soroban.stellar.io
```

---

## 🔒 Security

### Smart Contract Security
- ✅ **Reentrancy protection** - Soroban execution model prevents reentrancy
- ✅ **Access control** - All functions properly authenticated
- ✅ **Input validation** - All parameters validated before processing
- ✅ **Balance checks** - Prevent overdrafts with explicit checks
- ✅ **Emergency stop** - Cancel function for abnormal situations

### WebRTC Security
- ✅ **SRTP encryption** - All media encrypted end-to-end
- ✅ **DTLS** - Data channel encryption
- ✅ **SDP sanitization** - Validate session descriptions
- ✅ **STUN/TURN security** - Token-based authentication

### Frontend Security
- ✅ **Wallet validation** - Verify Freighter wallet authenticity
- ✅ **Input sanitization** - Prevent XSS attacks
- ✅ **Secure storage** - Encrypt sensitive localStorage data
- ✅ **HTTPS enforcement** - Force HTTPS in production

### Audit Considerations
1. **Contract audit** - Consider professional smart contract audit
2. **Penetration testing** - Test WebRTC implementation
3. **Bug bounty** - Establish bug bounty program for production

---

## 🧪 Testing

### Smart Contract Tests
```bash
cd contracts/paypercall
cargo test

# Run specific test suite
cargo test test_initialize_call -- --nocapture
```

### Frontend Tests
```bash
cd frontend
npm test
npm run test:e2e  # End-to-end tests
```

### Integration Tests
```bash
# Run full test suite
npm run test:all

# Load testing
npm run test:load

# Security audit
npm run audit
```

### Test Coverage
- ✅ **Unit tests** - Individual function testing
- ✅ **Integration tests** - Cross-component testing
- ✅ **E2E tests** - Full user flow testing
- ✅ **Load tests** - Performance under load
- ✅ **Security tests** - Vulnerability scanning

---

## 🤝 Contributing

We love contributions! Here's how to help:

### Development Workflow
1. **Fork** the repository
2. **Create a feature branch**
   ```bash
   git checkout -b feature/amazing-feature
   ```
3. **Commit your changes**
   ```bash
   git commit -m 'Add amazing feature'
   ```
4. **Push to the branch**
   ```bash
   git push origin feature/amazing-feature
   ```
5. **Open a Pull Request**

### Development Guidelines
- Follow TypeScript strict mode
- Write tests for new features
- Update documentation
- Use conventional commits
- Keep PRs focused and small

### Project Board
Check our [GitHub Projects](https://github.com/yourusername/stellar-pay-per-call/projects) for:
- 🎯 **Todo** - Features to be implemented
- 🔄 **In Progress** - Currently being worked on
- ✅ **Done** - Completed features
- 🐛 **Bugs** - Issues to fix

### Need Help?
- Join our [Discord community](https://discord.gg/your-invite)
- Check [existing issues](https://github.com/yourusername/stellar-pay-per-call/issues)
- Read the [developer guide](docs/DEVELOPER.md)

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

### Third-Party Licenses
- **Stellar SDK** - Apache 2.0
- **Soroban** - Apache 2.0
- **WebRTC** - BSD-3-Clause
- **Next.js** - MIT

### Commercial Use
This software can be used commercially. Attribution is appreciated but not required.

---

## 🙏 Acknowledgments

- **Stellar Development Foundation** for the amazing blockchain
- **Soroban team** for smart contract capabilities
- **WebRTC community** for real-time communication standards
- **All contributors** who help improve this project

## 📞 Support

- **Documentation**: [docs.stellarpaypercall.com](https://docs.stellarpaypercall.com)
- **Community**: [Discord](https://discord.gg/your-invite)
- **Issues**: [GitHub Issues](https://github.com/yourusername/stellar-pay-per-call/issues)
- **Email**: support@stellarpaypercall.com

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=yourusername/stellar-pay-per-call&type=Date)](https://star-history.com/#yourusername/stellar-pay-per-call&Date)

---

**Made with ❤️ by the decentralized communications community**

---

*Note: This is production-ready software. Always test with small amounts first. Cryptocurrency transactions are irreversible.*
