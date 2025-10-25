# Project 10: Token Vesting Contract

**Chapter:** 05 - Intermediate Solidity
**Difficulty:** Intermediate
**Estimated Time:** 5-6 hours

---

## 📋 Project Description

Create a token vesting contract that gradually releases tokens to beneficiaries over time. Common for team allocations, advisor tokens, and investor lockups.

---

## 🎯 Learning Objectives

- Work with time-based logic
- Implement cliff and vesting schedules
- Handle ERC20 token transfers
- Write comprehensive tests

---

## 🛠️ Technology Stack

- Hardhat
- Solidity 0.8.20+
- OpenZeppelin ERC20
- Ethers.js v6
- Chai testing

---

## ✅ TODO List

### Phase 1: Contract Design
- [ ] Define VestingSchedule struct
- [ ] Store beneficiary addresses
- [ ] Track vesting start time, cliff, duration
- [ ] Store total allocation and released amounts

### Phase 2: Core Functions
- [ ] `createVestingSchedule()` - setup vesting for beneficiary
- [ ] `releasableAmount()` - calculate tokens available to release
- [ ] `release()` - transfer vested tokens to beneficiary
- [ ] `vestedAmount()` - total vested so far
- [ ] `revoke()` - cancel vesting (if revocable)

### Phase 3: Vesting Logic
- [ ] Linear vesting calculation
- [ ] Cliff period (e.g., nothing for first 6 months)
- [ ] After cliff, tokens unlock gradually
- [ ] Handle different schedules per beneficiary

### Phase 4: Events & Safety
- [ ] VestingScheduleCreated event
- [ ] TokensReleased event
- [ ] VestingRevoked event
- [ ] Access control (only owner creates schedules)
- [ ] Prevent double-release

### Phase 5: Testing
- [ ] Test vesting creation
- [ ] Test release before cliff (should fail)
- [ ] Test release after cliff
- [ ] Test gradual unlocking
- [ ] Test full vesting completion
- [ ] Test revocation
- [ ] Test multiple beneficiaries
- [ ] Time manipulation in tests

### Phase 6: Deployment
- [ ] Deploy token contract
- [ ] Deploy vesting contract
- [ ] Transfer tokens to vesting contract
- [ ] Create sample schedules
- [ ] Verify on Sepolia

---

## 💡 Hints

**Hint 1: Vesting Schedule Struct**
```solidity
struct VestingSchedule {
    address beneficiary;
    uint256 cliff;          // Cliff duration in seconds
    uint256 start;          // Start timestamp
    uint256 duration;       // Total vesting duration
    uint256 totalAmount;    // Total tokens to vest
    uint256 released;       // Tokens already released
    bool revocable;
    bool revoked;
}

mapping(bytes32 => VestingSchedule) public vestingSchedules;
```

**Hint 2: Vested Amount Calculation**
```solidity
function vestedAmount(bytes32 scheduleId) public view returns (uint256) {
    VestingSchedule memory schedule = vestingSchedules[scheduleId];
    
    if (block.timestamp < schedule.start + schedule.cliff) {
        return 0; // Still in cliff period
    }
    
    if (block.timestamp >= schedule.start + schedule.duration) {
        return schedule.totalAmount; // Fully vested
    }
    
    // Linear vesting
    uint256 timeVested = block.timestamp - schedule.start;
    return (schedule.totalAmount * timeVested) / schedule.duration;
}
```

**Hint 3: Releasable Amount**
```solidity
function releasableAmount(bytes32 scheduleId) public view returns (uint256) {
    uint256 vested = vestedAmount(scheduleId);
    uint256 released = vestingSchedules[scheduleId].released;
    return vested - released;
}
```

**Hint 4: Time Travel in Tests**
```javascript
// Hardhat helper to advance time
await ethers.provider.send("evm_increaseTime", [30 * 24 * 60 * 60]); // 30 days
await ethers.provider.send("evm_mine");

// Test after 6 months
await ethers.provider.send("evm_increaseTime", [180 * 24 * 60 * 60]);
await ethers.provider.send("evm_mine");

const releasable = await vesting.releasableAmount(scheduleId);
expect(releasable).to.be.gt(0);
```

**Hint 5: Example Vesting Scenarios**
```
Team Tokens: 1 year cliff, 4 year vesting
Advisors: 6 month cliff, 2 year vesting
Investors: 3 month cliff, 18 month vesting

Example: 1,000,000 tokens, 1 year cliff, 4 year vesting
- Month 0-12: 0 tokens released
- Month 13: 250,000 tokens available (25%)
- Month 25: 500,000 tokens available (50%)
- Month 48: 1,000,000 tokens available (100%)
```

---

## 🎨 Bonus Challenges

1. **Multiple Schedules**: One beneficiary can have multiple schedules
2. **Batch Create**: Add multiple beneficiaries at once
3. **Dashboard**: Frontend to view vesting progress
4. **Non-Linear Vesting**: Exponential or custom curves
5. **Delegation**: Beneficiary can delegate voting power while vesting
6. **CSV Import**: Import vesting schedules from CSV

---

## 📚 Reference Links

- **Token Vesting Tutorial:** https://docs.openzeppelin.com/contracts/5.x/api/finance#VestingWallet
- **Sablier (Production Vesting):** https://github.com/sablier-labs/v2-core
- **Time-Based Testing:** https://hardhat.org/hardhat-network/docs/reference#evm_increasetime

---

## ✨ Success Criteria

- ✅ Correct vesting calculations
- ✅ Cliff period enforced
- ✅ Linear vesting works
- ✅ Revocation functional
- ✅ All tests passing
- ✅ Events emitted
- ✅ Gas efficient

---

*Project for Blockchain Learning Series - Chapter 05*