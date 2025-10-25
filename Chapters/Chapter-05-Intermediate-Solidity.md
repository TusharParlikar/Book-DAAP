# Chapter 05: Level Up Your Skills! 🛠️

**Hey!** Ready to go pro? Let's add real-time updates, batching, and React hooks! 🚀

[← Chapter 04](Chapter-04-JavaScript-DApp-Development.md) | [Index](../Index.md) | [Next: Full DApp →](Chapter-06-Full-Stack-DApps.md)

---

## 🎯 What's Happening Here?

**You can now:**
- ✅ Read smart contracts
- ✅ Send transactions
- ✅ Connect MetaMask

**Let's add superpowers:**
- 🔥 Real-time updates (events!)
- ⚡ 100x faster reads (multicall!)
- ⚛️ React hooks (Wagmi!)
- 🔨 Deploy your own contracts (Hardhat!)

**Get pumped!** You're about to become a PRO! 💪

---

## 📡 Events: Real-Time Updates (Like WebSockets!)

### Why Events Are Awesome

```javascript
// Events = Contract's way of saying "Something happened!"

const eventAnalogy = {
  realLife: [
    'Door sensor: "Door opened at 3:45pm"',
    'Motion detector: "Movement detected in room 5"',
    'Smoke alarm: "Smoke detected!"'
  ],
  
  blockchain: [
    'Token contract: "Transfer: Alice sent 100 USDC to Bob"',
    'NFT contract: "Mint: Token #1234 created for Alice"',
    'DeFi contract: "Swap: Alice swapped 1 ETH for 2000 USDC"'
  ],
  
  whyUseful: [
    'Track activity without reading every block',
    'Update UI in real-time',
    'Build activity feeds',
    'Analytics and monitoring'
  ]
};
```

### Example: Listen to USDC Transfers

```javascript
const { ethers } = require('ethers');

const USDC_ADDRESS = '0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48';

const USDC_ABI = [
  'event Transfer(address indexed from, address indexed to, uint256 value)'
];

async function listenToTransfers() {
  const provider = new ethers.JsonRpcProvider('https://eth.llamarpc.com');
  const usdc = new ethers.Contract(USDC_ADDRESS, USDC_ABI, provider);
  
  // Listen to ALL transfers
  usdc.on('Transfer', (from, to, value, event) => {
    const amount = ethers.formatUnits(value, 6);
    console.log(`
      Transfer detected!
      From: ${from}
      To: ${to}
      Amount: $${amount} USDC
      Tx: ${event.log.transactionHash}
    `);
  });
  
  console.log('Listening to USDC transfers...');
}

listenToTransfers();

// Output (real-time!):
// Transfer detected!
// From: 0x123...
// To: 0x456...
// Amount: $1000 USDC
// Tx: 0xabc...
```

### Filter Events (Only Watch Specific Addresses)

```javascript
async function watchVitalik() {
  const provider = new ethers.JsonRpcProvider('https://eth.llamarpc.com');
  const usdc = new ethers.Contract(USDC_ADDRESS, USDC_ABI, provider);
  
  const vitalik = '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045';
  
  // Filter: Only transfers TO or FROM Vitalik
  const filter = usdc.filters.Transfer(vitalik, null); // from Vitalik
  const filter2 = usdc.filters.Transfer(null, vitalik); // to Vitalik
  
  usdc.on(filter, (from, to, value) => {
    console.log(`Vitalik SENT ${ethers.formatUnits(value, 6)} USDC`);
  });
  
  usdc.on(filter2, (from, to, value) => {
    console.log(`Vitalik RECEIVED ${ethers.formatUnits(value, 6)} USDC`);
  });
  
  console.log('Watching Vitalik\'s USDC activity...');
}
```

### Get Historical Events

```javascript
async function getRecentTransfers() {
  const provider = new ethers.JsonRpcProvider('https://eth.llamarpc.com');
  const usdc = new ethers.Contract(USDC_ADDRESS, USDC_ABI, provider);
  
  // Get current block
  const currentBlock = await provider.getBlockNumber();
  
  // Get last 1000 blocks (~3.5 hours)
  const fromBlock = currentBlock - 1000;
  
  // Query events
  const filter = usdc.filters.Transfer();
  const events = await usdc.queryFilter(filter, fromBlock, currentBlock);
  
  console.log(`Found ${events.length} transfers in last 1000 blocks`);
  
  // Process events
  events.forEach((event) => {
    const { from, to, value } = event.args;
    const amount = ethers.formatUnits(value, 6);
    console.log(`${from} → ${to}: $${amount} USDC`);
  });
}
```

---

## 📦 Multicall (Batch Multiple Reads)

### The Problem

```javascript
// Slow way (many requests):
async function slowWay() {
  const usdc = new ethers.Contract(USDC_ADDRESS, USDC_ABI, provider);
  
  const addresses = [
    '0x123...',
    '0x456...',
    '0x789...',
    // ... 100 more addresses
  ];
  
  // This makes 103 network requests! (SLOW!)
  for (const addr of addresses) {
    const balance = await usdc.balanceOf(addr);
    console.log(balance);
  }
  
  // Takes: 10+ seconds
}

// Fast way (one request):
async function fastWay() {
  // Uses Multicall3 contract (deployed on all major chains)
  // Batches all calls into ONE request
  
  // Takes: <1 second
}
```

### Using Wagmi (React Hooks)

```javascript
import { useContractReads } from 'wagmi';

const USDC_ADDRESS = '0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48';

function TokenBalances() {
  const addresses = ['0x123...', '0x456...', '0x789...'];
  
  // Batch all reads into one call!
  const { data, isLoading } = useContractReads({
    contracts: addresses.map(address => ({
      address: USDC_ADDRESS,
      abi: USDC_ABI,
      functionName: 'balanceOf',
      args: [address]
    }))
  });
  
  if (isLoading) return <div>Loading...</div>;
  
  return (
    <div>
      {addresses.map((addr, i) => (
        <div key={addr}>
          {addr}: {data[i].result.toString()} USDC
        </div>
      ))}
    </div>
  );
}
```

---

## 🔨 Hardhat Setup (Deploy Contracts)

### Install Hardhat

```bash
npm init -y
npm install --save-dev hardhat @nomicfoundation/hardhat-toolbox
npx hardhat init
```

### Project Structure

```
my-project/
├── contracts/         # Solidity contracts here
├── scripts/          # Deploy scripts (JavaScript!)
├── test/             # Test files (JavaScript!)
├── hardhat.config.js # Configuration
└── package.json
```

### Deploy Script (JavaScript!)

```javascript
// scripts/deploy.js

const { ethers } = require('hardhat');

async function main() {
  console.log('Deploying contract...');
  
  // Get deployer account
  const [deployer] = await ethers.getSigners();
  console.log('Deploying with:', deployer.address);
  
  // Deploy contract
  const Token = await ethers.getContractFactory('MyToken');
  const token = await Token.deploy('My Token', 'MTK');
  
  await token.waitForDeployment();
  
  const address = await token.getAddress();
  console.log('Contract deployed to:', address);
  
  // Verify deployment
  const name = await token.name();
  const symbol = await token.symbol();
  console.log(`Token: ${name} (${symbol})`);
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

### Run Deploy Script

```bash
# Deploy to local network
npx hardhat run scripts/deploy.js

# Deploy to testnet (Sepolia)
npx hardhat run scripts/deploy.js --network sepolia

# Deploy to mainnet (CAREFUL!)
npx hardhat run scripts/deploy.js --network mainnet
```

### Configuration (hardhat.config.js)

```javascript
require('@nomicfoundation/hardhat-toolbox');
require('dotenv').config();

module.exports = {
  solidity: '0.8.20',
  
  networks: {
    // Local development
    hardhat: {
      chainId: 31337
    },
    
    // Ethereum Sepolia testnet
    sepolia: {
      url: process.env.SEPOLIA_RPC_URL,
      accounts: [process.env.PRIVATE_KEY],
      chainId: 11155111
    },
    
    // Arbitrum mainnet
    arbitrum: {
      url: 'https://arb1.arbitrum.io/rpc',
      accounts: [process.env.PRIVATE_KEY],
      chainId: 42161
    },
    
    // Polygon mainnet
    polygon: {
      url: 'https://polygon-rpc.com',
      accounts: [process.env.PRIVATE_KEY],
      chainId: 137
    }
  },
  
  etherscan: {
    apiKey: process.env.ETHERSCAN_API_KEY
  }
};
```

### Environment Variables (.env)

```bash
# .env file (NEVER commit to git!)

SEPOLIA_RPC_URL=https://eth-sepolia.g.alchemy.com/v2/YOUR_KEY
PRIVATE_KEY=your_private_key_here
ETHERSCAN_API_KEY=your_etherscan_api_key
```

---

## ⚛️ Wagmi (React Hooks for Web3)

### Install Wagmi

```bash
npm install wagmi viem @tanstack/react-query
```

### Setup (App.tsx)

```javascript
import { WagmiProvider, createConfig, http } from 'wagmi';
import { mainnet, arbitrum, polygon } from 'wagmi/chains';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ConnectKitProvider, getDefaultConfig } from 'connectkit';

// Configure chains
const config = createConfig(
  getDefaultConfig({
    appName: 'My DApp',
    chains: [mainnet, arbitrum, polygon],
    transports: {
      [mainnet.id]: http(),
      [arbitrum.id]: http(),
      [polygon.id]: http()
    },
    walletConnectProjectId: 'YOUR_PROJECT_ID'
  })
);

const queryClient = new QueryClient();

function App() {
  return (
    <WagmiProvider config={config}>
      <QueryClientProvider client={queryClient}>
        <ConnectKitProvider>
          <YourApp />
        </ConnectKitProvider>
      </QueryClientProvider>
    </WagmiProvider>
  );
}
```

### Connect Wallet (One Hook!)

```javascript
import { useAccount, useConnect, useDisconnect } from 'wagmi';

function WalletButton() {
  const { address, isConnected } = useAccount();
  const { connect, connectors } = useConnect();
  const { disconnect } = useDisconnect();
  
  if (isConnected) {
    return (
      <div>
        <p>Connected: {address}</p>
        <button onClick={() => disconnect()}>Disconnect</button>
      </div>
    );
  }
  
  return (
    <div>
      {connectors.map((connector) => (
        <button
          key={connector.id}
          onClick={() => connect({ connector })}
        >
          Connect {connector.name}
        </button>
      ))}
    </div>
  );
}
```

### Read Contract (useReadContract)

```javascript
import { useReadContract } from 'wagmi';

const USDC_ADDRESS = '0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48';
const USDC_ABI = [
  {
    name: 'balanceOf',
    type: 'function',
    stateMutability: 'view',
    inputs: [{ name: 'account', type: 'address' }],
    outputs: [{ name: 'balance', type: 'uint256' }]
  }
];

function USDCBalance({ address }) {
  const { data, isLoading, error } = useReadContract({
    address: USDC_ADDRESS,
    abi: USDC_ABI,
    functionName: 'balanceOf',
    args: [address]
  });
  
  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;
  
  return <div>Balance: {data?.toString()} USDC</div>;
}
```

### Write Contract (useWriteContract)

```javascript
import { useWriteContract, useWaitForTransactionReceipt } from 'wagmi';
import { parseUnits } from 'viem';

function TransferUSDC() {
  const { data: hash, writeContract } = useWriteContract();
  
  const { isLoading: isConfirming, isSuccess } = 
    useWaitForTransactionReceipt({ hash });
  
  async function transfer() {
    writeContract({
      address: USDC_ADDRESS,
      abi: USDC_ABI,
      functionName: 'transfer',
      args: [
        '0xRecipientAddress...',
        parseUnits('10', 6) // 10 USDC
      ]
    });
  }
  
  return (
    <div>
      <button onClick={transfer} disabled={isConfirming}>
        {isConfirming ? 'Transferring...' : 'Transfer 10 USDC'}
      </button>
      
      {hash && <div>Transaction: {hash}</div>}
      {isSuccess && <div>✅ Transfer successful!</div>}
    </div>
  );
}
```

---

## 🚀 Viem (Modern Alternative to Ethers.js)

### Why Viem?

```javascript
const comparison = {
  ethers: {
    pros: ['Most popular', 'Lots of tutorials', 'Well-tested'],
    cons: ['Larger bundle size', 'Slower', 'Less TypeScript-friendly']
  },
  
  viem: {
    pros: [
      'Faster (2-5x)',
      'Smaller bundle (~10KB vs ~80KB)',
      'Better TypeScript support',
      'Modern API',
      'Tree-shakeable'
    ],
    cons: ['Newer (less tutorials)', 'Breaking changes']
  },
  
  recommendation: 'Use Viem for new projects, Ethers for legacy'
};
```

### Viem Basic Usage

```javascript
import { createPublicClient, http, formatUnits } from 'viem';
import { mainnet } from 'viem/chains';

// Create client
const client = createPublicClient({
  chain: mainnet,
  transport: http()
});

// Get balance
const balance = await client.getBalance({
  address: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045'
});

console.log('Balance:', formatUnits(balance, 18), 'ETH');

// Read contract
const usdcBalance = await client.readContract({
  address: '0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48',
  abi: USDC_ABI,
  functionName: 'balanceOf',
  args: ['0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045']
});

console.log('USDC:', formatUnits(usdcBalance, 6));
```

---

## 📝 TL;DR (Quick Summary)

**Advanced patterns:**
```javascript
{
  events: 'contract.on("Transfer", callback) // Real-time listening',
  multicall: 'Batch reads → 100x faster',
  deployment: 'Hardhat + JavaScript deploy scripts',
  react: 'Wagmi hooks → useReadContract, useWriteContract',
  modern: 'Viem → Faster, smaller, better TypeScript'
}
```

**Typical workflow:**
1. Hardhat → Write + test contracts locally
2. Deploy to testnet (Sepolia)
3. Frontend with Wagmi/Viem
4. Listen to events for real-time updates
5. Deploy to mainnet (or Layer 2!)

**Best practices:**
- ✅ Test on testnet first (free!)
- ✅ Use environment variables for keys
- ✅ Never commit `.env` to git
- ✅ Estimate gas before transactions
- ✅ Handle errors gracefully

---

## ✅ Ready to Build!

**What you know now:**
- ✅ Listen to contract events in real-time
- ✅ Batch multiple calls (multicall)
- ✅ Deploy contracts with Hardhat
- ✅ Use Wagmi for React integration
- ✅ Viem as modern alternative

**Projects:**
- **Project 10:** Real-time Activity Feed (events)
- **Project 11:** Portfolio Dashboard (multicall)
- **Project 12:** Deploy Your First Contract

**Next:** Build complete full-stack DApp (putting it all together!)

---

## 🧠 Quick Quiz

1. **What are contract events for?**  
   → Track activity without reading every block

2. **Why use multicall?**  
   → Batch many reads into one request (100x faster!)

3. **What's Hardhat?**  
   → JavaScript tool for deploying/testing contracts

4. **What's Wagmi?**  
   → React hooks for Web3 (useReadContract, etc.)

5. **Viem vs Ethers?**  
   → Viem = faster, smaller, better TypeScript (use for new projects!)

---

[← Chapter 04](Chapter-04-JavaScript-DApp-Development.md) | [Index](../Index.md) | [Next: Full-Stack DApps →](Chapter-06-Full-Stack-DApps.md)

*Chapter 5/8 • Level Up Your Skills! 🛠️*
        emit OwnershipTransferred(owner, newOwner);
        owner = newOwner;
    }
}

// Child contract (Derived) - inherits from Ownable
contract MyContract is Ownable {
    uint256 public value;
    
    // This contract automatically has:
    // - owner variable
    // - onlyOwner modifier
    // - transferOwnership function
    
    function setValue(uint256 _value) public onlyOwner {
        // Can use onlyOwner modifier from parent contract
        value = _value;
    }
}

// Multiple Inheritance
contract Pausable {
    bool public paused;
    
    event Paused(address account);
    event Unpaused(address account);
    
    modifier whenNotPaused() {
        require(!paused, "Contract is paused");
        _;
    }
    
    modifier whenPaused() {
        require(paused, "Contract is not paused");
        _;
    }
    
    function pause() internal virtual {
        paused = true;
        emit Paused(msg.sender);
    }
    
    function unpause() internal virtual {
        paused = false;
        emit Unpaused(msg.sender);
    }
}

// Contract inheriting from multiple parents
contract AdvancedContract is Ownable, Pausable {
    uint256 public value;
    
    // Has features from both parent contracts:
    // - From Ownable: owner, onlyOwner, transferOwnership
    // - From Pausable: paused, whenNotPaused, pause, unpause
    
    function setValue(uint256 _value) public onlyOwner whenNotPaused {
        // Using modifiers from both parents
        value = _value;
    }
    
    function pauseContract() public onlyOwner {
        pause();  // Call parent function
    }
    
    function unpauseContract() public onlyOwner {
        unpause();  // Call parent function
    }
}

// Overriding Functions
contract CustomOwnable is Ownable {
    // Override parent function with custom logic
    function transferOwnership(address newOwner) public override onlyOwner {
        require(newOwner != address(0), "Invalid address");
        require(newOwner != owner, "Already owner");  // Additional check
        
        // Call parent implementation
        super.transferOwnership(newOwner);
    }
}
```

**Inheritance Patterns:**
```solidity
// Linear Inheritance
contract A { }
contract B is A { }
contract C is B { }  // C inherits from B and A

// Multiple Inheritance
contract A { }
contract B { }
contract C is A, B { }  // C inherits from both A and B

// Diamond Problem Resolution
contract A {
    function foo() public virtual returns (string memory) {
        return "A";
    }
}

contract B is A {
    function foo() public virtual override returns (string memory) {
        return "B";
    }
}

contract C is A {
    function foo() public virtual override returns (string memory) {
        return "C";
    }
}

contract D is B, C {
    // Must override because both B and C override foo()
    function foo() public override(B, C) returns (string memory) {
        return "D";
    }
}
```

### Interfaces

**Interface** = Contract blueprint that defines functions without implementation

```solidity
// Interface definition
interface IERC20 {
    // Events
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    
    // Functions (no implementation)
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

// Implementing an interface
contract MyToken is IERC20 {
    mapping(address => uint256) private balances;
    mapping(address => mapping(address => uint256)) private allowances;
    uint256 private _totalSupply;
    
    constructor(uint256 initialSupply) {
        _totalSupply = initialSupply;
        balances[msg.sender] = initialSupply;
    }
    
    // Must implement ALL interface functions
    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }
    
    function balanceOf(address account) external view override returns (uint256) {
        return balances[account];
    }
    
    function transfer(address to, uint256 amount) external override returns (bool) {
        require(balances[msg.sender] >= amount, "Insufficient balance");
        balances[msg.sender] -= amount;
        balances[to] += amount;
        emit Transfer(msg.sender, to, amount);
        return true;
    }
    
    function allowance(address owner, address spender) external view override returns (uint256) {
        return allowances[owner][spender];
    }
    
    function approve(address spender, uint256 amount) external override returns (bool) {
        allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }
    
    function transferFrom(address from, address to, uint256 amount) external override returns (bool) {
        require(balances[from] >= amount, "Insufficient balance");
        require(allowances[from][msg.sender] >= amount, "Insufficient allowance");
        
        balances[from] -= amount;
        balances[to] += amount;
        allowances[from][msg.sender] -= amount;
        
        emit Transfer(from, to, amount);
        return true;
    }
}

// Using interfaces to interact with other contracts
contract TokenUser {
    // Can interact with any contract that implements IERC20
    function checkBalance(address tokenAddress, address account) public view returns (uint256) {
        // Cast address to IERC20 interface
        IERC20 token = IERC20(tokenAddress);
        
        // Call interface function
        return token.balanceOf(account);
    }
    
    function transferTokens(
        address tokenAddress,
        address to,
        uint256 amount
    ) public {
        IERC20 token = IERC20(tokenAddress);
        
        // Call interface function
        bool success = token.transfer(to, amount);
        require(success, "Transfer failed");
    }
}
```

**Interface Rules:**
- Cannot have state variables
- Cannot have constructor
- All functions must be external
- Cannot inherit from contracts (only other interfaces)
- Used for defining standards (ERC20, ERC721, etc.)

---

## 💸 Sending and Receiving ETH

### Payable Functions

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract EtherWallet {
    address public owner;
    
    // Track deposits per user
    mapping(address => uint256) public deposits;
    
    event Deposit(address indexed from, uint256 amount);
    event Withdrawal(address indexed to, uint256 amount);
    
    constructor() {
        owner = msg.sender;
    }
    
    // Payable function - can receive ETH
    function deposit() public payable {
        require(msg.value > 0, "Must send ETH");
        
        // msg.value contains the amount of ETH sent (in wei)
        deposits[msg.sender] += msg.value;
        
        emit Deposit(msg.sender, msg.value);
    }
    
    // Payable fallback - receives ETH sent to contract address
    receive() external payable {
        deposits[msg.sender] += msg.value;
        emit Deposit(msg.sender, msg.value);
    }
    
    // Get contract ETH balance
    function getBalance() public view returns (uint256) {
        return address(this).balance;  // Contract's ETH balance in wei
    }
    
    // Get user's deposit
    function getDeposit(address user) public view returns (uint256) {
        return deposits[user];
    }
}
```

### Sending ETH - Three Methods

```solidity
contract SendEther {
    // Store ETH in contract
    receive() external payable {}
    
    // METHOD 1: transfer() - 2300 gas, throws error on failure
    function sendViaTransfer(address payable _to) public payable {
        // Sends ETH and reverts on failure
        _to.transfer(msg.value);
        
        // Pros: Simple, automatic revert on failure
        // Cons: Fixed 2300 gas (may fail with complex receive logic)
        // Status: Not recommended anymore
    }
    
    // METHOD 2: send() - 2300 gas, returns bool
    function sendViaSend(address payable _to) public payable {
        // Sends ETH and returns success/failure
        bool sent = _to.send(msg.value);
        require(sent, "Failed to send Ether");
        
        // Pros: Returns bool so you can handle failure
        // Cons: Fixed 2300 gas, need manual require
        // Status: Not recommended anymore
    }
    
    // METHOD 3: call() - forwards all gas, returns bool (RECOMMENDED)
    function sendViaCall(address payable _to) public payable {
        // Sends ETH with all remaining gas
        (bool sent, bytes memory data) = _to.call{value: msg.value}("");
        require(sent, "Failed to send Ether");
        
        // Pros: Forwards all gas, most flexible
        // Cons: Need to handle reentrancy (more on security)
        // Status: ✅ RECOMMENDED (as of 2025)
    }
    
    // Sending specific amount
    function sendAmount(address payable _to, uint256 _amount) public {
        require(address(this).balance >= _amount, "Insufficient balance");
        
        (bool sent, ) = _to.call{value: _amount}("");
        require(sent, "Failed to send Ether");
    }
    
    // Send with gas limit
    function sendWithGasLimit(address payable _to, uint256 _amount, uint256 _gasLimit) public {
        (bool sent, ) = _to.call{value: _amount, gas: _gasLimit}("");
        require(sent, "Failed to send Ether");
    }
}

// Receiving ETH
contract ReceiveEther {
    event Received(address sender, uint256 amount);
    
    // receive() - called when ETH sent with empty calldata
    receive() external payable {
        emit Received(msg.sender, msg.value);
    }
    
    // fallback() - called when function doesn't exist or ETH sent with data
    fallback() external payable {
        emit Received(msg.sender, msg.value);
    }
    
    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }
}
```

**Decision Tree for Sending ETH:**
```
Need to send ETH?
│
├─ Simple transfer, no complex logic in receiver?
│  └─ Use: call{value: amount}("")  ✅
│
├─ Receiver might have complex logic?
│  └─ Use: call{value: amount}("")  ✅
│     (But implement reentrancy guard!)
│
└─ Want gas limit control?
   └─ Use: call{value: amount, gas: limit}("")  ✅
```

---

## 🔗 Contract Interaction

### Calling Other Contracts

```solidity
// Target contract
contract Calculator {
    function add(uint256 a, uint256 b) public pure returns (uint256) {
        return a + b;
    }
    
    function multiply(uint256 a, uint256 b) public pure returns (uint256) {
        return a * b;
    }
}

// Calling contract - Method 1: Using interface
interface ICalculator {
    function add(uint256 a, uint256 b) external pure returns (uint256);
    function multiply(uint256 a, uint256 b) external pure returns (uint256);
}

contract CallerWithInterface {
    function useCalculator(address _calculatorAddress) public view returns (uint256) {
        // Cast address to interface
        ICalculator calc = ICalculator(_calculatorAddress);
        
        // Call functions
        uint256 sum = calc.add(5, 3);           // 8
        uint256 product = calc.multiply(4, 2);  // 8
        
        return sum + product;  // 16
    }
}

// Calling contract - Method 2: Direct instantiation
contract CallerDirect {
    Calculator public calc;
    
    constructor(address _calculatorAddress) {
        calc = Calculator(_calculatorAddress);
    }
    
    function performCalculations() public view returns (uint256, uint256) {
        uint256 sum = calc.add(10, 5);
        uint256 product = calc.multiply(3, 4);
        return (sum, product);
    }
}

// Calling contract - Method 3: Creating new contract
contract CallerWithCreation {
    Calculator public calc;
    
    function createCalculator() public {
        // Deploy new Calculator contract
        calc = new Calculator();
    }
    
    function useCalculator() public view returns (uint256) {
        require(address(calc) != address(0), "Calculator not created");
        return calc.add(7, 3);
    }
}
```

### Advanced Contract Interaction

```solidity
// Example: Interacting with ERC20 token
interface IERC20 {
    function transfer(address to, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

contract TokenManager {
    // Manage multiple tokens
    mapping(address => bool) public supportedTokens;
    
    function addSupportedToken(address _tokenAddress) public {
        supportedTokens[_tokenAddress] = true;
    }
    
    function checkTokenBalance(address _tokenAddress, address _account) 
        public 
        view 
        returns (uint256) 
    {
        require(supportedTokens[_tokenAddress], "Token not supported");
        
        IERC20 token = IERC20(_tokenAddress);
        return token.balanceOf(_account);
    }
    
    function transferTokens(
        address _tokenAddress,
        address _to,
        uint256 _amount
    ) public returns (bool) {
        require(supportedTokens[_tokenAddress], "Token not supported");
        
        IERC20 token = IERC20(_tokenAddress);
        
        // Transfer from caller to recipient
        return token.transferFrom(msg.sender, _to, _amount);
    }
    
    // Batch transfer to multiple recipients
    function batchTransfer(
        address _tokenAddress,
        address[] memory _recipients,
        uint256[] memory _amounts
    ) public {
        require(_recipients.length == _amounts.length, "Length mismatch");
        require(supportedTokens[_tokenAddress], "Token not supported");
        
        IERC20 token = IERC20(_tokenAddress);
        
        for (uint256 i = 0; i < _recipients.length; i++) {
            require(
                token.transferFrom(msg.sender, _recipients[i], _amounts[i]),
                "Transfer failed"
            );
        }
    }
}
```

---

## 🔨 Hardhat Development Framework

### What is Hardhat?

**Hardhat** = Professional Ethereum development environment

**Features:**
- ✅ Compile contracts
- ✅ Run tests
- ✅ Deploy to networks
- ✅ Debug transactions
- ✅ Local blockchain (Hardhat Network)
- ✅ Plugin ecosystem

### Installing Hardhat

**Prerequisites:**
- Node.js 18+ (Download from: https://nodejs.org)
- npm or yarn package manager

**Step-by-Step Setup:**

```bash
# Step 1: Create project directory
mkdir my-blockchain-project
cd my-blockchain-project

# Step 2: Initialize npm project
npm init -y
# Creates package.json

# Step 3: Install Hardhat
npm install --save-dev hardhat

# Step 4: Create Hardhat project
npx hardhat init
# Select: "Create a JavaScript project"
# Accept defaults (press Enter)

# Step 5: Install dependencies
npm install --save-dev @nomicfoundation/hardhat-toolbox
npm install --save-dev @openzeppelin/contracts

# Project created! 🎉
```

### Hardhat Project Structure

```
my-blockchain-project/
├── contracts/              # Solidity contracts
│   └── Lock.sol           # Example contract
├── scripts/               # Deployment scripts
│   └── deploy.js
├── test/                  # Test files
│   └── Lock.js
├── hardhat.config.js      # Hardhat configuration
├── package.json           # Dependencies
├── node_modules/          # Installed packages
└── README.md
```

### Hardhat Configuration

```javascript
// hardhat.config.js
require("@nomicfoundation/hardhat-toolbox");
require("@nomicfoundation/hardhat-verify");

/** @type import('hardhat/config').HardhatUserConfig */
module.exports = {
  solidity: {
    version: "0.8.20",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200
      }
    }
  },
  
  networks: {
    // Hardhat local network (default)
    hardhat: {
      chainId: 31337
    },
    
    // Sepolia testnet
    sepolia: {
      url: "https://ethereum-sepolia-rpc.publicnode.com",
      accounts: [process.env.PRIVATE_KEY], // Your wallet private key
      chainId: 11155111
    },
    
    // Ethereum mainnet (use with caution!)
    mainnet: {
      url: "https://ethereum-rpc.publicnode.com",
      accounts: [process.env.PRIVATE_KEY],
      chainId: 1
    }
  },
  
  etherscan: {
    apiKey: process.env.ETHERSCAN_API_KEY
  }
};
```

**Environment Variables (.env file):**
```bash
# .env (NEVER commit to git!)
PRIVATE_KEY=your_private_key_here_without_0x
ETHERSCAN_API_KEY=your_etherscan_api_key
```

**Important: Create .gitignore:**
```
node_modules/
.env
cache/
artifacts/
coverage/
```

---

## ✍️ Writing a Contract for Hardhat

```solidity
// contracts/SimpleToken.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SimpleToken {
    string public name = "Simple Token";
    string public symbol = "SIM";
    uint8 public decimals = 18;
    uint256 public totalSupply;
    
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    
    constructor(uint256 _initialSupply) {
        totalSupply = _initialSupply * 10 ** decimals;
        balanceOf[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
    }
    
    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(balanceOf[msg.sender] >= _value, "Insufficient balance");
        require(_to != address(0), "Invalid address");
        
        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;
        
        emit Transfer(msg.sender, _to, _value);
        return true;
    }
    
    function approve(address _spender, uint256 _value) public returns (bool success) {
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }
    
    function transferFrom(address _from, address _to, uint256 _value) 
        public 
        returns (bool success) 
    {
        require(_value <= balanceOf[_from], "Insufficient balance");
        require(_value <= allowance[_from][msg.sender], "Insufficient allowance");
        require(_to != address(0), "Invalid address");
        
        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        allowance[_from][msg.sender] -= _value;
        
        emit Transfer(_from, _to, _value);
        return true;
    }
}
```

### Compiling with Hardhat

```bash
# Compile contracts
npx hardhat compile

# Output:
# Compiled 1 Solidity file successfully (evm target: paris).
# Creates: artifacts/ and cache/ folders
```

---

## 🧪 Writing Tests

### Test Structure with Ethers.js

```javascript
// test/SimpleToken.js
const { expect } = require("chai");
const { ethers } = require("hardhat");

describe("SimpleToken Contract", function () {
  // Variables accessible in all tests
  let SimpleToken;
  let token;
  let owner;
  let addr1;
  let addr2;
  
  // Runs before each test
  beforeEach(async function () {
    // Get contract factory
    SimpleToken = await ethers.getContractFactory("SimpleToken");
    
    // Get signers (test accounts)
    [owner, addr1, addr2] = await ethers.getSigners();
    
    // Deploy contract with initial supply of 1000 tokens
    token = await SimpleToken.deploy(1000);
    await token.waitForDeployment();
  });
  
  // Test 1: Deployment
  describe("Deployment", function () {
    it("Should set the right owner balance", async function () {
      const ownerBalance = await token.balanceOf(owner.address);
      const totalSupply = await token.totalSupply();
      
      // Owner should have all tokens
      expect(ownerBalance).to.equal(totalSupply);
    });
    
    it("Should have correct name and symbol", async function () {
      expect(await token.name()).to.equal("Simple Token");
      expect(await token.symbol()).to.equal("SIM");
    });
  });
  
  // Test 2: Transfers
  describe("Transactions", function () {
    it("Should transfer tokens between accounts", async function () {
      const amount = ethers.parseUnits("50", 18);  // 50 tokens
      
      // Transfer 50 tokens from owner to addr1
      await token.transfer(addr1.address, amount);
      
      // Check balances
      const addr1Balance = await token.balanceOf(addr1.address);
      expect(addr1Balance).to.equal(amount);
      
      // Transfer 25 tokens from addr1 to addr2
      const transferAmount = ethers.parseUnits("25", 18);
      await token.connect(addr1).transfer(addr2.address, transferAmount);
      
      const addr2Balance = await token.balanceOf(addr2.address);
      expect(addr2Balance).to.equal(transferAmount);
    });
    
    it("Should fail if sender doesn't have enough tokens", async function () {
      const initialOwnerBalance = await token.balanceOf(owner.address);
      const tooMuch = initialOwnerBalance + BigInt(1);
      
      // Try to send more than balance (should fail)
      await expect(
        token.connect(addr1).transfer(owner.address, tooMuch)
      ).to.be.revertedWith("Insufficient balance");
    });
    
    it("Should emit Transfer event", async function () {
      const amount = ethers.parseUnits("100", 18);
      
      await expect(token.transfer(addr1.address, amount))
        .to.emit(token, "Transfer")
        .withArgs(owner.address, addr1.address, amount);
    });
  });
  
  // Test 3: Allowances
  describe("Allowances", function () {
    it("Should approve and transferFrom", async function () {
      const amount = ethers.parseUnits("100", 18);
      
      // Owner approves addr1 to spend 100 tokens
      await token.approve(addr1.address, amount);
      
      // Check allowance
      const allowance = await token.allowance(owner.address, addr1.address);
      expect(allowance).to.equal(amount);
      
      // addr1 transfers 50 tokens from owner to addr2
      const transferAmount = ethers.parseUnits("50", 18);
      await token.connect(addr1).transferFrom(
        owner.address,
        addr2.address,
        transferAmount
      );
      
      // Check balances and allowance
      const addr2Balance = await token.balanceOf(addr2.address);
      expect(addr2Balance).to.equal(transferAmount);
      
      const remainingAllowance = await token.allowance(owner.address, addr1.address);
      expect(remainingAllowance).to.equal(amount - transferAmount);
    });
    
    it("Should fail if allowance is exceeded", async function () {
      const approvedAmount = ethers.parseUnits("100", 18);
      const tooMuch = ethers.parseUnits("150", 18);
      
      await token.approve(addr1.address, approvedAmount);
      
      await expect(
        token.connect(addr1).transferFrom(owner.address, addr2.address, tooMuch)
      ).to.be.revertedWith("Insufficient allowance");
    });
  });
});
```

### Running Tests

```bash
# Run all tests
npx hardhat test

# Run specific test file
npx hardhat test test/SimpleToken.js

# Run with gas reporting
REPORT_GAS=true npx hardhat test

# Run with coverage
npx hardhat coverage
```

**Example Output:**
```
SimpleToken Contract
  Deployment
    ✔ Should set the right owner balance (523ms)
    ✔ Should have correct name and symbol (41ms)
  Transactions
    ✔ Should transfer tokens between accounts (152ms)
    ✔ Should fail if sender doesn't have enough tokens (98ms)
    ✔ Should emit Transfer event (87ms)
  Allowances
    ✔ Should approve and transferFrom (201ms)
    ✔ Should fail if allowance is exceeded (112ms)

7 passing (1.2s)
```

---

## 🚀 Deployment Scripts

### Local Deployment

```javascript
// scripts/deploy.js
const hre = require("hardhat");

async function main() {
  console.log("Deploying SimpleToken...");
  
  // Get contract factory
  const SimpleToken = await hre.ethers.getContractFactory("SimpleToken");
  
  // Deploy with constructor argument (initial supply)
  const initialSupply = 1000000;  // 1 million tokens
  const token = await SimpleToken.deploy(initialSupply);
  
  // Wait for deployment to complete
  await token.waitForDeployment();
  
  const address = await token.getAddress();
  console.log("SimpleToken deployed to:", address);
  
  // Verify deployment
  console.log("Token name:", await token.name());
  console.log("Token symbol:", await token.symbol());
  console.log("Total supply:", await token.totalSupply());
  
  return token;
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

**Run Local Deployment:**
```bash
# Start local Hardhat node (in separate terminal)
npx hardhat node
# Provides 20 test accounts with 10,000 ETH each

# Deploy to local network
npx hardhat run scripts/deploy.js --network localhost
```

### Testnet Deployment

```bash
# Deploy to Sepolia testnet
npx hardhat run scripts/deploy.js --network sepolia

# Verify contract on Etherscan
npx hardhat verify --network sepolia DEPLOYED_CONTRACT_ADDRESS "1000000"
```

---

## 📚 Using OpenZeppelin Libraries

### What is OpenZeppelin?

**OpenZeppelin** = Battle-tested library of secure smart contract components

**Benefits:**
- ✅ Audited and secure
- ✅ Gas optimized
- ✅ Industry standard
- ✅ Well-documented

### Installing OpenZeppelin

```bash
npm install @openzeppelin/contracts
```

### Using OpenZeppelin ERC20

```solidity
// contracts/MyToken.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

// Inherit from OpenZeppelin contracts
contract MyToken is ERC20, Ownable {
    // Constructor - calls parent constructors
    constructor(uint256 initialSupply) 
        ERC20("My Token", "MTK")           // Set name and symbol
        Ownable(msg.sender)                // Set owner
    {
        // Mint initial supply to deployer
        _mint(msg.sender, initialSupply * 10 ** decimals());
    }
    
    // Only owner can mint new tokens
    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
    }
    
    // Only owner can burn tokens
    function burn(uint256 amount) public onlyOwner {
        _burn(msg.sender, amount);
    }
}

// That's it! You have a fully functional ERC20 token with:
// - transfer()
// - approve()
// - transferFrom()
// - balanceOf()
// - And all ERC20 standard functions
// Without writing hundreds of lines of code!
```

### Using OpenZeppelin ERC721 (NFT)

```solidity
// contracts/MyNFT.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/Counters.sol";

contract MyNFT is ERC721, ERC721URIStorage, Ownable {
    using Counters for Counters.Counter;
    Counters.Counter private _tokenIds;
    
    constructor() 
        ERC721("My NFT", "MNFT")
        Ownable(msg.sender)
    {}
    
    // Mint new NFT
    function mintNFT(address recipient, string memory tokenURI) 
        public 
        onlyOwner 
        returns (uint256) 
    {
        _tokenIds.increment();
        uint256 newTokenId = _tokenIds.current();
        
        // Mint token to recipient
        _mint(recipient, newTokenId);
        
        // Set metadata URI (points to JSON file with image, properties, etc.)
        _setTokenURI(newTokenId, tokenURI);
        
        return newTokenId;
    }
    
    // Required overrides
    function tokenURI(uint256 tokenId)
        public
        view
        override(ERC721, ERC721URIStorage)
        returns (string memory)
    {
        return super.tokenURI(tokenId);
    }
    
    function supportsInterface(bytes4 interfaceId)
        public
        view
        override(ERC721, ERC721URIStorage)
        returns (bool)
    {
        return super.supportsInterface(interfaceId);
    }
}
```

---

## ⛽ Gas Optimization Basics

### Gas-Expensive Operations

```solidity
contract GasOptimization {
    // ❌ EXPENSIVE: Storage writes
    uint256 public counter;
    
    function incrementBad() public {
        counter = counter + 1;  // ~5000 gas for storage write
    }
    
    // ❌ EXPENSIVE: Reading from storage multiple times
    function sumBad() public view returns (uint256) {
        return counter + counter + counter;  // 3 storage reads!
    }
    
    // ✅ BETTER: Cache storage in memory
    function sumGood() public view returns (uint256) {
        uint256 temp = counter;  // 1 storage read
        return temp + temp + temp;  // Memory operations are cheap
    }
    
    // ❌ EXPENSIVE: Large arrays in storage
    uint256[] public largeArray;
    
    function addToArrayBad(uint256 value) public {
        largeArray.push(value);  // Expensive!
    }
    
    // ❌ EXPENSIVE: String storage
    string public longString;
    
    function setStringBad(string memory str) public {
        longString = str;  // Very expensive!
    }
    
    // ✅ BETTER: Use bytes32 for short strings
    bytes32 public shortString;
    
    function setStringGood(bytes32 str) public {
        shortString = str;  // Much cheaper!
    }
    
    // ❌ EXPENSIVE: Loops over storage arrays
    function sumArrayBad() public view returns (uint256) {
        uint256 sum = 0;
        for (uint256 i = 0; i < largeArray.length; i++) {
            sum += largeArray[i];  // Multiple storage reads
        }
        return sum;
    }
    
    // ❌ EXPENSIVE: Using > 0 for uint
    function checkBad(uint256 value) public pure returns (bool) {
        return value > 0;  // More expensive
    }
    
    // ✅ BETTER: Using != 0 for uint
    function checkGood(uint256 value) public pure returns (bool) {
        return value != 0;  // Cheaper!
    }
}
```

### Gas Optimization Techniques

```solidity
contract GasOptimized {
    // 1. Use uint256 instead of smaller uints (unless in structs/arrays)
    uint256 public value1;  // ✅ Cheaper in most cases
    uint8 public value2;    // ❌ More expensive (except in struct/array packing)
    
    // 2. Pack variables in storage
    struct Packed {
        uint128 a;  // 16 bytes
        uint128 b;  // 16 bytes
        // Total: 32 bytes = 1 storage slot ✅
    }
    
    struct NotPacked {
        uint256 a;  // 32 bytes = 1 slot
        uint256 b;  // 32 bytes = 1 slot
        // Total: 2 storage slots ❌
    }
    
    // 3. Use immutable and constant
    address public immutable owner;  // Set once in constructor, cheaper to read
    uint256 public constant MAX = 100;  // Hardcoded, no storage, very cheap
    
    constructor() {
        owner = msg.sender;
    }
    
    // 4. Use calldata for function parameters (read-only)
    function processBad(uint256[] memory data) public pure returns (uint256) {
        // "memory" copies data, costs gas
        return data[0];
    }
    
    function processGood(uint256[] calldata data) public pure returns (uint256) {
        // "calldata" reads directly, no copy, cheaper ✅
        return data[0];
    }
    
    // 5. Use events instead of storing non-critical data
    event DataLogged(uint256 indexed id, string data);
    
    function logData(uint256 id, string memory data) public {
        emit DataLogged(id, data);  // Much cheaper than storing!
    }
    
    // 6. Batch operations
    function transferMultiple(address[] calldata recipients, uint256[] calldata amounts) public {
        require(recipients.length == amounts.length);
        
        for (uint256 i = 0; i < recipients.length; i++) {
            // Process multiple transfers in one transaction
            // Saves base transaction gas cost
        }
    }
}
```

---

## 🔒 Security Basics

### Reentrancy Attack Prevention

```solidity
// ❌ VULNERABLE to reentrancy
contract VulnerableBank {
    mapping(address => uint256) public balances;
    
    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }
    
    function withdraw() public {
        uint256 balance = balances[msg.sender];
        
        // ❌ DANGER: External call before state update
        (bool sent, ) = msg.sender.call{value: balance}("");
        require(sent);
        
        // Attacker can re-enter here before balance is set to 0!
        balances[msg.sender] = 0;
    }
}

// ✅ PROTECTED with Checks-Effects-Interactions pattern
contract SecureBank {
    mapping(address => uint256) public balances;
    
    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }
    
    function withdraw() public {
        uint256 balance = balances[msg.sender];
        require(balance > 0, "No balance");
        
        // 1. CHECKS: Validate conditions
        // 2. EFFECTS: Update state FIRST
        balances[msg.sender] = 0;
        
        // 3. INTERACTIONS: External calls LAST
        (bool sent, ) = msg.sender.call{value: balance}("");
        require(sent, "Failed to send");
    }
}

// ✅ PROTECTED with ReentrancyGuard (OpenZeppelin)
import "@openzeppelin/contracts/utils/ReentrancyGuard.sol";

contract SafeBank is ReentrancyGuard {
    mapping(address => uint256) public balances;
    
    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }
    
    function withdraw() public nonReentrant {  // ✅ Prevents reentrancy
        uint256 balance = balances[msg.sender];
        require(balance > 0);
        
        balances[msg.sender] = 0;
        (bool sent, ) = msg.sender.call{value: balance}("");
        require(sent);
    }
}
```

### Integer Overflow/Underflow

```solidity
// Before Solidity 0.8.0:
// ❌ Vulnerable to overflow/underflow
contract Vulnerable {
    uint8 public count = 255;
    
    function increment() public {
        count++;  // Overflows to 0 (in Solidity < 0.8.0)
    }
}

// Solidity 0.8.0+:
// ✅ Automatic overflow/underflow protection
contract Safe {
    uint8 public count = 255;
    
    function increment() public {
        count++;  // Reverts with error (in Solidity >= 0.8.0) ✅
    }
    
    // If you NEED wrapping behavior:
    function incrementUnchecked() public {
        unchecked {
            count++;  // Wraps to 0 (explicit)
        }
    }
}
```

---

## 📝 Chapter Summary

**Key Takeaways:**

1. **Inheritance** allows contracts to reuse code from parent contracts
2. **Interfaces** define function signatures for interacting with other contracts
3. **Send ETH** using `call{value: amount}("")` (recommended method as of 2025)
4. **Hardhat** is the professional development framework with testing, deployment, and debugging
5. **Ethers.js** is used for testing and interacting with contracts
6. **OpenZeppelin** provides secure, audited contract libraries (ERC20, ERC721, etc.)
7. **Gas optimization** focuses on minimizing storage operations and using efficient patterns
8. **Security** requires checking for reentrancy, following Checks-Effects-Interactions, and using latest Solidity

---

## 🔗 Reference Links

### Official Documentation
- **Hardhat Docs:** https://hardhat.org/docs
- **Ethers.js Docs:** https://docs.ethers.org
- **OpenZeppelin Docs:** https://docs.openzeppelin.com/contracts

### GitHub Repositories
- **Hardhat:** https://github.com/NomicFoundation/hardhat
- **OpenZeppelin Contracts:** https://github.com/OpenZeppelin/openzeppelin-contracts
- **Ethers.js:** https://github.com/ethers-io/ethers.js

### Tools & Services
- **Alchemy (RPC Provider):** https://www.alchemy.com
- **Infura (RPC Provider):** https://www.infura.io
- **Etherscan API:** https://docs.etherscan.io
- **Sepolia Faucet:** https://sepoliafaucet.com

### Security Resources
- **Smart Contract Security Best Practices:** https://consensys.github.io/smart-contract-best-practices/
- **SWC Registry (Vulnerability List):** https://swcregistry.io
- **OpenZeppelin Security:** https://www.openzeppelin.com/security-audits

### Learning Resources
- **Hardhat Tutorial:** https://hardhat.org/tutorial
- **OpenZeppelin Learn:** https://docs.openzeppelin.com/learn/
- **Ethers.js Examples:** https://github.com/ethers-io/ethers.js/tree/main/packages/contracts

---

[← Previous](Chapter-04-JavaScript-DApp-Development.md) | [Index](../Index.md) | [Next Chapter →](Chapter-06-Full-Stack-DApps.md)

---

*Last Updated: October 25, 2025*
*Blockchain Learning Series for Beginners*
