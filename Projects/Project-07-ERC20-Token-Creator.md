# Project 07: Simple ERC20 Token Creator

**Chapter:** 04 - Solidity Basics
**Difficulty:** Beginner
**Estimated Time:** 3-4 hours

---

## 📋 Project Description

Create your first ERC20 token from scratch using Solidity. Learn token standards, implement core functions, and deploy to testnet using Remix IDE.

---

## 🎯 Learning Objectives

- Understand ERC20 standard
- Write basic Solidity contracts
- Implement token transfer logic
- Deploy to Sepolia testnet

---

## 🛠️ Technology Stack

- Solidity 0.8.20+
- Remix IDE
- MetaMask
- Sepolia testnet
- OpenZeppelin (optional reference)

---

## ✅ TODO List

### Phase 1: Contract Structure
- [ ] Create contract with constructor
- [ ] Define state variables (name, symbol, decimals, totalSupply)
- [ ] Implement mapping for balances
- [ ] Set up allowances mapping

### Phase 2: Core Functions
- [ ] `totalSupply()` - return total tokens
- [ ] `balanceOf(address)` - check balance
- [ ] `transfer(to, amount)` - send tokens
- [ ] `approve(spender, amount)` - allow spending
- [ ] `allowance(owner, spender)` - check allowance
- [ ] `transferFrom(from, to, amount)` - delegated transfer

### Phase 3: Events
- [ ] `Transfer` event
- [ ] `Approval` event
- [ ] Emit events in appropriate functions

### Phase 4: Safety
- [ ] Require statements for validation
- [ ] Check for zero address
- [ ] Ensure sufficient balance
- [ ] Prevent overflow (Solidity 0.8+ auto-checks)

### Phase 5: Deployment
- [ ] Get Sepolia ETH from faucet
- [ ] Compile in Remix
- [ ] Deploy with initial supply
- [ ] Test all functions
- [ ] Verify on Etherscan

---

## 💡 Hints

**Hint 1: Contract Structure**
```solidity
contract MyToken {
    string public name;
    string public symbol;
    uint8 public decimals = 18;
    uint256 public totalSupply;
    
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}
```

**Hint 2: Transfer Function Logic**
```
1. Check sender has enough balance
2. Subtract from sender
3. Add to recipient
4. Emit Transfer event
```

**Hint 3: Approval Pattern**
```
User A has tokens
User A approves Contract B to spend X tokens
Contract B calls transferFrom(A, C, amount)
Contract checks allowance
Transfers tokens from A to C
```

**Hint 4: Constructor Minting**
```solidity
constructor(uint256 initialSupply) {
    totalSupply = initialSupply * 10**decimals;
    balanceOf[msg.sender] = totalSupply;
    emit Transfer(address(0), msg.sender, totalSupply);
}
```

**Hint 5: Common Validations**
- `require(to != address(0), "Invalid recipient");`
- `require(amount <= balanceOf[msg.sender], "Insufficient balance");`
- `require(amount > 0, "Amount must be positive");`

---

## 🎨 Bonus Challenges

1. **Mint Function**: Allow owner to create new tokens
2. **Burn Function**: Destroy tokens to reduce supply
3. **Pause Mechanism**: Emergency stop functionality
4. **Max Supply Cap**: Prevent unlimited minting
5. **Token Sale**: Simple ICO contract that sells your token
6. **Vesting**: Lock tokens for team members

---

## 📚 Reference Links

- **ERC20 Standard:** https://eips.ethereum.org/EIPS/eip-20
- **OpenZeppelin ERC20:** https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol
- **Remix IDE:** https://remix.ethereum.org/
- **Sepolia Faucet:** https://sepoliafaucet.com/

---

## ✨ Success Criteria

- ✅ All ERC20 functions implemented
- ✅ Events properly emitted
- ✅ No compilation warnings
- ✅ Deployed to Sepolia
- ✅ All functions tested
- ✅ Verified on Etherscan

---

*Project for Blockchain Learning Series - Chapter 04*