# Project 04: Bitcoin Mining Profitability Calculator

**Chapter:** 02 - Cryptocurrencies & Bitcoin
**Difficulty:** Beginner-Intermediate
**Estimated Time:** 3-4 hours

---

## 📋 Project Description

Create a calculator that helps users understand Bitcoin mining economics. Calculate profitability based on hashrate, electricity costs, hardware costs, and current Bitcoin price/difficulty.

---

## 🎯 Learning Objectives

- Understand Bitcoin mining economics
- Work with real-time Bitcoin data (difficulty, price)
- Calculate ROI and break-even points
- Learn about energy consumption in PoW

---

## 🛠️ Technology Stack

- React or Next.js
- CoinGecko API (for BTC price)
- Blockchain.com API (for difficulty)
- Chart.js (for profit graphs)
- Tailwind CSS

---

## ✅ TODO List

### Phase 1: Data Fetching
- [ ] Fetch current Bitcoin price from CoinGecko
- [ ] Fetch current mining difficulty
- [ ] Fetch average block time
- [ ] Calculate current network hashrate

### Phase 2: Input Form
- [ ] Hardware hashrate input (TH/s)
- [ ] Power consumption input (Watts)
- [ ] Electricity cost input ($/kWh)
- [ ] Hardware cost input ($)
- [ ] Pool fee percentage input

### Phase 3: Calculation Engine
- [ ] Calculate expected BTC mined per day
- [ ] Calculate electricity cost per day
- [ ] Calculate daily, monthly, yearly profit
- [ ] Calculate break-even time
- [ ] Calculate ROI percentage

### Phase 4: Results Display
- [ ] Show daily/monthly/yearly profit
- [ ] Display break-even timeline
- [ ] Show electricity vs revenue chart
- [ ] Display current vs future scenarios

### Phase 5: Advanced Features
- [ ] Halving countdown and impact calculator
- [ ] Difficulty adjustment predictions
- [ ] Multiple mining hardware presets (Antminer S19, etc.)
- [ ] Historical profitability graph

---

## 💡 Hints

**Hint 1: Mining Profitability Formula**
```javascript
// Daily BTC mined
const networkHashrate = ... // EH/s
const yourHashrate = ... // TH/s
const blockReward = 3.125; // Current (post-2024 halving)
const blocksPerDay = 144; // (24 * 60) / 10 minutes

const yourShare = yourHashrate / (networkHashrate * 1000000);
const dailyBTC = yourShare * blockReward * blocksPerDay;
```

**Hint 2: Electricity Cost**
```javascript
const powerKW = watts / 1000;
const hoursPerDay = 24;
const dailyElectricityCost = powerKW * hoursPerDay * electricityRate;
```

**Hint 3: Popular Mining Hardware (2025)**
- Antminer S19 XP: 140 TH/s, 3010W
- Whatsminer M50S: 130 TH/s, 3100W
- Antminer S21: 200 TH/s, 3500W

**Hint 4: API Integration**
```javascript
// CoinGecko - BTC Price
GET https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=usd

// Blockchain.com - Network Stats
GET https://blockchain.info/stats
```

**Hint 5: Break-Even Calculation**
```javascript
const dailyProfit = dailyRevenue - dailyElectricity - poolFee;
const breakEvenDays = hardwareCost / dailyProfit;
```

---

## 🎨 Bonus Challenges

1. **Halving Countdown**: Show days until next halving and impact
2. **Cloud Mining**: Compare self-mining vs cloud mining
3. **Multi-Currency**: Add Ethereum, Litecoin mining calculations
4. **Historical Data**: Show how profitability changed over time
5. **Comparison Mode**: Compare multiple mining setups side-by-side
6. **Tax Calculator**: Estimate mining income taxes

---

## 📚 Reference Links

- **CoinGecko API:** https://www.coingecko.com/en/api
- **Mining Hardware Comparison:** https://www.asicminervalue.com
- **Bitcoin Mining Pools:** https://miningpoolstats.stream/bitcoin

---

## ✨ Success Criteria

- ✅ Accurate profitability calculations
- ✅ Real-time BTC price and difficulty data
- ✅ Clear visualization of costs vs revenue
- ✅ Hardware presets for ease of use
- ✅ Educational tooltips explaining each metric
- ✅ Mobile-responsive design

---

*Project for Blockchain Learning Series - Chapter 02*