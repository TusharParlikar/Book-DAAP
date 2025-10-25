# Chapter 3.5: Solidity Deep Dive - From Zero to Hero 🔥

**Hey!** Want to ACTUALLY write smart contracts? This is your chapter! Pure Solidity from basics to advanced! 💪

[← Chapter 03](Chapter-03-Ethereum-Smart-Contracts.md) | [Index](../Index.md) | [Next: Using Contracts →](Chapter-04-JavaScript-DApp-Development.md)

---

## 👋 Welcome to Solidity!

**Real talk:** Most developers USE contracts, but some want to WRITE them. If that's you, buckle up! 🚀

### What You'll Learn:

- ✅ Solidity syntax (variables, functions, modifiers)
- ✅ Remix IDE (browser-based Solidity editor)
- ✅ Hardhat (professional development environment)
- ✅ Writing ERC20 tokens (create your own crypto!)
- ✅ Testing contracts (catch bugs before deployment)
- ✅ Security best practices (don't get hacked!)
- ✅ Deploying to testnets (real blockchain, fake money)

**Time:** 2-3 hours • **Prerequisites:** Chapter 03

---

## 📄 Understanding the Whitepapers

Before we code, let's understand the foundational documents:

### Bitcoin Whitepaper (2008)

**"Bitcoin: A Peer-to-Peer Electronic Cash System" by Satoshi Nakamoto**

📖 Read it: https://bitcoin.org/bitcoin.pdf (Only 9 pages!)

**Key Concepts:**
```javascript
const bitcoinWhitepaper = {
  problem: 'Need electronic payments without financial institutions',
  solution: 'Peer-to-peer network using proof-of-work',
  
  mainIdeas: {
    1: 'Transactions: Digital signatures prove ownership',
    2: 'Timestamp Server: Orders transactions chronologically',
    3: 'Proof-of-Work: Miners solve puzzles to add blocks',
    4: 'Network: Longest chain = valid chain',
    5: 'Incentive: Miners rewarded with new coins',
    6: 'Privacy: Public keys, not identities'
  },
  
  revolutionaryIdea: 'Double-spending solved without central authority!',
  
  impact: [
    'Created $500B+ cryptocurrency market',
    'Inspired 20,000+ cryptocurrencies',
    'Led to Ethereum and smart contracts'
  ]
};
```

**Why Read It?**
- Only 9 pages, very readable
- Understand blockchain foundations
- See how simple ideas = massive impact

---

### Ethereum Whitepaper (2013)

**"Ethereum: A Next-Generation Smart Contract and Decentralized Application Platform" by Vitalik Buterin**

📖 Read it: https://ethereum.org/en/whitepaper/

**Key Concepts:**
```javascript
const ethereumWhitepaper = {
  problem: 'Bitcoin limited to currency, need programmable blockchain',
  solution: 'Turing-complete blockchain (can run any code)',
  
  mainIdeas: {
    1: 'Accounts: Two types (EOA and Contract)',
    2: 'Messages: Transactions trigger code execution',
    3: 'EVM: Virtual machine runs smart contracts',
    4: 'Gas: Prevents infinite loops, pays for computation',
    5: 'Turing Complete: Can implement any algorithm',
    6: 'State Machine: World state updated with each block'
  },
  
  revolutionaryIdea: 'Blockchain as world computer!',
  
  useCases: [
    'Financial derivatives',
    'Decentralized organizations (DAOs)',
    'Savings wallets with rules',
    'Crop insurance',
    'Decentralized data feeds',
    'Anything you can program!'
  ]
};
```

**Why Read It?**
- Understand smart contract philosophy
- See Vitalik's vision (most happened!)
- Grasp EVM design decisions

---

### Solidity Documentation

📖 Official Docs: https://docs.soliditylang.org/

**Key Sections to Read:**
- Introduction
- Layout of a Solidity Source File
- Types
- Units and Global Variables
- Control Structures
- Contracts

---

## 🛠️ Setup: Remix IDE (No Installation!)

### What is Remix?

**Remix = Browser-based Solidity IDE**

**Features:**
- Write Solidity code
- Compile to bytecode
- Deploy to blockchain
- Test functions
- Debug transactions
- All in your browser!

### Getting Started with Remix

```javascript
// Step 1: Open Remix
// Go to: https://remix.ethereum.org

// Step 2: Create new file
// Click "contracts" folder → New File → "MyFirstContract.sol"

// Step 3: Write your first contract
```

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

// Your first smart contract!
contract HelloWorld {
    // State variable (stored on blockchain forever!)
    string public message;
    
    // Constructor (runs once when deployed)
    constructor() {
        message = "Hello, Blockchain!";
    }
    
    // Function to update message
    function setMessage(string memory newMessage) public {
        message = newMessage;
    }
    
    // Function to read message (free!)
    function getMessage() public view returns (string memory) {
        return message;
    }
}
```

**Understanding:** This simple contract stores a string on the blockchain. The `public` keyword automatically creates a getter function.

### Compile and Deploy in Remix

**Step 1: Compile**
1. Click "Solidity Compiler" tab (left sidebar)
2. Select compiler version: 0.8.20+
3. Click "Compile MyFirstContract.sol"
4. ✅ Green checkmark = success!

**Step 2: Deploy**
1. Click "Deploy & Run" tab
2. Environment: "Remix VM (Shanghai)" (fake blockchain in browser!)
3. Click "Deploy"
4. See your contract appear below!

**Step 3: Interact**
1. Click dropdown arrow on deployed contract
2. Type new message in `setMessage` field
3. Click "setMessage" button (orange = costs gas)
4. Click "getMessage" button (blue = free!)
5. See your message! 🎉

---

## 📚 Solidity Basics

### Data Types

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract DataTypes {
    // 1. INTEGERS
    uint256 public unsignedInt = 100;        // Positive only (0 to 2^256-1)
    int256 public signedInt = -50;            // Can be negative
    
    uint8 public smallNumber = 255;           // 0 to 255 (saves gas!)
    uint public defaultUint = 500;            // Same as uint256
    
    // 2. BOOLEAN
    bool public isActive = true;
    bool public hasAccess = false;
    
    // 3. ADDRESS
    address public owner = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;
    address payable public wallet;            // Can receive ETH
    
    // 4. STRINGS
    string public name = "My Token";
    string public symbol = "MTK";
    
    // 5. BYTES
    bytes32 public hash;                      // Fixed-size (gas efficient)
    bytes public data;                        // Dynamic size
    
    // 6. ARRAYS
    uint[] public dynamicArray;               // Can grow
    uint[5] public fixedArray;                // Fixed size
    
    // 7. MAPPINGS (like JavaScript objects)
    mapping(address => uint) public balances;
    mapping(address => bool) public whitelist;
    
    // 8. STRUCTS (custom types)
    struct User {
        string name;
        uint age;
        bool isActive;
    }
    
    User public user1;
    
    // 9. ENUMS (predefined options)
    enum Status { Pending, Active, Completed, Cancelled }
    Status public currentStatus = Status.Pending;
}
```

---

### Functions

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Functions {
    uint public count = 0;
    address public owner;
    
    constructor() {
        owner = msg.sender;  // Who deployed the contract
    }
    
    // 1. PUBLIC FUNCTION (anyone can call)
    function increment() public {
        count += 1;
    }
    
    // 2. EXTERNAL FUNCTION (only from outside contract)
    function externalFunc() external pure returns (string memory) {
        return "Called from outside";
    }
    
    // 3. INTERNAL FUNCTION (only from this contract or derived contracts)
    function internalFunc() internal pure returns (uint) {
        return 42;
    }
    
    // 4. PRIVATE FUNCTION (only from this contract)
    function privateFunc() private pure returns (uint) {
        return 100;
    }
    
    // 5. VIEW FUNCTION (reads state, doesn't modify)
    function getCount() public view returns (uint) {
        return count;  // FREE to call!
    }
    
    // 6. PURE FUNCTION (no state access at all)
    function add(uint a, uint b) public pure returns (uint) {
        return a + b;  // Just math, no blockchain data
    }
    
    // 7. PAYABLE FUNCTION (can receive ETH)
    function deposit() public payable {
        // msg.value = amount of ETH sent
    }
    
    // 8. FUNCTION WITH MULTIPLE RETURNS
    function getInfo() public view returns (uint, address, string memory) {
        return (count, owner, "Hello");
    }
    
    // 9. FUNCTION WITH MODIFIERS
    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner!");
        _;  // Continue function execution
    }
    
    function reset() public onlyOwner {
        count = 0;
    }
}
```

---

### Events (Logging)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Events {
    // Define events (like console.log but on blockchain)
    event Transfer(address indexed from, address indexed to, uint amount);
    event Approval(address indexed owner, address indexed spender, uint amount);
    event MessageSent(string message, address sender);
    
    mapping(address => uint) public balances;
    
    function transfer(address to, uint amount) public {
        require(balances[msg.sender] >= amount, "Insufficient balance");
        
        balances[msg.sender] -= amount;
        balances[to] += amount;
        
        // Emit event (frontend can listen to this!)
        emit Transfer(msg.sender, to, amount);
    }
    
    function sendMessage(string memory message) public {
        emit MessageSent(message, msg.sender);
    }
}
```

---

## 🪙 Creating ERC20 Token (Your Own Crypto!)

### Simple ERC20 from Scratch

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SimpleERC20 {
    // Token info
    string public name = "My Token";
    string public symbol = "MTK";
    uint8 public decimals = 18;
    uint256 public totalSupply;
    
    // Balances
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    
    // Events
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    
    // Constructor: Create initial supply
    constructor(uint256 initialSupply) {
        totalSupply = initialSupply * 10 ** decimals;
        balanceOf[msg.sender] = totalSupply;  // Give all tokens to creator
    }
    
    // Transfer tokens
    function transfer(address to, uint256 amount) public returns (bool) {
        require(balanceOf[msg.sender] >= amount, "Insufficient balance");
        
        balanceOf[msg.sender] -= amount;
        balanceOf[to] += amount;
        
        emit Transfer(msg.sender, to, amount);
        return true;
    }
    
    // Approve someone to spend your tokens
    function approve(address spender, uint256 amount) public returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }
    
    // Transfer tokens on behalf of someone (if approved)
    function transferFrom(address from, address to, uint256 amount) public returns (bool) {
        require(balanceOf[from] >= amount, "Insufficient balance");
        require(allowance[from][msg.sender] >= amount, "Allowance exceeded");
        
        balanceOf[from] -= amount;
        balanceOf[to] += amount;
        allowance[from][msg.sender] -= amount;
        
        emit Transfer(from, to, amount);
        return true;
    }
}
```

**Understanding:** This simple implementation shows the core ERC20 functions - transfer, approve, and transferFrom.

**Deploy in Remix:**
1. Type (don't copy-paste!) the code to Remix
2. Compile (0.8.20+)
3. Deploy with `initialSupply` = 1000000 (1 million tokens!)
4. Experiment with `transfer`, `approve`, `transferFrom`

**Learning by typing:** You'll remember better when you type each line yourself!

---

### Professional ERC20 with OpenZeppelin

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract MyToken is ERC20, Ownable {
    constructor(uint256 initialSupply) 
        ERC20("My Token", "MTK")
        Ownable(msg.sender)
    {
        _mint(msg.sender, initialSupply * 10 ** decimals());
    }
    
    // Only owner can mint new tokens
    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
    }
    
    // Burn (destroy) tokens
    function burn(uint256 amount) public {
        _burn(msg.sender, amount);
    }
}
```

**OpenZeppelin = Battle-tested, secure contracts!**

---

## 🔧 Hardhat Setup (Professional Dev Environment)

### What is Hardhat?

**Hardhat = Professional Solidity development toolkit**

**Features:**
- Local blockchain (like Remix VM but better)
- Testing framework (catch bugs!)
- Deployment scripts (automated deployments)
- Console debugging (print statements!)
- Mainnet forking (test with real data)

---

### Installing Hardhat

```bash
# Step 1: Create project folder
mkdir my-token-project
cd my-token-project

# Step 2: Initialize npm
npm init -y

# Step 3: Install Hardhat
npm install --save-dev hardhat

# Step 4: Create Hardhat project
npx hardhat init

# Choose: "Create a TypeScript project"
# Press Enter for all questions (accept defaults)

# Step 5: Install OpenZeppelin
npm install @openzeppelin/contracts

# Step 6: Install testing libraries
npm install --save-dev @nomicfoundation/hardhat-toolbox
```

---

### Project Structure

```
my-token-project/
├── contracts/          # Your Solidity files
│   └── MyToken.sol
├── test/              # Test files
│   └── MyToken.test.ts
├── scripts/           # Deployment scripts
│   └── deploy.ts
├── hardhat.config.ts  # Configuration
└── package.json
```

---

### Writing a Contract in Hardhat

```solidity
// contracts/MyToken.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract MyToken is ERC20, Ownable {
    uint256 public constant MAX_SUPPLY = 1000000 * 10 ** 18;  // 1 million
    
    constructor() 
        ERC20("My Token", "MTK")
        Ownable(msg.sender)
    {
        _mint(msg.sender, 100000 * 10 ** decimals());  // Initial: 100k
    }
    
    function mint(address to, uint256 amount) public onlyOwner {
        require(totalSupply() + amount <= MAX_SUPPLY, "Max supply exceeded");
        _mint(to, amount);
    }
}
```

---

### Testing Your Contract

```typescript
// test/MyToken.test.ts
import { expect } from "chai";
import { ethers } from "hardhat";

describe("MyToken", function () {
  it("Should deploy with correct name and symbol", async function () {
    const MyToken = await ethers.getContractFactory("MyToken");
    const token = await MyToken.deploy();
    
    expect(await token.name()).to.equal("My Token");
    expect(await token.symbol()).to.equal("MTK");
  });
  
  it("Should mint initial supply to owner", async function () {
    const MyToken = await ethers.getContractFactory("MyToken");
    const token = await MyToken.deploy();
    const [owner] = await ethers.getSigners();
    
    const balance = await token.balanceOf(owner.address);
    expect(balance).to.equal(ethers.parseEther("100000"));
  });
  
  it("Should transfer tokens", async function () {
    const MyToken = await ethers.getContractFactory("MyToken");
    const token = await MyToken.deploy();
    const [owner, addr1] = await ethers.getSigners();
    
    await token.transfer(addr1.address, ethers.parseEther("1000"));
    expect(await token.balanceOf(addr1.address)).to.equal(ethers.parseEther("1000"));
  });
  
  it("Should fail if sender doesn't have enough tokens", async function () {
    const MyToken = await ethers.getContractFactory("MyToken");
    const token = await MyToken.deploy();
    const [owner, addr1, addr2] = await ethers.getSigners();
    
    await expect(
      token.connect(addr1).transfer(addr2.address, ethers.parseEther("1"))
    ).to.be.revertedWith("ERC20: transfer amount exceeds balance");
  });
  
  it("Should allow minting by owner only", async function () {
    const MyToken = await ethers.getContractFactory("MyToken");
    const token = await MyToken.deploy();
    const [owner, addr1] = await ethers.getSigners();
    
    await token.mint(addr1.address, ethers.parseEther("5000"));
    expect(await token.balanceOf(addr1.address)).to.equal(ethers.parseEther("5000"));
    
    // Non-owner should fail
    await expect(
      token.connect(addr1).mint(addr1.address, ethers.parseEther("1000"))
    ).to.be.revertedWithCustomError(token, "OwnableUnauthorizedAccount");
  });
});
```

**Run tests:**
```bash
npx hardhat test
```

**You should see:**
```
  MyToken
    ✔ Should deploy with correct name and symbol
    ✔ Should mint initial supply to owner
    ✔ Should transfer tokens
    ✔ Should fail if sender doesn't have enough tokens
    ✔ Should allow minting by owner only

  5 passing (2s)
```

---

### Deployment Script

```typescript
// scripts/deploy.ts
import { ethers } from "hardhat";

async function main() {
  console.log("Deploying MyToken...");
  
  const MyToken = await ethers.getContractFactory("MyToken");
  const token = await MyToken.deploy();
  
  await token.waitForDeployment();
  
  const address = await token.getAddress();
  console.log(`✅ MyToken deployed to: ${address}`);
  console.log(`   Name: ${await token.name()}`);
  console.log(`   Symbol: ${await token.symbol()}`);
  console.log(`   Total Supply: ${ethers.formatEther(await token.totalSupply())} MTK`);
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

**Deploy locally:**
```bash
npx hardhat run scripts/deploy.ts
```

---

## 🌐 Deploying to Testnets

### Getting Test ETH (Free!)

**Sepolia Testnet (recommended):**
1. Get Sepolia ETH: https://sepoliafaucet.com
2. Enter your wallet address
3. Get 0.5 SepoliaETH (fake, but works like real!)

**Other Testnets:**
- **Goerli:** https://goerlifaucet.com
- **Mumbai (Polygon):** https://faucet.polygon.technology
- **Arbitrum Sepolia:** https://bridge.arbitrum.io

---

### Configuring Hardhat for Testnet

```typescript
// hardhat.config.ts
import { HardhatUserConfig } from "hardhat/config";
import "@nomicfoundation/hardhat-toolbox";
import * as dotenv from "dotenv";

dotenv.config();

const config: HardhatUserConfig = {
  solidity: "0.8.20",
  networks: {
    sepolia: {
      url: `https://eth-sepolia.g.alchemy.com/v2/${process.env.ALCHEMY_API_KEY}`,
      accounts: [process.env.PRIVATE_KEY!]
    },
    arbitrumSepolia: {
      url: "https://sepolia-rollup.arbitrum.io/rpc",
      accounts: [process.env.PRIVATE_KEY!]
    },
    polygonMumbai: {
      url: "https://rpc-mumbai.maticvigil.com",
      accounts: [process.env.PRIVATE_KEY!]
    }
  },
  etherscan: {
    apiKey: process.env.ETHERSCAN_API_KEY
  }
};

export default config;
```

**Create `.env` file:**
```bash
# .env
ALCHEMY_API_KEY=your_alchemy_key_here
PRIVATE_KEY=your_wallet_private_key_here
ETHERSCAN_API_KEY=your_etherscan_key_here
```

**⚠️ NEVER commit .env to GitHub!**

Add to `.gitignore`:
```
.env
node_modules/
```

---

### Deploy to Sepolia

```bash
npx hardhat run scripts/deploy.ts --network sepolia
```

**You'll see:**
```
Deploying MyToken...
✅ MyToken deployed to: 0x123...abc
   Name: My Token
   Symbol: MTK
   Total Supply: 100000.0 MTK
```

**Verify on Etherscan:**
1. Go to https://sepolia.etherscan.io
2. Search your contract address
3. See your deployed contract! 🎉

---

## 🔐 Security Best Practices

### Common Vulnerabilities

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SecurityExamples {
    mapping(address => uint) public balances;
    
    // ❌ BAD: Reentrancy vulnerability
    function withdrawBad() public {
        uint amount = balances[msg.sender];
        (bool success,) = msg.sender.call{value: amount}("");  // Sends ETH first
        require(success, "Transfer failed");
        balances[msg.sender] = 0;  // Updates state AFTER (dangerous!)
    }
    
    // ✅ GOOD: Checks-Effects-Interactions pattern
    function withdrawGood() public {
        uint amount = balances[msg.sender];
        balances[msg.sender] = 0;  // Update state FIRST
        (bool success,) = msg.sender.call{value: amount}("");  // Send ETH LAST
        require(success, "Transfer failed");
    }
    
    // ❌ BAD: Integer overflow (old Solidity)
    function incrementBad_OldSolidity() public {
        uint8 x = 255;
        x = x + 1;  // Wraps to 0 (in Solidity <0.8.0)
    }
    
    // ✅ GOOD: Solidity 0.8+ has built-in overflow protection!
    function incrementGood() public pure returns (uint8) {
        uint8 x = 255;
        return x + 1;  // Reverts automatically!
    }
    
    // ❌ BAD: No access control
    function setOwnerBad(address newOwner) public {
        // Anyone can call this!
    }
    
    address public owner;
    
    constructor() {
        owner = msg.sender;
    }
    
    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }
    
    // ✅ GOOD: Only owner can call
    function setOwnerGood(address newOwner) public onlyOwner {
        owner = newOwner;
    }
}
```

---

### Using OpenZeppelin for Security

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/ReentrancyGuard.sol";
import "@openzeppelin/contracts/utils/Pausable.sol";

contract SecureToken is ERC20, Ownable, ReentrancyGuard, Pausable {
    constructor() 
        ERC20("Secure Token", "SEC")
        Ownable(msg.sender)
    {
        _mint(msg.sender, 1000000 * 10 ** decimals());
    }
    
    // ReentrancyGuard prevents reentrancy attacks
    function withdraw() public nonReentrant {
        // Safe withdrawal logic
    }
    
    // Pausable allows emergency stop
    function pause() public onlyOwner {
        _pause();
    }
    
    function unpause() public onlyOwner {
        _unpause();
    }
    
    // Override transfer to respect pause
    function _update(address from, address to, uint256 value) 
        internal 
        override 
        whenNotPaused 
    {
        super._update(from, to, value);
    }
}
```

---

## 🧪 Advanced Testing

### Testing with Hardhat

```typescript
// test/AdvancedTests.test.ts
import { expect } from "chai";
import { ethers } from "hardhat";
import { time, loadFixture } from "@nomicfoundation/hardhat-network-helpers";

describe("Advanced Token Tests", function () {
  // Fixture: Deploy contract once, reuse in tests
  async function deployTokenFixture() {
    const [owner, addr1, addr2] = await ethers.getSigners();
    const Token = await ethers.getContractFactory("MyToken");
    const token = await Token.deploy();
    return { token, owner, addr1, addr2 };
  }
  
  it("Should handle large transfers correctly", async function () {
    const { token, owner, addr1 } = await loadFixture(deployTokenFixture);
    const largeAmount = ethers.parseEther("50000");
    
    await token.transfer(addr1.address, largeAmount);
    expect(await token.balanceOf(addr1.address)).to.equal(largeAmount);
  });
  
  it("Should emit Transfer event", async function () {
    const { token, owner, addr1 } = await loadFixture(deployTokenFixture);
    const amount = ethers.parseEther("1000");
    
    await expect(token.transfer(addr1.address, amount))
      .to.emit(token, "Transfer")
      .withArgs(owner.address, addr1.address, amount);
  });
  
  it("Should respect max supply", async function () {
    const { token, owner, addr1 } = await loadFixture(deployTokenFixture);
    const maxSupply = await token.MAX_SUPPLY();
    const currentSupply = await token.totalSupply();
    const remaining = maxSupply - currentSupply;
    
    // Should succeed (within limit)
    await token.mint(addr1.address, remaining);
    
    // Should fail (exceeds limit)
    await expect(
      token.mint(addr1.address, ethers.parseEther("1"))
    ).to.be.revertedWith("Max supply exceeded");
  });
  
  it("Should handle time-based logic", async function () {
    const { token } = await loadFixture(deployTokenFixture);
    
    // Fast-forward time by 1 day
    await time.increase(86400);
    
    // Test time-dependent functions
  });
});
```

**Run with coverage:**
```bash
npx hardhat coverage
```

**You'll see:**
```
----------|----------|----------|----------|----------|
File      | % Stmts  | % Branch | % Funcs  | % Lines  |
----------|----------|----------|----------|----------|
MyToken   | 100      | 100      | 100      | 100      |
----------|----------|----------|----------|----------|
```

---

## 🎯 Practical Examples

### Example 1: Simple Voting Contract

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SimpleVoting {
    struct Proposal {
        string description;
        uint yesVotes;
        uint noVotes;
        bool executed;
        mapping(address => bool) hasVoted;
    }
    
    Proposal[] public proposals;
    address public owner;
    
    constructor() {
        owner = msg.sender;
    }
    
    function createProposal(string memory description) public {
        require(msg.sender == owner, "Only owner can create proposals");
        
        Proposal storage newProposal = proposals.push();
        newProposal.description = description;
    }
    
    function vote(uint proposalId, bool support) public {
        require(proposalId < proposals.length, "Invalid proposal");
        Proposal storage proposal = proposals[proposalId];
        require(!proposal.hasVoted[msg.sender], "Already voted");
        require(!proposal.executed, "Proposal already executed");
        
        proposal.hasVoted[msg.sender] = true;
        
        if (support) {
            proposal.yesVotes++;
        } else {
            proposal.noVotes++;
        }
    }
    
    function getProposalVotes(uint proposalId) public view returns (uint yes, uint no) {
        require(proposalId < proposals.length, "Invalid proposal");
        Proposal storage proposal = proposals[proposalId];
        return (proposal.yesVotes, proposal.noVotes);
    }
}
```

---

### Example 2: NFT Contract

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract SimpleNFT is ERC721, Ownable {
    uint256 private _tokenIdCounter;
    uint256 public constant MAX_SUPPLY = 10000;
    uint256 public constant PRICE = 0.01 ether;
    
    constructor() 
        ERC721("Simple NFT", "SNFT")
        Ownable(msg.sender)
    {}
    
    function mint() public payable {
        require(_tokenIdCounter < MAX_SUPPLY, "Max supply reached");
        require(msg.value >= PRICE, "Insufficient payment");
        
        uint256 tokenId = _tokenIdCounter;
        _tokenIdCounter++;
        _safeMint(msg.sender, tokenId);
    }
    
    function withdraw() public onlyOwner {
        uint256 balance = address(this).balance;
        payable(owner()).transfer(balance);
    }
    
    function totalSupply() public view returns (uint256) {
        return _tokenIdCounter;
    }
}
```

---

## 📝 Quick Reference

### Gas Optimization Tips

```solidity
// ✅ GOOD: Use uint256 (native EVM size)
uint256 public value;

// ❌ BAD: Smaller types cost MORE gas in some cases
uint8 public smallValue;  // Sometimes MORE expensive!

// ✅ GOOD: Pack storage variables
contract GoodPacking {
    uint128 a;  // Slot 0 (16 bytes)
    uint128 b;  // Slot 0 (16 bytes) - SAME SLOT!
    uint256 c;  // Slot 1
}

// ❌ BAD: Wasted storage slots
contract BadPacking {
    uint128 a;  // Slot 0
    uint256 b;  // Slot 1 (wastes half of slot 0)
    uint128 c;  // Slot 2
}

// ✅ GOOD: Use calldata for read-only arrays
function process(uint[] calldata data) external {
    // Cheaper than memory
}

// ✅ GOOD: Cache storage variables
function expensiveLoop() public {
    uint length = items.length;  // Cache in memory
    for (uint i = 0; i < length; i++) {
        // Use length instead of items.length
    }
}

// ✅ GOOD: Use events instead of storage for historical data
event DataLogged(uint indexed value, uint timestamp);

// ❌ BAD: Store everything on-chain
uint[] public allHistoricalData;  // EXPENSIVE!
```

---

## 🎓 Summary

**You now know:**
- ✅ Solidity syntax (types, functions, modifiers)
- ✅ Remix IDE (browser-based development)
- ✅ Hardhat (professional toolkit)
- ✅ ERC20 tokens (create your own crypto!)
- ✅ Testing (catch bugs before deployment)
- ✅ Security (prevent exploits)
- ✅ Deployment (testnets and mainnet)
- ✅ Whitepapers (Bitcoin and Ethereum fundamentals)

**Next Steps:**
1. Build the example contracts in Remix
2. Set up Hardhat and run tests
3. Deploy to Sepolia testnet
4. Read Bitcoin whitepaper (9 pages!)
5. Read Ethereum whitepaper
6. Build your own token project!

---

## 📚 Resources

**Whitepapers:**
- Bitcoin: https://bitcoin.org/bitcoin.pdf
- Ethereum: https://ethereum.org/en/whitepaper/

**Tools:**
- Remix IDE: https://remix.ethereum.org
- Hardhat: https://hardhat.org
- OpenZeppelin: https://www.openzeppelin.com/contracts

**Learning:**
- Solidity Docs: https://docs.soliditylang.org
- CryptoZombies: https://cryptozombies.io
- Solidity by Example: https://solidity-by-example.org

**Testnets:**
- Sepolia Faucet: https://sepoliafaucet.com
- Alchemy: https://www.alchemy.com
- Etherscan Sepolia: https://sepolia.etherscan.io

---

[← Chapter 03](Chapter-03-Ethereum-Smart-Contracts.md) | [Index](../Index.md) | [Next: Using Contracts →](Chapter-04-JavaScript-DApp-Development.md)

*Chapter 3.5/8 • Pure Solidity Power! 🔥*
