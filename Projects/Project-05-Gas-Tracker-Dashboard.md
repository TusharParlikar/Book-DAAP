# Project 05: Ethereum Gas Tracker Dashboard

**Chapter:** 03 - Ethereum & Smart Contracts
**Difficulty:** Intermediate
**Estimated Time:** 4-5 hours

---

## 📋 Project Description

Build a real-time gas price tracking dashboard that helps users choose optimal transaction fees. Display current gas prices, historical trends, and fee predictions.

---

## 🎯 Learning Objectives

- Understand Ethereum gas mechanics
- Work with Etherscan/Alchemy APIs
- Visualize blockchain metrics
- Implement real-time data updates

---

## 🛠️ Technology Stack

- Next.js 14+
- Etherscan Gas Tracker API
- Alchemy or Infura
- Chart.js or Recharts
- Tailwind CSS
- WebSocket for real-time updates

---

## ✅ TODO List

### Phase 1: API Integration
- [ ] Setup Etherscan API key
- [ ] Fetch current gas prices (slow/average/fast)
- [ ] Get gas price history
- [ ] Implement auto-refresh every 15 seconds

### Phase 2: Current Gas Display
- [ ] Show current gas prices in Gwei
- [ ] Calculate USD cost for common operations (transfer, swap, NFT mint)
- [ ] Show estimated confirmation times
- [ ] Display network congestion level

### Phase 3: Historical Charts
- [ ] 24-hour gas price line chart
- [ ] 7-day average trends
- [ ] Peak hours analysis
- [ ] Compare weekday vs weekend

### Phase 4: Transaction Cost Calculator
- [ ] Select transaction type (transfer, approve, swap, etc.)
- [ ] Show gas cost for each priority level
- [ ] Display total cost in ETH and USD
- [ ] Suggest best time to transact

### Phase 5: Advanced Features
- [ ] Gas price alerts (notify when price drops below threshold)
- [ ] Save favorite gas limits for different operations
- [ ] Export data as CSV
- [ ] Compare L1 vs L2 costs

---

## 💡 Hints

**Hint 1: Etherscan Gas API**
```javascript
GET https://api.etherscan.io/api?module=gastracker&action=gasoracle&apikey=YOUR_KEY

Response: {
  SafeGasPrice: "20",
  ProposeGasPrice: "25",
  FastGasPrice: "30"
}
```

**Hint 2: Transaction Cost Calculation**
```javascript
const gasCost = gasLimit * gasPriceGwei;
const costInETH = gasCost / 1e9; // Convert from Gwei to ETH
const costInUSD = costInETH * ethPriceUSD;

// Common gas limits:
// ETH transfer: 21,000
// ERC20 transfer: 65,000
// Uniswap swap: 150,000
// NFT mint: 200,000
```

**Hint 3: Real-Time Updates**
- Use `setInterval` or WebSocket
- Update every 12-15 seconds
- Show "Last updated" timestamp
- Add loading indicators

**Hint 4: Network Congestion Indicator**
```javascript
if (gasPrice < 20) return "Low - Great time to transact!";
if (gasPrice < 50) return "Normal";
if (gasPrice < 100) return "High - Consider waiting";
return "Extreme - Wait if possible";
```

**Hint 5: EIP-1559 Display**
- Show base fee + priority fee
- Explain fee burning
- Show how much ETH was burned today

---

## 🎨 Bonus Challenges

1. **Browser Extension**: Create Chrome extension for quick gas check
2. **Price Predictions**: ML model to predict gas prices
3. **Telegram Bot**: Send gas price alerts via Telegram
4. **Layer 2 Comparison**: Show Arbitrum, Optimism, Polygon fees
5. **Historical Events**: Mark significant events on chart (NFT drops, etc.)
6. **Gas Token**: Calculate savings from using CHI/GST2 gas tokens

---

## 📚 Reference Links

- **Etherscan Gas Tracker:** https://etherscan.io/gastracker
- **Alchemy Gas API:** https://docs.alchemy.com/reference/eth-gasprice
- **EIP-1559 Explained:** https://eips.ethereum.org/EIPS/eip-1559

---

## ✨ Success Criteria

- ✅ Real-time gas price updates
- ✅ Accurate cost calculations
- ✅ Clear visualizations
- ✅ Historical trends analysis
- ✅ Mobile-responsive
- ✅ Educational tooltips

---

*Project for Blockchain Learning Series - Chapter 03*