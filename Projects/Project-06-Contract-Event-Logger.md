# Project 06: Smart Contract Event Logger

**Chapter:** 03 - Ethereum & Smart Contracts
**Difficulty:** Intermediate
**Estimated Time:** 4-6 hours

---

## 📋 Project Description

Create a tool that listens to smart contract events in real-time and displays them in a user-friendly dashboard. Perfect for monitoring DeFi protocols, NFT mints, or any contract activity.

---

## 🎯 Learning Objectives

- Work with contract events
- Use Ethers.js event listeners
- Real-time data streaming
- Parse and format blockchain events

---

## 🛠️ Technology Stack

- Next.js with WebSockets
- Ethers.js v6
- Alchemy or Infura WebSocket endpoint
- Tailwind CSS
- Database (optional): SQLite or PostgreSQL

---

## ✅ TODO List

### Phase 1: Contract Setup
- [ ] Input for contract address
- [ ] Fetch contract ABI from Etherscan
- [ ] Display available events
- [ ] Allow user to select events to monitor

### Phase 2: Event Listener
- [ ] Connect to Ethereum via WebSocket
- [ ] Set up event listeners for selected events
- [ ] Parse event data
- [ ] Display events in real-time

### Phase 3: Event Display
- [ ] Show event name, parameters, transaction hash
- [ ] Display timestamp
- [ ] Link to Etherscan
- [ ] Color-code different event types

### Phase 4: Filtering & Search
- [ ] Filter by event type
- [ ] Search by address or value
- [ ] Date range filter
- [ ] Export filtered results

### Phase 5: Persistence
- [ ] Save events to database
- [ ] Historical event viewer
- [ ] Statistics (events per hour, top participants)
- [ ] Webhook notifications for specific events

---

## 💡 Hints

**Hint 1: WebSocket Connection**
```javascript
import { ethers } from 'ethers';

const provider = new ethers.WebSocketProvider(
  'wss://eth-mainnet.g.alchemy.com/v2/YOUR-API-KEY'
);
```

**Hint 2: Listening to Events**
```javascript
const contract = new ethers.Contract(address, abi, provider);

contract.on("Transfer", (from, to, amount, event) => {
  console.log(`Transfer: ${from} → ${to}, Amount: ${amount}`);
  // Add to UI
});
```

**Hint 3: Popular Contracts to Monitor**
- USDC: `0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48`
- Uniswap V3 Router: `0xE592427A0AEce92De3Edee1F18E0157C05861564`
- Bored Ape Yacht Club: `0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D`

**Hint 4: Event Formatting**
```javascript
// Format addresses
const shortAddress = address.slice(0, 6) + '...' + address.slice(-4);

// Format amounts
const formatted = ethers.formatUnits(amount, 18);

// Format timestamp
const date = new Date(timestamp * 1000).toLocaleString();
```

**Hint 5: Error Handling**
- Handle WebSocket disconnections
- Reconnect automatically
- Queue events during disconnection
- Show connection status

---

## 🎨 Bonus Challenges

1. **Multi-Contract Monitoring**: Watch multiple contracts simultaneously
2. **Telegram Notifications**: Send alerts via Telegram bot
3. **Pattern Detection**: Identify unusual activity (whale movements)
4. **Event Analytics**: Visualize event patterns over time
5. **Smart Filters**: "Notify me when transfer > 100 ETH"
6. **Historical Sync**: Fetch past events on startup

---

## 📚 Reference Links

- **Ethers.js Events:** https://docs.ethers.org/v6/api/contract/#ContractEvent
- **Alchemy WebSockets:** https://docs.alchemy.com/docs/using-websockets
- **Etherscan API:** https://docs.etherscan.io/

---

## ✨ Success Criteria

- ✅ Real-time event monitoring
- ✅ Clean, readable event display
- ✅ Handles multiple event types
- ✅ Reliable WebSocket connection
- ✅ Historical event browsing
- ✅ Export functionality

---

*Project for Blockchain Learning Series - Chapter 03*