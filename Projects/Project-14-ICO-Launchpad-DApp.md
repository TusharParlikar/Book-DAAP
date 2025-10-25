# Project 14: ICO Launchpad DApp

**Chapter:** 07 - Advanced DeFi & DePIN
**Difficulty:** Expert
**Estimated Time:** 15-18 hours

---

## 📋 Project Description

Build a complete ICO (Initial Coin Offering) launchpad where projects can create token sales and investors can participate. Multi-stage sales, vesting schedules, and refund mechanisms.

---

## 🎯 Learning Objectives

- Implement token sale mechanics
- Handle multiple sale stages
- Create vesting schedules
- Build crowdfunding logic
- Advanced DeFi patterns

---

## 🛠️ Technology Stack

- Solidity 0.8.20+
- Hardhat with full test coverage
- OpenZeppelin (ERC20, Ownable, ReentrancyGuard)
- Next.js 14+ with TypeScript
- RainbowKit + Wagmi + Viem
- Chart.js for progress visualization
- Tailwind CSS

---

## ✅ TODO List

### Phase 1: Token Contract
- [ ] Create ERC20 token for sale
- [ ] Mintable by ICO contract only
- [ ] Define tokenomics (total supply, distribution)
- [ ] Team/advisor allocations with vesting

### Phase 2: ICO Smart Contract
- [ ] Multiple sale stages (Seed, Private, Public)
- [ ] Price per token per stage
- [ ] Soft cap and hard cap
- [ ] Minimum and maximum contribution
- [ ] Whitelist for early stages
- [ ] Automatic stage progression

### Phase 3: Purchase Logic
- [ ] `buyTokens()` function (payable)
- [ ] Calculate tokens based on stage price
- [ ] Track contributions per address
- [ ] Enforce caps and limits
- [ ] Emit TokensPurchased event

### Phase 4: Claiming & Vesting
- [ ] Claim tokens after ICO ends
- [ ] Vesting schedule (e.g., 25% at TGE, rest over 6 months)
- [ ] `claimTokens()` function
- [ ] Calculate claimable amount
- [ ] Prevent double claims

### Phase 5: Refund Mechanism
- [ ] If soft cap not reached, allow refunds
- [ ] `refund()` function
- [ ] Return ETH to contributors
- [ ] Only available if ICO failed

### Phase 6: Admin Functions
- [ ] Add/remove from whitelist
- [ ] Set stage parameters
- [ ] Emergency pause
- [ ] Withdraw raised funds (if successful)
- [ ] Burn unsold tokens

### Phase 7: Frontend - Public View
- [ ] ICO overview (project info, tokenomics)
- [ ] Current stage display
- [ ] Progress bar (raised / hard cap)
- [ ] Token price and exchange rate
- [ ] Countdown to next stage
- [ ] Your contribution stats

### Phase 8: Frontend - Invest Flow
- [ ] Connect wallet button
- [ ] Check whitelist status
- [ ] Input ETH amount
- [ ] Preview token amount received
- [ ] Buy button with transaction
- [ ] Transaction status modal

### Phase 9: Frontend - Claim Page
- [ ] Show vested tokens
- [ ] Display claimable amount
- [ ] Vesting schedule timeline
- [ ] Claim button
- [ ] Claimed history

### Phase 10: Testing & Deployment
- [ ] Comprehensive test suite
- [ ] Test all stages
- [ ] Test refund scenario
- [ ] Test vesting claims
- [ ] Deploy to Sepolia
- [ ] Verify contracts
- [ ] Deploy frontend to Vercel

---

## 💡 Hints

**Hint 1: ICO Contract Structure**
```solidity
contract TokenICO is ReentrancyGuard, Ownable {
    IERC20 public token;
    
    enum Stage { Seed, Private, Public, Ended }
    Stage public currentStage = Stage.Seed;
    
    uint256 public constant SEED_PRICE = 0.0001 ether;  // per token
    uint256 public constant PRIVATE_PRICE = 0.0002 ether;
    uint256 public constant PUBLIC_PRICE = 0.0005 ether;
    
    uint256 public constant SOFT_CAP = 10 ether;
    uint256 public constant HARD_CAP = 100 ether;
    
    uint256 public totalRaised;
    uint256 public tokensSold;
    
    mapping(address => uint256) public contributions;
    mapping(address => uint256) public tokensPurchased;
    mapping(address => uint256) public tokensClaimed;
    mapping(address => bool) public whitelist;
    
    uint256 public saleEndTime;
    uint256 public claimStartTime;
    
    event TokensPurchased(address indexed buyer, uint256 ethAmount, uint256 tokenAmount);
    event TokensClaimed(address indexed claimer, uint256 amount);
    event Refunded(address indexed contributor, uint256 amount);
}
```

**Hint 2: Buy Tokens Function**
```solidity
function buyTokens() external payable nonReentrant {
    require(currentStage != Stage.Ended, "ICO ended");
    require(msg.value > 0, "Send ETH to buy");
    require(totalRaised + msg.value <= HARD_CAP, "Hard cap reached");
    
    // Check whitelist for early stages
    if (currentStage == Stage.Seed || currentStage == Stage.Private) {
        require(whitelist[msg.sender], "Not whitelisted");
    }
    
    // Calculate tokens based on current stage
    uint256 price = getCurrentPrice();
    uint256 tokenAmount = (msg.value * 1e18) / price;
    
    // Update state
    contributions[msg.sender] += msg.value;
    tokensPurchased[msg.sender] += tokenAmount;
    totalRaised += msg.value;
    tokensSold += tokenAmount;
    
    emit TokensPurchased(msg.sender, msg.value, tokenAmount);
}

function getCurrentPrice() public view returns (uint256) {
    if (currentStage == Stage.Seed) return SEED_PRICE;
    if (currentStage == Stage.Private) return PRIVATE_PRICE;
    return PUBLIC_PRICE;
}
```

**Hint 3: Vesting Calculation**
```solidity
function calculateClaimable(address user) public view returns (uint256) {
    if (block.timestamp < claimStartTime) return 0;
    
    uint256 purchased = tokensPurchased[user];
    uint256 claimed = tokensClaimed[user];
    
    // 25% at TGE (Token Generation Event)
    uint256 tgeAmount = purchased / 4;
    
    // Remaining 75% vested over 180 days (6 months)
    uint256 timeElapsed = block.timestamp - claimStartTime;
    uint256 vestingDuration = 180 days;
    
    uint256 vestedAmount;
    if (timeElapsed >= vestingDuration) {
        vestedAmount = purchased; // Fully vested
    } else {
        uint256 remainingAfterTGE = purchased - tgeAmount;
        uint256 vestedFromRemaining = (remainingAfterTGE * timeElapsed) / vestingDuration;
        vestedAmount = tgeAmount + vestedFromRemaining;
    }
    
    return vestedAmount - claimed;
}
```

**Hint 4: Refund Logic**
```solidity
function refund() external nonReentrant {
    require(block.timestamp > saleEndTime, "Sale not ended");
    require(totalRaised < SOFT_CAP, "Soft cap reached, no refunds");
    
    uint256 contribution = contributions[msg.sender];
    require(contribution > 0, "No contribution");
    
    contributions[msg.sender] = 0;
    
    (bool success, ) = msg.sender.call{value: contribution}("");
    require(success, "Refund failed");
    
    emit Refunded(msg.sender, contribution);
}
```

**Hint 5: Frontend Progress Bar**
```typescript
function ICOProgress() {
  const { data: totalRaised } = useReadContract({
    address: ICO_CONTRACT,
    abi: icoABI,
    functionName: 'totalRaised',
  });

  const hardCap = parseEther('100'); // 100 ETH
  const raisedETH = formatEther(totalRaised || 0n);
  const percentage = (Number(raisedETH) / 100) * 100;

  return (
    <div>
      <div className="w-full bg-gray-200 rounded-full h-4">
        <div 
          className="bg-blue-600 h-4 rounded-full" 
          style={{ width: `${percentage}%` }}
        />
      </div>
      <p>{raisedETH} ETH / 100 ETH ({percentage.toFixed(1)}%)</p>
    </div>
  );
}
```

**Hint 6: Stage Display**
```typescript
const stageNames = ['Seed Sale', 'Private Sale', 'Public Sale', 'Ended'];
const stagePrices = ['0.0001 ETH', '0.0002 ETH', '0.0005 ETH', '-'];

function CurrentStageDisplay() {
  const { data: stage } = useReadContract({
    address: ICO_CONTRACT,
    abi: icoABI,
    functionName: 'currentStage',
  });

  return (
    <div>
      <h2>Current Stage: {stageNames[stage || 0]}</h2>
      <p>Price: {stagePrices[stage || 0]} per token</p>
    </div>
  );
}
```

---

## 🎨 Bonus Challenges

1. **KYC Integration**: Verify identity before large contributions
2. **Dynamic Pricing**: Bonding curve pricing mechanism
3. **Lottery System**: Fair distribution if oversubscribed
4. **Referral Rewards**: Bonus tokens for referrals
5. **DAO Governance**: Token holders vote on fund usage
6. **Multi-Chain**: Deploy on multiple chains simultaneously

---

## 📚 Reference Links

- **Token Sale Best Practices:** https://github.com/OpenZeppelin/openzeppelin-contracts/tree/master/contracts/token/ERC20
- **CoinList ICO Platform:** https://coinlist.co/
- **Ethereum ICO Security:** https://consensys.net/diligence/blog/2019/09/stop-using-soliditys-transfer-now/
- **Vesting Contracts:** https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/finance/VestingWallet.sol

---

## ✨ Success Criteria

- ✅ Multi-stage sales work correctly
- ✅ Caps enforced properly
- ✅ Vesting schedule accurate
- ✅ Refunds available if soft cap missed
- ✅ No fund loss vulnerabilities
- ✅ Beautiful, professional UI
- ✅ Real-time updates
- ✅ Comprehensive test coverage
- ✅ Deployed and audited

---

*Project for Blockchain Learning Series - Chapter 07*

---

*Congratulations! You've completed all 14 projects in the Blockchain Learning Series. You're now ready to build production-grade blockchain applications!* 🎉