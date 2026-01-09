# 📞 Pay-Per-Call on Stellar Network

A decentralized, blockchain-based voice/video call platform that enables **micropayments per second** using the Stellar network. Users pay for calls with cryptocurrency directly to recipients—no intermediaries, transparent billing, instant settlements.

---

## 🌟 Features

- **⚡ Real-time billing** – Pay per second, not per minute
- **💰 Ultra-low fees** – Stellar's negligible transaction costs
- **🌍 Global payments** – No currency conversion hassles
- **🔒 Secure escrow** – Funds held safely until call completion
- **📊 Transparent ledger** – All transactions on public blockchain
- **🎯 Multiple tokens** – Pay with XLM, USDC, or custom tokens
- **📱 VoIP integration** – Works with WebRTC/SIP standards

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Client Layer                            │
│  • Web Interface (React/Vue)                               │
│  • Mobile Apps (React Native/Flutter)                      │
│  • VoIP Client (WebRTC/SIP.js)                             │
└───────────────────────┬─────────────────────────────────────┘
                        │
┌───────────────────────▼─────────────────────────────────────┐
│                Business Logic Layer                          │
│  • Call Session Manager                                    │
│  • Rate Calculator                                         │
│  • Payment Processor                                       │
│  • User Authentication                                     │
└───────────────────────┬─────────────────────────────────────┘
                        │
┌───────────────────────▼─────────────────────────────────────┐
│              Stellar Integration Layer                       │
│  • Account Management                                      │
│  • Transaction Builder                                     │
│  • Custom Asset (Token) Handler                            │
│  • Escrow Smart Contracts (Soroban)                        │
└───────────────────────┬─────────────────────────────────────┘
                        │
┌───────────────────────▼─────────────────────────────────────┐
│                 Stellar Network                              │
│  • Horizon API                                            │
│  • Testnet/Mainnet                                        │
│  • Custom Assets                                          │
└─────────────────────────────────────────────────────────────┘
```

---

## 📦 Prerequisites

- **Node.js** (v16+)
- **npm** or **yarn**
- **Stellar Account** (with test XLM)
- **VoIP Service** (Twilio, SIP provider, or self-hosted)
- **Redis** (for session management)
- **PostgreSQL** (optional, for user data)

---

## 🚀 Quick Start

### 1. Clone Repository
```bash
git clone https://github.com/yourusername/stellar-pay-per-call.git
cd stellar-pay-per-call
```

### 2. Install Dependencies
```bash
npm install
# or
yarn install
```

### 3. Environment Configuration
Create `.env` file:
```env
# Stellar Configuration
STELLAR_NETWORK=TESTNET  # TESTNET or PUBLIC
HORIZON_URL=https://horizon-testnet.stellar.org
ISSUER_SECRET_KEY=your_issuer_secret_here
PLATFORM_PUBLIC_KEY=your_platform_public_key
PLATFORM_SECRET_KEY=your_platform_secret_key

# Application
PORT=3000
SESSION_SECRET=your_session_secret
DATABASE_URL=postgresql://user:pass@localhost:5432/paypercall
REDIS_URL=redis://localhost:6379

# VoIP Configuration (Example for Twilio)
TWILIO_ACCOUNT_SID=your_twilio_sid
TWILIO_AUTH_TOKEN=your_twilio_token
TWILIO_PHONE_NUMBER=+1234567890

# Token Configuration
TOKEN_CODE=CALL
TOKEN_NAME="PayPerCall Token"
TOKEN_DESCRIPTION="Token for paying calls"
```

### 4. Initialize Stellar Accounts
```bash
npm run init-accounts
```
This will:
- Create issuer account
- Create platform escrow account
- Create CALL token asset
- Fund test accounts (testnet only)

### 5. Start Development Server
```bash
npm run dev
# Access at http://localhost:3000
```

---

## 🔧 Core Components

### 1. Smart Contracts (Soroban)
Located in `/contracts` directory:

- **Escrow Contract** – Holds funds during calls
- **Token Contract** – Custom CALL token implementation
- **Rate Contract** – Dynamic pricing management

Deploy contracts:
```bash
npm run deploy-contracts
```

### 2. Backend API
RESTful API in `/backend`:
- **/api/call/start** – Initiate call with escrow
- **/api/call/end** – End call and release payment
- **/api/call/status** – Check call status
- **/api/wallet/balance** – Get user balance
- **/api/wallet/deposit** – Deposit funds
- **/api/wallet/withdraw** – Withdraw earnings

### 3. Frontend Interface
React application in `/frontend`:
- Call dashboard
- Wallet management
- Contact list
- Call history
- Rate settings

### 4. VoIP Integration
WebRTC implementation in `/voip`:
- Peer-to-peer audio/video
- SIP.js for traditional telephony
- Twilio programmable voice

---

## 💰 Token Economics

### CALL Token (Custom Asset)
- **Asset Code:** CALL
- **Issuer:** Platform issuer account
- **Decimal:** 7 (like XLM)
- **Supply:** 10,000,000 (mintable)

### Rate Structure
```javascript
// Default rates (configurable per user)
const rates = {
  'basic': '0.01',      // 0.01 CALL per second
  'professional': '0.05',
  'premium': '0.10',
  'emergency': '0.25'
};

// Platform fee: 5% of each transaction
```

---

## 📞 How It Works

### For Callers:
1. **Fund Wallet** – Deposit XLM or CALL tokens
2. **Search Callee** – Find by Stellar address or username
3. **Initiate Call** – Payment placed in escrow
4. **Talk** – Timer runs, funds deducted in real-time
5. **End Call** – Remaining balance returned

### For Callees:
1. **Set Rate** – Configure per-second charge
2. **Receive Calls** – Accept incoming calls
3. **Earn** – Payment released immediately after call
4. **Withdraw** – Convert to XLM or fiat

---

## 🔐 Security Features

### 1. Escrow Protection
```javascript
// Multi-signature escrow account
const escrow = new StellarSdk.TransactionBuilder(account, {
  fee: StellarSdk.BASE_FEE,
  networkPassphrase
})
.addOperation(StellarSdk.Operation.setOptions({
  masterWeight: 0, // Disable master key
  lowThreshold: 2,
  medThreshold: 2,
  highThreshold: 2,
  signer: {
    ed25519PublicKey: callerPublicKey,
    weight: 1
  },
  signer: {
    ed25519PublicKey: calleePublicKey,
    weight: 1
  }
}));
```

### 2. Fraud Prevention
- Rate limiting per account
- Maximum call duration limits
- Suspicious activity monitoring
- Dispute resolution mechanism

### 3. Privacy
- No personal data on-chain
- End-to-end encrypted calls (optional)
- Pseudonymous addresses

---

## 🧪 Testing

### Unit Tests
```bash
npm test
```

### Integration Tests
```bash
npm run test:integration
```

### Load Testing
```bash
npm run test:load
```

### Test Scenarios Covered:
1. Call initiation and payment escrow
2. Partial refund for early termination
3. Rate calculation accuracy
4. Concurrent calls handling
5. Network failure recovery

---

## 🌐 Deployment

### 1. Docker Deployment
```bash
docker-compose up -d
```

### 2. Manual Deployment
```bash
# Build
npm run build

# Set production environment
export NODE_ENV=production
export STELLAR_NETWORK=PUBLIC

# Start
npm start
```

### 3. Environment Variables for Production
```env
STELLAR_NETWORK=PUBLIC
HORIZON_URL=https://horizon.stellar.org
ISSUER_SECRET_KEY=# Keep secure!
PLATFORM_SECRET_KEY=# Keep secure!
SSL_CERT_PATH=/path/to/cert
SSL_KEY_PATH=/path/to/key
```

---

## 📊 Monitoring & Analytics

### Built-in Dashboards:
1. **Transaction Monitor** – Real-time payment tracking
2. **Call Analytics** – Duration, frequency, revenue
3. **User Metrics** – Active users, retention
4. **Revenue Reports** – Daily/weekly/monthly

### Logging:
```bash
# Structured JSON logs
npm run logs
```

### Health Checks:
```bash
curl http://localhost:3000/health
```

---

## 🔄 API Documentation

### Start a Call
```http
POST /api/call/start
Content-Type: application/json
Authorization: Bearer <jwt_token>

{
  "callee": "GABCD...1234",
  "duration": 300,  // seconds
  "token": "CALL",  // or "XLM", "USDC"
  "rate": "0.01"    // optional, overrides callee's rate
}
```

### Response:
```json
{
  "callId": "abc123",
  "escrowId": "GEFGH...5678",
  "escrowAmount": "5.00",
  "websocketUrl": "wss://yourserver.com/call/abc123",
  "expiresAt": "2024-01-01T12:00:00Z"
}
```

Complete API docs: [API.md](docs/API.md)

---

## 🤝 Contributing

1. **Fork** the repository
2. **Create feature branch**: `git checkout -b feature/amazing-feature`
3. **Commit changes**: `git commit -m 'Add amazing feature'`
4. **Push to branch**: `git push origin feature/amazing-feature`
5. **Open Pull Request**

### Development Guidelines:
- Follow ESLint configuration
- Write tests for new features
- Update documentation
- Use conventional commits

---

## 📝 License

MIT License - see [LICENSE](LICENSE) file

---

## 🆘 Support

- **Issues**: [GitHub Issues](https://github.com/yourusername/stellar-pay-per-call/issues)
- **Discussions**: [GitHub Discussions](https://github.com/yourusername/stellar-pay-per-call/discussions)
- **Email**: support@paypercall.example.com

---

## 🙏 Acknowledgments

- Stellar Development Foundation
- Soroban smart contract platform
- WebRTC community
- Open source contributors

---

## 📈 Roadmap

### Phase 1 (Current)
- [x] Basic call functionality
- [x] Stellar payments integration
- [x] Web interface
- [ ] Mobile apps

### Phase 2 (Q2 2024)
- [ ] Soroban smart contracts
- [ ] Group calls
- [ ] Video calls
- [ ] Rate auctions

### Phase 3 (Q4 2024)
- [ ] Cross-chain payments
- [ ] AI-powered rate optimization
- [ ] Call recording (with consent)
- [ ] Marketplace for experts

---

## 🔗 Useful Links

- [Stellar Documentation](https://developers.stellar.org/)
- [Soroban Documentation](https://soroban.stellar.org/)
- [WebRTC Documentation](https://webrtc.org/)
- [Demo Video](https://youtube.com/demo)
- [Live Demo](https://demo.paypercall.example.com)

---

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=yourusername/stellar-pay-per-call&type=Date)](https://star-history.com/#yourusername/stellar-pay-per-call&Date)

---

**Made with ❤️ and ⭐ by the Decentralized Communications Team**

---

*Note: This is a prototype. Use at your own risk. Cryptocurrency transactions are irreversible.*
