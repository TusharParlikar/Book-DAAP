# Project 03: Bitcoin Transaction Analyzer

**Chapter:** 02 - Cryptocurrencies & Bitcoin
**Difficulty:** Intermediate
**Estimated Time:** 4-5 hours

---

## 📋 Project Description

Build a web application that fetches and analyzes real Bitcoin transactions using blockchain APIs. Display transaction details, UTXO flows, and fee analysis in an educational format.

---

## 🎯 Learning Objectives

- Work with Bitcoin blockchain APIs
- Understand UTXO model
- Analyze transaction fees
- Parse and display blockchain data

---

## 🛠️ Technology Stack

- Next.js 14+ or React
- Blockchain.com API or Blockchair API
- Axios for API calls
- Tailwind CSS
- Chart.js (for visualizations)

---

## ✅ TODO List

### Phase 1: API Setup
- [ ] Register for blockchain API access
- [ ] Set up API client
- [ ] Test API endpoints (get transaction, get block)
- [ ] Handle API rate limits

### Phase 2: Transaction Lookup
- [ ] Create transaction ID input
- [ ] Fetch transaction data
- [ ] Display basic info (hash, time, size, fee)
- [ ] Show confirmations count

### Phase 3: UTXO Visualization
- [ ] Parse inputs (which UTXOs were spent)
- [ ] Parse outputs (new UTXOs created)
- [ ] Create visual flow diagram
- [ ] Show addresses involved

### Phase 4: Fee Analysis
- [ ] Calculate fee in BTC and USD
- [ ] Show fee per byte (sat/vB)
- [ ] Compare with current mempool fees
- [ ] Explain if fee was too high/low/appropriate

### Phase 5: Advanced Features
- [ ] Show transaction in block
- [ ] Link to addresses' balance and history
- [ ] Export transaction data as JSON
- [ ] Search by block height or address

---

## 💡 Hints

**Hint 1: Blockchain.com API**
```javascript
// Get transaction
GET https://blockchain.info/rawtx/{tx_hash}

// Get latest block
GET https://blockchain.info/latestblock
```

**Hint 2: UTXO Flow Visualization**
- Use arrows to show flow from inputs to outputs
- Label each UTXO with address and amount
- Use colors to distinguish different addresses
- Show "change" output returning to sender

**Hint 3: Fee Calculation**
```
Fee = Total Inputs - Total Outputs
Fee Rate (sat/vB) = (Fee in satoshis) / (Transaction size in bytes)
```

**Hint 4: Real Transaction Examples**
- Bitcoin Genesis Block Coinbase: `4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b`
- Pizza Transaction (first real-world BTC transaction)
- Use testnet for experiments

**Hint 5: Error Handling**
- Invalid transaction ID
- API rate limits
- Network errors
- Graceful fallbacks

---

## 🎨 Bonus Challenges

1. **Mempool Viewer**: Show pending transactions
2. **Address Tracker**: Track balance and history of any address
3. **Fee Estimator**: Suggest appropriate fees based on current network
4. **Transaction Builder**: Simulate creating a transaction
5. **SegWit Detection**: Identify and explain SegWit transactions
6. **Multi-sig Detection**: Identify multi-signature transactions

---

## 📚 Reference Links

- **Blockchain.com API:** https://www.blockchain.com/api
- **Blockchair API:** https://blockchair.com/api
- **Bitcoin Transaction Explorer:** https://www.blockchain.com/explorer

---

## ✨ Success Criteria

- ✅ Correctly fetches real Bitcoin transaction data
- ✅ UTXO flow is visually clear
- ✅ Fee analysis is accurate and educational
- ✅ Handles errors gracefully
- ✅ Responsive design
- ✅ Links to blockchain explorer for deeper inspection

---

*Project for Blockchain Learning Series - Chapter 02*