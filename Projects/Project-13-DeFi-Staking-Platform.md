# Project 13: DeFi Staking Platform

**Chapter:** 07 - Advanced DeFi & DePIN
**Difficulty:** Expert
**Estimated Time:** 12-15 hours

---

## 📋 Project Description

Build a complete DeFi staking platform where users can stake tokens, earn rewards over time, and unstake. Includes reward calculation, APY display, and full-stack interface.

---

## 🎯 Learning Objectives

- Implement staking mechanics
- Calculate rewards with compound interest
- Handle token deposits and withdrawals
- Build DeFi UI/UX
- Integrate with DeFi protocols

---

## 🛠️ Technology Stack

- Solidity 0.8.20+
- Hardhat
- OpenZeppelin (ERC20, ReentrancyGuard, Ownable)
- Next.js 14+ with TypeScript
- RainbowKit + Wagmi + Viem
- Chart.js for APY visualization
- Tailwind CSS

---

## ✅ TODO List

### Phase 1: Token Contracts
- [ ] Deploy staking token (e.g., STAKE)
- [ ] Deploy reward token (can be same token)
- [ ] Mint initial supply
- [ ] Approve staking contract

### Phase 2: Staking Contract
- [ ] State variables (totalStaked, rewardRate, etc.)
- [ ] Staker struct (amount, rewardDebt, lastUpdate)
- [ ] `stake(amount)` function
- [ ] `unstake(amount)` function
- [ ] `claimRewards()` function
- [ ] `calculateRewards(address)` view function
- [ ] Emergency withdraw

### Phase 3: Reward Mechanism
- [ ] Set reward rate (e.g., 10% APY)
- [ ] Track staking duration
- [ ] Calculate rewards proportionally
- [ ] Handle compound rewards (optional)
- [ ] Owner can adjust reward rate

### Phase 4: Security
- [ ] ReentrancyGuard on all functions
- [ ] Check sufficient token balance
- [ ] Prevent staking 0 tokens
- [ ] SafeERC20 for transfers
- [ ] Pause mechanism

### Phase 5: Testing
- [ ] Test staking tokens
- [ ] Test reward calculation accuracy
- [ ] Test unstaking
- [ ] Test multiple stakers
- [ ] Test reward rate changes
- [ ] Test edge cases (stake/unstake same block)

### Phase 6: Frontend
- [ ] Connect wallet
- [ ] Display user's token balance
- [ ] Stake input and button
- [ ] Show staked amount
- [ ] Display pending rewards (live counter)
- [ ] Claim rewards button
- [ ] Unstake functionality
- [ ] Pool statistics (TVL, APY, your share)

### Phase 7: Advanced UI
- [ ] Chart showing reward accumulation
- [ ] APY calculator (input amount, see returns)
- [ ] Historical staking data
- [ ] Leaderboard (top stakers)
- [ ] Dark mode
- [ ] Mobile responsive

---

## 💡 Hints

**Hint 1: Staking Contract Structure**
```solidity
contract StakingPool is ReentrancyGuard, Ownable {
    IERC20 public stakingToken;
    IERC20 public rewardToken;
    
    uint256 public rewardRate = 100; // 100 tokens per day per 1 token staked
    uint256 public totalStaked;
    
    struct Staker {
        uint256 amount;
        uint256 rewardDebt;
        uint256 lastUpdateTime;
    }
    
    mapping(address => Staker) public stakers;
    
    event Staked(address indexed user, uint256 amount);
    event Unstaked(address indexed user, uint256 amount);
    event RewardsClaimed(address indexed user, uint256 amount);
}
```

**Hint 2: Reward Calculation**
```solidity
function calculateReward(address user) public view returns (uint256) {
    Staker memory staker = stakers[user];
    if (staker.amount == 0) return 0;
    
    uint256 timeElapsed = block.timestamp - staker.lastUpdateTime;
    uint256 reward = (staker.amount * rewardRate * timeElapsed) / 1 days / 1e18;
    
    return reward + staker.rewardDebt;
}
```

**Hint 3: Stake Function**
```solidity
function stake(uint256 amount) external nonReentrant {
    require(amount > 0, "Cannot stake 0");
    
    // Claim pending rewards first
    if (stakers[msg.sender].amount > 0) {
        uint256 pending = calculateReward(msg.sender);
        if (pending > 0) {
            stakers[msg.sender].rewardDebt = 0;
            rewardToken.transfer(msg.sender, pending);
        }
    }
    
    // Transfer tokens
    stakingToken.transferFrom(msg.sender, address(this), amount);
    
    // Update state
    stakers[msg.sender].amount += amount;
    stakers[msg.sender].lastUpdateTime = block.timestamp;
    totalStaked += amount;
    
    emit Staked(msg.sender, amount);
}
```

**Hint 4: Live Reward Counter (Frontend)**
```typescript
import { useState, useEffect } from 'react';

function RewardCounter({ stakedAmount, rewardRate, lastUpdateTime }: Props) {
  const [rewards, setRewards] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      const timeElapsed = Date.now() / 1000 - lastUpdateTime;
      const calculated = (stakedAmount * rewardRate * timeElapsed) / 86400 / 1e18;
      setRewards(calculated);
    }, 1000); // Update every second

    return () => clearInterval(interval);
  }, [stakedAmount, rewardRate, lastUpdateTime]);

  return <div>Pending Rewards: {rewards.toFixed(6)} REWARD</div>;
}
```

**Hint 5: APY Calculation**
```typescript
// APY = (1 + rate/periods)^periods - 1
// For daily compounding:
const dailyRate = rewardRate / 365;
const APY = ((1 + dailyRate) ** 365 - 1) * 100;

// Simple APY (no compounding):
const simpleAPY = (rewardRate / stakedAmount) * 365 * 100;
```

**Hint 6: Pool Statistics**
```typescript
function PoolStats() {
  const { data: totalStaked } = useReadContract({
    address: STAKING_CONTRACT,
    abi: stakingABI,
    functionName: 'totalStaked',
  });

  const { data: rewardRate } = useReadContract({
    address: STAKING_CONTRACT,
    abi: stakingABI,
    functionName: 'rewardRate',
  });

  const TVL = formatEther(totalStaked || 0n);
  const APY = calculateAPY(rewardRate);

  return (
    <div>
      <p>Total Value Locked: {TVL} STAKE</p>
      <p>Current APY: {APY}%</p>
    </div>
  );
}
```

---

## 🎨 Bonus Challenges

1. **Lock Periods**: Higher APY for longer lockups (30/60/90 days)
2. **NFT Boosters**: Staking with NFT increases rewards
3. **Compound Button**: Auto-restake rewards
4. **Referral System**: Earn bonus for referring stakers
5. **Governance**: Stakers vote on reward rate
6. **Multi-Token Support**: Stake multiple different tokens

---

## 📚 Reference Links

- **Synthetix Staking:** https://github.com/Synthetixio/synthetix/blob/master/contracts/StakingRewards.sol
- **MasterChef (Sushi):** https://github.com/sushiswap/sushiswap/blob/master/protocols/masterchef/contracts/MasterChef.sol
- **Aave Staking:** https://github.com/aave/aave-stake-v2
- **DeFi Best Practices:** https://github.com/OpenZeppelin/openzeppelin-contracts

---

## ✨ Success Criteria

- ✅ Accurate reward calculations
- ✅ No fund loss bugs
- ✅ Reentrancy protected
- ✅ Live reward counter
- ✅ Clean, intuitive UI
- ✅ All tests passing
- ✅ Gas optimized
- ✅ Deployed and verified

---

*Project for Blockchain Learning Series - Chapter 07*