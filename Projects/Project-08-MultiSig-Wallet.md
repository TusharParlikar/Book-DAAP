# Project 08: Multi-Signature Wallet Contract

**Chapter:** 04 - Solidity Basics
**Difficulty:** Intermediate
**Estimated Time:** 5-6 hours

---

## 📋 Project Description

Build a secure multi-signature wallet that requires multiple owners to approve transactions before execution. Great practice for access control, state management, and Solidity patterns.

---

## 🎯 Learning Objectives

- Implement access control patterns
- Manage contract state
- Work with structs and arrays
- Handle ETH transfers in contracts

---

## 🛠️ Technology Stack

- Solidity 0.8.20+
- Remix IDE
- MetaMask
- Sepolia testnet

---

## ✅ TODO List

### Phase 1: Contract Setup
- [ ] Define owners array
- [ ] Set required confirmations (e.g., 2 of 3)
- [ ] Create Transaction struct
- [ ] Initialize in constructor

### Phase 2: Transaction Submission
- [ ] `submitTransaction(to, value, data)` function
- [ ] Store transaction in array
- [ ] Emit TransactionSubmitted event
- [ ] Only owners can submit

### Phase 3: Confirmation System
- [ ] `confirmTransaction(txId)` function
- [ ] Track confirmations per transaction
- [ ] Prevent double confirmation
- [ ] Emit TransactionConfirmed event

### Phase 4: Execution
- [ ] `executeTransaction(txId)` function
- [ ] Check if enough confirmations
- [ ] Execute the transaction
- [ ] Emit TransactionExecuted event
- [ ] Handle execution failures

### Phase 5: Additional Features
- [ ] `revokeConfirmation(txId)` - undo confirmation
- [ ] `getTransactionCount()` - total transactions
- [ ] `getTransaction(txId)` - view details
- [ ] `isConfirmed(txId)` - check status
- [ ] Receive ETH function

---

## 💡 Hints

**Hint 1: Transaction Struct**
```solidity
struct Transaction {
    address to;
    uint256 value;
    bytes data;
    bool executed;
    uint256 confirmations;
}

Transaction[] public transactions;
mapping(uint256 => mapping(address => bool)) public isConfirmed;
```

**Hint 2: Modifiers**
```solidity
modifier onlyOwner() {
    require(isOwner[msg.sender], "Not owner");
    _;
}

modifier txExists(uint256 txId) {
    require(txId < transactions.length, "Transaction does not exist");
    _;
}

modifier notExecuted(uint256 txId) {
    require(!transactions[txId].executed, "Already executed");
    _;
}
```

**Hint 3: Confirmation Logic**
```
1. Check transaction exists
2. Check not already confirmed by this owner
3. Check transaction not executed
4. Mark as confirmed
5. Increment confirmation count
```

**Hint 4: Execution Logic**
```solidity
Transaction storage transaction = transactions[txId];
require(transaction.confirmations >= required, "Not enough confirmations");

(bool success, ) = transaction.to.call{value: transaction.value}(transaction.data);
require(success, "Transaction failed");

transaction.executed = true;
```

**Hint 5: Testing Scenarios**
- Deploy with 3 owners, require 2 confirmations
- Submit transaction to send ETH
- Confirm from 2 different owners
- Execute transaction
- Try to execute again (should fail)
- Try to confirm after execution (should fail)

---

## 🎨 Bonus Challenges

1. **Add Owner**: Function to add new owner (requires confirmations)
2. **Remove Owner**: Remove an owner with approval
3. **Change Requirement**: Update confirmation threshold
4. **Daily Limit**: Small amounts don't need confirmations
5. **Time Lock**: Delay execution by X hours after confirmations
6. **Frontend**: Build UI to interact with wallet

---

## 📚 Reference Links

- **Gnosis Safe (Production MultiSig):** https://github.com/safe-global/safe-contracts
- **Solidity by Example:** https://solidity-by-example.org/app/multi-sig-wallet/
- **OpenZeppelin Access Control:** https://docs.openzeppelin.com/contracts/5.x/access-control

---

## ✨ Success Criteria

- ✅ Multiple owners configured
- ✅ Transaction submission works
- ✅ Confirmation system functional
- ✅ Executes only with enough confirmations
- ✅ Prevents replay attacks
- ✅ Handles ETH correctly
- ✅ All edge cases handled

---

*Project for Blockchain Learning Series - Chapter 04*