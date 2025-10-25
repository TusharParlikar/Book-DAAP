# Project 02: Simple Blockchain Simulator

**Chapter:** 01 - Blockchain Fundamentals
**Difficulty:** Beginner-Intermediate
**Estimated Time:** 4-6 hours

---

## 📋 Project Description

Create a functional blockchain simulator in JavaScript that demonstrates how blocks are linked together through hashing. Users can add transactions, mine blocks, and see how tampering with data breaks the chain.

---

## 🎯 Learning Objectives

- Understand block structure
- Implement proof-of-work mining
- Demonstrate immutability through chain validation
- Visualize blockchain concepts

---

## 🛠️ Technology Stack

- HTML5/CSS3
- JavaScript (ES6+)
- CryptoJS for SHA-256
- Optional: React or Vue.js

---

## ✅ TODO List

### Phase 1: Block Class
- [ ] Create Block class with properties: index, timestamp, data, previousHash, hash, nonce
- [ ] Implement calculateHash() method
- [ ] Implement mineBlock(difficulty) method

### Phase 2: Blockchain Class
- [ ] Create Blockchain class
- [ ] Implement createGenesisBlock() method
- [ ] Implement getLatestBlock() method
- [ ] Implement addBlock(newBlock) method
- [ ] Implement isChainValid() method

### Phase 3: UI Components
- [ ] Display current blockchain (all blocks)
- [ ] Show block details (index, timestamp, hash, previous hash)
- [ ] Add transaction form (add data to pending transactions)
- [ ] Mine block button
- [ ] Difficulty selector (1-5 leading zeros)

### Phase 4: Tampering Demo
- [ ] Add "Edit Block Data" feature
- [ ] Show chain validity status (valid/invalid)
- [ ] Highlight invalid blocks in red
- [ ] Explain why chain is broken

### Phase 5: Advanced Features
- [ ] Show mining progress (hash attempts counter)
- [ ] Display mining time
- [ ] Add multiple transactions per block
- [ ] Export/Import blockchain as JSON

---

## 💡 Hints

**Hint 1: Block Structure**
```javascript
class Block {
    constructor(index, timestamp, data, previousHash = '') {
        this.index = index;
        this.timestamp = timestamp;
        this.data = data;
        this.previousHash = previousHash;
        this.hash = this.calculateHash();
        this.nonce = 0;
    }
    
    // Calculate hash from block data
    calculateHash() {
        // Combine all properties and hash them
        // Use CryptoJS.SHA256(...)
    }
    
    // Keep trying nonces until hash meets difficulty
    mineBlock(difficulty) {
        // Loop until hash starts with N zeros
        // Increment nonce each time
    }
}
```

**Hint 2: Proof of Work**
- Difficulty = number of leading zeros required
- Example: difficulty 3 means hash must start with "000"
- Keep incrementing nonce until condition is met
- This is how Bitcoin mining works (simplified)!

**Hint 3: Chain Validation**
- Check each block's hash is correctly calculated
- Check each block's previousHash matches previous block's hash
- Genesis block is a special case (previousHash = "0")

**Hint 4: Visual Representation**
- Use boxes/cards for each block
- Show arrows connecting blocks
- Use colors: green for valid, red for invalid
- Animate block creation

**Hint 5: Genesis Block**
```javascript
createGenesisBlock() {
    return new Block(0, "01/01/2009", "Genesis Block", "0");
}
```

---

## 🎨 Bonus Challenges

1. **Transaction Pool**: Implement mempool concept
2. **Network Simulation**: Simulate multiple nodes with different chain states
3. **Fork Handling**: Show what happens when chains diverge
4. **Block Size Limit**: Limit transactions per block
5. **Merkle Tree**: Implement merkle root for transactions
6. **Persistence**: Save blockchain to localStorage

---

## 📚 Reference Links

- **Anders Brownworth's Blockchain Demo:** https://andersbrownworth.com/blockchain/
- **Blockchain from Scratch Tutorial:** https://www.youtube.com/watch?v=zVqczFZr124

---

## ✨ Success Criteria

- ✅ Can create and mine new blocks
- ✅ Blocks are correctly linked via hashes
- ✅ Tampering with any block invalidates the chain
- ✅ Mining difficulty is adjustable
- ✅ Clear visual feedback of chain validity
- ✅ Code is well-organized and commented

---

*Project for Blockchain Learning Series - Chapter 01*
*Last Updated: October 25, 2025*