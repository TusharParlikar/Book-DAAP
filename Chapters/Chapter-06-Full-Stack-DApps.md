# Chapter 06: Building Full-Stack DApps 🖥️🔗

**Subtitle:** Creating User Interfaces for Your Contracts

[← Previous](Chapter-05-Intermediate-Solidity.md) | [Index](../Index.md) | [Next Chapter →](Chapter-07-Advanced-DeFi-DePIN.md)

---

## 📚 Learning Objectives

By the end of this chapter, you will:
- Understand what a DApp (Decentralized Application) is
- Learn the basics of React and Next.js (don't worry if you're new!)
- Connect wallets to websites using simple libraries
- Read data from smart contracts and display it on a webpage
- Send transactions from a website
- Build your first complete blockchain application

---

## 🤔 What is a DApp?

### Understanding DApps for Complete Beginners

**Traditional App:**
```
User → Website → Company's Server → Database
                 (They control everything)
```

**Decentralized App (DApp):**
```
User → Website → Blockchain (No company controls it)
                 (Code runs on Ethereum, not a company server)
```

**Key Differences:**

| Traditional App | DApp |
|----------------|------|
| Facebook controls your data | You control your data |
| PayPal can freeze your account | Nobody can freeze your wallet |
| Netflix decides what you can watch | Smart contracts enforce rules automatically |
| Need email/password | Connect with your crypto wallet |

**Real World Example:**
- **Traditional:** Instagram stores your photos on their servers
- **DApp:** NFT platform stores proof of ownership on blockchain, you truly own your images

---

## 🧩 Technology Stack Explained (For Beginners)

Before we start coding, let's understand what each tool does:

### 1️⃣ **React** - The Building Block Language
**What is it?** A JavaScript library for building user interfaces (the visual part users see)

**Think of it like LEGO:**
- Each piece (component) is reusable
- You combine pieces to build something bigger
- Example: A "Connect Wallet Button" is one LEGO piece you can use anywhere

**Why React?**
- Makes building complex UIs simple
- Most popular choice for DApps (lots of help online)
- Components can be reused across your app

### 2️⃣ **Next.js** - React's Powerful Friend
**What is it?** A framework built on top of React that adds extra features

**Think of it like:**
- React = Bicycle 🚲
- Next.js = Bicycle with motor, lights, GPS, and training wheels 🏍️

**What Next.js adds:**
- **Routing:** Go from page to page easily (like clicking links)
- **Performance:** Your website loads super fast
- **SEO:** Google can find your site
- **Built-in API routes:** Can have backend code too

### 3️⃣ **TypeScript** - JavaScript with Safety
**What is it?** JavaScript but with type checking (catches errors before you run code)

**Example:**
```javascript
// JavaScript - No error until you run it
let age = "twenty"; 
age = age + 5; // Bug! "twenty5" 😱

// TypeScript - Error immediately while coding
let age: number = "twenty"; // ❌ TypeScript says: "Hey! Age should be a number!"
```

**Why Use It?**
- Catches mistakes as you type
- Makes code easier to understand
- IDEs give you better autocomplete suggestions

### 4️⃣ **RainbowKit** - Wallet Connection Made Easy
**What is it?** A library that adds a "Connect Wallet" button to your site

**Without RainbowKit:** 😰
- Write 200+ lines of code
- Handle MetaMask, WalletConnect, Coinbase Wallet separately
- Design the UI yourself
- Handle edge cases

**With RainbowKit:** 😎
```jsx
<ConnectButton />  // That's it! One line gives you everything
```

### 5️⃣ **Wagmi** - Talk to Blockchain Easily
**What is it?** React hooks (simple functions) to interact with Ethereum

**Think of it as:**
- **You:** "Hey Wagmi, what's the balance of this address?"
- **Wagmi:** "Here you go! 5.2 ETH"

**Common Wagmi Functions:**
- `useAccount()` - "Who is the connected user?"
- `useReadContract()` - "Read data from smart contract"
- `useWriteContract()` - "Send a transaction to blockchain"

### 6️⃣ **Viem** - Blockchain Data Helper
**What is it?** A library to format blockchain data for humans

**Example:**
```typescript
// Raw blockchain data: "5000000000000000000" 😵
// Viem formats it: "5.0 ETH" 😊

import { formatEther } from 'viem';
const readable = formatEther("5000000000000000000"); // "5.0"
```

---

## 🎯 Chapter Roadmap - What We'll Build

We'll build a **Token Balance Checker** app step by step:

**Step 1:** Create empty Next.js project ✅
**Step 2:** Add "Connect Wallet" button ✅
**Step 3:** Show user's wallet address ✅
**Step 4:** Read token balance from blockchain ✅
**Step 5:** Send tokens to another address ✅
**Step 6:** Make it look pretty with CSS ✅

**What You'll Learn:**
- How to set up a project from scratch
- How wallets connect to websites
- How to read blockchain data
- How to send transactions
- How to handle loading states and errors

---

## 🚀 Step 1: Setting Up Your Project

### Next.js 14+ with App Router (2025 Standard)

**Why Next.js?**
- ✅ React framework with built-in routing
- ✅ Server-side rendering (SSR) and static generation
- ✅ API routes for backend logic
- ✅ Excellent performance and SEO
- ✅ Industry standard for Web3 apps

**Creating a Next.js Project:**

```bash
# Step 1: Create Next.js app
# Open your terminal and type this:
npx create-next-app@latest my-dapp

# You'll see several questions. Answer them like this:
# ✅ TypeScript? → Press Enter (Yes)
#    Why: Catches errors for us automatically
# 
# ✅ ESLint? → Press Enter (Yes)
#    Why: Tells us when our code has problems
#
# ✅ Tailwind CSS? → Press Enter (Yes)
#    Why: Makes styling super easy
#
# ✅ App Router? → Press Enter (Yes)
#    Why: This is the new, better way to do routing
#
# ✅ Import alias? → Press Enter (Yes)
#    Why: Makes importing files cleaner

# Step 2: Go into the project folder
cd my-dapp

# Step 3: Install Web3 libraries (these let us talk to blockchain)
npm install wagmi@2 viem@2 @rainbow-me/rainbowkit@2 @tanstack/react-query

# What did we just install?
# - wagmi: Helps us interact with Ethereum
# - viem: Formats blockchain data
# - rainbowkit: Gives us the "Connect Wallet" button
# - react-query: Manages data fetching

# Step 4: Start the development server
npm run dev

# You should see: ✔ Ready in 2s
# Open your browser and go to: http://localhost:3000
# You'll see a basic Next.js welcome page! 🎉
```

**⏸️ PAUSE HERE:** Did it work? You should see a webpage. If you see errors, common fixes:
- Error "npm not found"? → Install Node.js first
- Error "port 3000 in use"? → Close other apps or use `npm run dev -- -p 3001`

---

## 🌈 Step 2: Adding Wallet Connection

### What We're Building

We're going to add a button that says "Connect Wallet". When clicked:
1. MetaMask popup appears
2. User approves connection
3. Website can now see user's wallet address
4. Button shows "0x1234..." (their address)

### Getting Your WalletConnect Project ID

**Before we code, we need a FREE API key:**

1. Go to https://cloud.walletconnect.com/
2. Click "Sign Up" (it's free!)
3. Create a new project
4. Copy the "Project ID" (looks like: `a1b2c3d4...`)
5. Keep this handy!

**Why do we need this?** WalletConnect helps mobile wallets connect to your site.

---

### Setting Up RainbowKit

Let's add wallet connection to our project step by step:

**Step 1: Create the Providers File**

This file sets up all the libraries we need. Think of it as turning on all the switches.

```typescript
// Create a new file: app/providers.tsx
// This is like a "wrapper" that gives our app superpowers

'use client';  // This tells Next.js: "This runs in the browser"

// Import the libraries we installed
import '@rainbow-me/rainbowkit/styles.css';  // Makes the wallet button look nice
import {
  getDefaultConfig,
  RainbowKitProvider,
} from '@rainbow-me/rainbowkit';
import { WagmiProvider } from 'wagmi';
import {
  mainnet,  // Ethereum mainnet (real money!)
  sepolia,  // Sepolia testnet (fake money for testing)
} from 'wagmi/chains';
import {
  QueryClientProvider,
  QueryClient,
} from '@tanstack/react-query';

// Configure which blockchains we want to support
const config = getDefaultConfig({
  appName: 'My First DApp',  // Your app's name
  projectId: 'YOUR_PROJECT_ID',  // 🔴 REPLACE with your WalletConnect ID
  chains: [
    mainnet,  // Real Ethereum
    sepolia   // Test Ethereum (we'll use this!)
  ],
  ssr: true,  // Needed for Next.js
});

// Create a query client (manages data fetching)
const queryClient = new QueryClient();

// This component wraps our entire app
export function Providers({ children }: { children: React.ReactNode }) {
  return (
    // Layer 1: Wagmi provides blockchain connection
    <WagmiProvider config={config}>
      {/* Layer 2: React Query manages data */}
      <QueryClientProvider client={queryClient}>
        {/* Layer 3: RainbowKit provides the UI */}
        <RainbowKitProvider>
          {children}  {/* Your app goes here */}
        </RainbowKitProvider>
      </QueryClientProvider>
    </WagmiProvider>
  );
}
```

**🎓 What did we just do?**
- Created a "wrapper" that gives our app blockchain powers
- Configured which networks to support (Ethereum mainnet and Sepolia testnet)
- Set up RainbowKit to show the wallet connection UI

---

**Step 2: Wrap Your App**

Now we tell Next.js to use our Providers:

```typescript
// Edit the file: app/layout.tsx
// This file wraps EVERY page in your app

import { Providers } from './providers';  // Import what we just made
import './globals.css';

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>
        {/* Everything inside Providers can now use blockchain features! */}
        <Providers>{children}</Providers>
      </body>
    </html>
  );
}
```

**🎓 What did we just do?**
- Wrapped our entire app with the Providers
- Now EVERY page can connect to wallets!

---

**Step 3: Add the Connect Button**

Let's create a simple navbar with the connect button:

```typescript
// Create a new file: components/ConnectButton.tsx
// This is our first reusable component!
```typescript
// Create a new file: components/ConnectButton.tsx
// This is our first reusable component!

import { ConnectButton } from '@rainbow-me/rainbowkit';

export default function ConnectWallet() {
  return (
    <div className="flex justify-end p-4">
      {/* This ONE line gives us a fully functional wallet button! */}
      <ConnectButton />
    </div>
  );
}

// 🎉 RainbowKit gives us ALL of this automatically:
// ✅ Beautiful wallet selection modal (MetaMask, WalletConnect, Coinbase, etc.)
// ✅ Chain switching (switch between Ethereum and Sepolia)
// ✅ Account display (shows your address)
// ✅ Disconnect button
// ✅ ENS name resolution (if you have name.eth, it shows that!)
// ✅ Mobile responsive design
// All with ONE component! 🤯
```

**Step 4: Use the Button in Your App**

Let's add it to the homepage:

```typescript
// Edit: app/page.tsx
// This is your homepage

'use client';  // Runs in browser

import ConnectWallet from '@/components/ConnectButton';  // Import our button

export default function Home() {
  return (
    <main className="min-h-screen bg-gray-50">
      {/* Header with our connect button */}
      <header className="bg-white shadow">
        <div className="max-w-7xl mx-auto px-4 py-4 flex justify-between items-center">
          <h1 className="text-3xl font-bold">My First DApp 🚀</h1>
          <ConnectWallet />  {/* Our button appears here! */}
        </div>
      </header>

      {/* Main content area */}
      <div className="max-w-7xl mx-auto px-4 py-8">
        <h2 className="text-2xl font-bold mb-4">
          Welcome! Connect your wallet to get started.
        </h2>
      </div>
    </main>
  );
}
```

---

### 🧪 Test Time!

1. **Save all files**
2. **Go to http://localhost:3000**
3. **You should see:**
   - A header with "My First DApp 🚀"
   - A "Connect Wallet" button in the top right

4. **Click "Connect Wallet"**
   - A popup appears with wallet options
   - Click "MetaMask"
   - MetaMask asks for permission → Click "Connect"
   - Button now shows your address! (like "0x1234...")

**🎉 Congratulations!** You just connected a wallet to your website!

---

## 📖 Step 3: Reading From the Blockchain

### What We're Building Now

Let's display the user's ETH balance. Here's what happens:
1. User connects wallet
2. We ask the blockchain: "How much ETH does this address have?"
3. Blockchain responds with the balance
4. We display it on the page

### Understanding Smart Contract Interaction

First, let's understand what we need to talk to a smart contract:

**You need 3 things:**
1. **Contract Address** - Where the contract lives (like a house address)
   - Example: `0x1234...abcd`
2. **ABI** - The contract's "menu" (what functions it has)
   - Example: ["balanceOf", "transfer", "totalSupply"]
3. **Function Name** - What you want to do
   - Example: "balanceOf" (check balance)

**Real World Analogy:**
- **Restaurant** = Smart Contract
- **Restaurant Address** = Contract Address
- **Menu** = ABI
- **Ordering food** = Calling a function

---

### Setting Up Contract Information

Let's create a file to store our contract details:

```typescript
// Create new file: lib/contracts/SimpleToken.ts
// This file stores our contract's information

// ABI = Application Binary Interface
// Think of it as the contract's "instruction manual"
// It lists all the functions we can call
export const SimpleTokenABI = [
  // View functions (free to call, just reading data)
  "function name() view returns (string)",           // Get token name
  "function symbol() view returns (string)",         // Get token symbol (like "ETH")
  "function totalSupply() view returns (uint256)",   // Total tokens that exist
  "function balanceOf(address) view returns (uint256)",  // Check someone's balance
  
  // Write function (costs gas, changes blockchain state)
  "function transfer(address to, uint256 amount) returns (bool)",  // Send tokens
  
  // Event (logs that we can listen to)
  "event Transfer(address indexed from, address indexed to, uint256 value)"
] as const;  // "as const" makes TypeScript happy

// Your deployed contract's address (you'll get this after deploying)
// For now, let's use a placeholder
export const SimpleTokenAddress = '0x...YOUR_CONTRACT_ADDRESS_HERE' as `0x${string}`;

// 🎓 Where do I get these?
// 1. ABI: From your contract after compiling in Hardhat/Remix
// 2. Address: After deploying your contract to Sepolia
// 3. We'll use a real token contract from Sepolia for testing!
```

---

### Reading Token Balance

Now let's create a component that shows the user's token balance:

```typescript
// Create new file: components/TokenBalance.tsx
// This component displays how many tokens the user has

'use client';

import { useAccount, useReadContract } from 'wagmi';  // Wagmi hooks
import { SimpleTokenABI, SimpleTokenAddress } from '@/lib/contracts/SimpleToken';
import { formatUnits } from 'viem';  // Formats big numbers

export default function TokenBalance() {
  // Get the connected user's address
  // useAccount is a Wagmi hook that gives us wallet info
  const { address, isConnected } = useAccount();
  
  // ℹ️ What's a "hook"?
  // A hook is a special React function that starts with "use"
  // It lets us tap into React/Wagmi features
  
  // If wallet not connected, show nothing
  if (!isConnected) {
    return (
      <div className="p-6 bg-yellow-50 border border-yellow-200 rounded-lg">
        <p className="text-yellow-800">
          👉 Connect your wallet to see your balance
        </p>
      </div>
    );
  }

  // Read token name from blockchain
  const { data: name } = useReadContract({
    address: SimpleTokenAddress,     // Where the contract is
    abi: SimpleTokenABI,             // The contract's "menu"
    functionName: 'name',            // What we want to call
    // No 'args' needed because name() takes no parameters
  });
  
  // ℹ️ What just happened?
  // useReadContract automatically:
  // 1. Connects to Ethereum
  // 2. Calls the contract's name() function
  // 3. Returns the result in 'data'
  // 4. Updates automatically if anything changes!

  // Read token symbol (like "ETH" or "USDC")
  const { data: symbol } = useReadContract({
    address: SimpleTokenAddress,
    abi: SimpleTokenABI,
    functionName: 'symbol',
  });

  // Read user's balance
  const { data: balance, isLoading } = useReadContract({
    address: SimpleTokenAddress,
    abi: SimpleTokenABI,
    functionName: 'balanceOf',
    args: [address],  // Check THIS user's balance
    // args is an array of parameters the function needs
  });
  
  // ℹ️ The blockchain stores numbers in "wei" (smallest unit)
  // Like: 1 ETH = 1,000,000,000,000,000,000 wei
  // We use formatUnits to make it readable

  return (
    <div className="p-6 bg-white rounded-lg shadow-lg">
      <h2 className="text-2xl font-bold mb-4">Your Balance 💰</h2>
      
      {/* Token Information */}
      <div className="space-y-2">
        <p className="text-gray-600">
          <span className="font-semibold">Token:</span> {name || 'Loading...'}
        </p>
        <p className="text-gray-600">
          <span className="font-semibold">Symbol:</span> {symbol || 'Loading...'}
        </p>
        
        {/* Balance Display */}
        <div className="mt-4 p-4 bg-blue-50 rounded">
          {isLoading ? (
            <p className="text-blue-600">Loading balance... ⏳</p>
          ) : (
            <p className="text-3xl font-bold text-blue-600">
              {/* Format the balance from wei to readable format */}
              {balance ? formatUnits(balance, 18) : '0'} {symbol}
            </p>
          )}
        </div>
        
        {/* Your Wallet Address */}
        <p className="text-sm text-gray-500 mt-4">
          Your address: {address?.slice(0, 6)}...{address?.slice(-4)}
        </p>
      </div>
    </div>
  );
}
```

**🎓 Let's break down what we learned:**

1. **`useAccount()`** - Gets info about the connected wallet
   - Returns: `address`, `isConnected`, `chain`, etc.

2. **`useReadContract()`** - Reads data from blockchain
   - Free (no gas cost!)
   - Returns immediately
   - Updates automatically

3. **`formatUnits()`** - Makes big numbers human-readable
   - Input: "5000000000000000000" wei
   - Output: "5.0" ETH

---

### Adding It To Your Homepage

```typescript
// Edit: app/page.tsx

'use client';

import { useAccount } from 'wagmi';  // To check if wallet is connected
import ConnectWallet from '@/components/ConnectButton';
import TokenBalance from '@/components/TokenBalance';  // Our new component!

export default function Home() {
  const { isConnected } = useAccount();  // Check connection status

  return (
    <main className="min-h-screen bg-gray-50">
      {/* Header */}
      <header className="bg-white shadow">
        <div className="max-w-7xl mx-auto px-4 py-4 flex justify-between items-center">
          <h1 className="text-3xl font-bold">My First DApp 🚀</h1>
          <ConnectWallet />
        </div>
      </header>

      {/* Main Content */}
      <div className="max-w-7xl mx-auto px-4 py-8">
        {/* Show different content based on connection status */}
        {!isConnected ? (
          // Not connected - show welcome message
          <div className="text-center py-20">
            <h2 className="text-4xl font-bold mb-4">
              Welcome to My Token DApp
            </h2>
            <p className="text-xl text-gray-600 mb-8">
              Connect your wallet to get started 👆
            </p>
          </div>
        ) : (
          // Connected - show balance
          <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
            <TokenBalance />  {/* Show token balance */}
            {/* We'll add more components here later! */}
          </div>
        )}
      </div>
    </main>
  );
}
```

---

### 🧪 Test It Out!

1. Make sure your dev server is running (`npm run dev`)
2. Go to http://localhost:3000
3. Connect your wallet
4. You should see your token balance!

**⚠️ Common Issues:**

**Problem:** "Contract address is invalid"
- **Solution:** Make sure you replaced `YOUR_CONTRACT_ADDRESS_HERE` with a real address

**Problem:** Balance shows "0"
- **Solution:** Your wallet might not have any of that token! That's okay, we'll fix this in the next section.

**Problem:** "Loading..." forever
- **Solution:** Check your internet connection, or the blockchain RPC might be slow

---

## 🚀 Step 4: Writing to the Blockchain (Sending Transactions)

### Understanding the Difference: Reading vs Writing

**Reading (What we just did):**
- ✅ Free (no cost!)
- ✅ Instant
- ✅ Just looking at data
- Example: Checking your bank balance

**Writing (What we're about to do):**
- ⚠️ Costs gas (you pay a small fee)
- ⚠️ Takes ~15 seconds to complete
- ⚠️ Changes blockchain state
- Example: Sending money to someone

**What happens when you send a transaction:**
```
1. You click "Send" button
2. MetaMask popup appears showing gas cost
3. You approve → Transaction sent to blockchain
4. Miners/validators process it (~15 seconds)
5. Transaction confirmed ✅
6. Blockchain is updated
```

---

### Creating a Token Transfer Component

Let's build a form where users can send tokens to others:

```typescript
// Create new file: components/TransferTokens.tsx
// This lets users send tokens to other addresses

'use client';

import { useState } from 'react';  // For form state
import { useAccount, useWriteContract, useWaitForTransactionReceipt } from 'wagmi';
import { SimpleTokenABI, SimpleTokenAddress } from '@/lib/contracts/SimpleToken';
import { parseUnits } from 'viem';  // Converts human numbers to blockchain numbers

export default function TransferTokens() {
  // Get connected wallet address
  const { address } = useAccount();
  
  // Form state (what user types)
  const [recipient, setRecipient] = useState('');  // Who to send to
  const [amount, setAmount] = useState('');        // How much to send

  // 🔧 useWriteContract - Sends transactions to blockchain
  const { 
    data: hash,        // Transaction hash (like a receipt number)
    isPending,         // True while waiting for user to approve in MetaMask
    writeContract,     // Function to call the contract
    error              // Any errors that occur
  } = useWriteContract();

  // 🔍 useWaitForTransactionReceipt - Waits for transaction to be mined
  const { 
    isLoading: isConfirming,   // True while transaction is being processed
    isSuccess: isConfirmed      // True when transaction is complete
  } = useWaitForTransactionReceipt({ 
      hash,  // The transaction hash we're watching
    });

  // Handle form submission
  const handleTransfer = async (e: React.FormEvent) => {
    e.preventDefault();  // Prevent page refresh
    
    // Validation: Make sure fields aren't empty
    if (!recipient || !amount) {
      alert('Please fill in both fields!');
      return;
    }

    // Validation: Check if recipient address looks valid
    if (!recipient.startsWith('0x') || recipient.length !== 42) {
      alert('Invalid Ethereum address! Should start with 0x and be 42 characters long.');
      return;
    }

    try {
      // Convert amount from "5.0" to "5000000000000000000" (wei)
      // Tokens usually have 18 decimals
      const amountInWei = parseUnits(amount, 18);
      
      // ℹ️ What is parseUnits?
      // User types: "5.0"
      // parseUnits converts to: "5000000000000000000"
      // Because blockchain stores tiny fractions!

      // 🚀 Send the transaction!
      writeContract({
        address: SimpleTokenAddress,           // Contract to call
        abi: SimpleTokenABI,                  // Contract's "menu"
        functionName: 'transfer',             // Which function to call
        args: [recipient as `0x${string}`, amountInWei],  // Function parameters
        // Args explained:
        // 1. recipient: Who receives the tokens
        // 2. amountInWei: How many tokens (in wei)
      });
      
      // At this point, MetaMask will popup!
      // User needs to approve the transaction
      
    } catch (err) {
      console.error('Transfer error:', err);
      alert('Something went wrong! Check the console for details.');
    }
  };

  return (
    <div className="p-6 bg-white rounded-lg shadow-lg">
      <h2 className="text-2xl font-bold mb-4">Send Tokens 📤</h2>
      
      <form onSubmit={handleTransfer} className="space-y-4">
        {/* Recipient Address Input */}
        <div>
          <label className="block text-sm font-medium mb-2 text-gray-700">
            Recipient Address
          </label>
          <input
            type="text"
            value={recipient}
            onChange={(e) => setRecipient(e.target.value)}
            placeholder="0x1234...abcd"
            className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
            disabled={isPending || isConfirming}  // Disable while processing
          />
          <p className="text-xs text-gray-500 mt-1">
            💡 Tip: Should start with 0x and be 42 characters
          </p>
        </div>

        {/* Amount Input */}
        <div>
          <label className="block text-sm font-medium mb-2 text-gray-700">
            Amount
          </label>
          <input
            type="number"
            value={amount}
            onChange={(e) => setAmount(e.target.value)}
            placeholder="0.0"
            step="0.000001"  // Allow decimals
            min="0"
            className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
            disabled={isPending || isConfirming}
          />
          <p className="text-xs text-gray-500 mt-1">
            💡 Tip: Check your balance first!
          </p>
        </div>

        {/* Submit Button */}
        <button
          type="submit"
          disabled={isPending || isConfirming || !recipient || !amount}
          className="w-full bg-blue-500 text-white p-3 rounded-lg hover:bg-blue-600 
                     disabled:bg-gray-400 disabled:cursor-not-allowed font-semibold
                     transition-colors duration-200"
        >
          {/* Button text changes based on state */}
          {isPending ? '⏳ Waiting for approval...' : 
           isConfirming ? '⏳ Processing transaction...' : 
           '🚀 Send Tokens'}
        </button>
      </form>

      {/* Transaction Status Messages */}
      
      {/* Show transaction hash after sending */}
      {hash && (
        <div className="mt-4 p-3 bg-blue-50 rounded-lg">
          <p className="text-sm font-semibold text-blue-800 mb-1">
            Transaction Sent! 🎉
          </p>
          <p className="text-xs text-blue-600 mb-2">
            Hash: {hash.slice(0, 10)}...{hash.slice(-8)}
          </p>
          <a 
            href={`https://sepolia.etherscan.io/tx/${hash}`}
            target="_blank"
            rel="noopener noreferrer"
            className="text-xs text-blue-500 hover:underline"
          >
            View on Etherscan →
          </a>
        </div>
      )}

      {/* Show success message when confirmed */}
      {isConfirmed && (
        <div className="mt-4 p-4 bg-green-100 border border-green-400 rounded-lg">
          <p className="text-green-800 font-semibold">
            ✅ Transfer Successful!
          </p>
          <p className="text-sm text-green-700 mt-1">
            Your tokens have been sent! Check your balance.
          </p>
        </div>
      )}

      {/* Show error message if something went wrong */}
      {error && (
        <div className="mt-4 p-4 bg-red-100 border border-red-400 rounded-lg">
          <p className="text-red-800 font-semibold">
            ❌ Transaction Failed
          </p>
          <p className="text-sm text-red-700 mt-1">
            {error.message}
          </p>
        </div>
      )}

      {/* Help text */}
      <div className="mt-6 p-4 bg-gray-50 rounded-lg">
        <p className="text-sm text-gray-600">
          <strong>How transactions work:</strong>
        </p>
        <ol className="text-sm text-gray-600 mt-2 space-y-1 list-decimal list-inside">
          <li>Click "Send Tokens"</li>
          <li>MetaMask popup appears</li>
          <li>Review gas fee and approve</li>
          <li>Wait ~15 seconds for confirmation</li>
          <li>Done! Tokens are transferred</li>
        </ol>
      </div>
    </div>
  );
}
```

**🎓 Let's understand what we learned:**

1. **`useWriteContract()`** - Sends transactions
   - Costs gas (user pays)
   - Requires MetaMask approval
   - Returns transaction hash

2. **`useWaitForTransactionReceipt()`** - Waits for confirmation
   - Polls blockchain until transaction is mined
   - Gives us `isConfirming` and `isConfirmed` states

3. **`parseUnits()`** - Converts human numbers to blockchain format
   - Input: "5.0"
   - Output: "5000000000000000000" wei

4. **Transaction States:**
   - `isPending` - Waiting for user to approve in MetaMask
   - `isConfirming` - Transaction sent, waiting for miners
   - `isConfirmed` - Transaction complete!

---

### Add Transfer to Homepage

```typescript
// Edit: app/page.tsx
// Add the Transfer component

'use client';

import { useAccount } from 'wagmi';
import ConnectWallet from '@/components/ConnectButton';
import TokenBalance from '@/components/TokenBalance';
import TransferTokens from '@/components/TransferTokens';  // Import new component

export default function Home() {
  const { isConnected } = useAccount();

  return (
    <main className="min-h-screen bg-gray-50">
      {/* Header */}
      <header className="bg-white shadow">
        <div className="max-w-7xl mx-auto px-4 py-4 flex justify-between items-center">
          <h1 className="text-3xl font-bold">My First DApp 🚀</h1>
          <ConnectWallet />
        </div>
      </header>

      {/* Main Content */}
      <div className="max-w-7xl mx-auto px-4 py-8">
        {!isConnected ? (
          <div className="text-center py-20">
            <h2 className="text-4xl font-bold mb-4">
              Welcome to My Token DApp
            </h2>
            <p className="text-xl text-gray-600 mb-8">
              Connect your wallet to get started 👆
            </p>
          </div>
        ) : (
          // Show both components side by side
          <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
            <TokenBalance />    {/* Left: Show balance */}
            <TransferTokens />  {/* Right: Send tokens */}
          </div>
        )}
      </div>
    </main>
  );
}
```

---

### 🧪 Test Your Transfer Function!

1. Make sure you have some test tokens in your wallet
2. Get a friend's address (or use a second wallet)
3. Try sending a small amount (like 0.1 tokens)
4. Watch the magic happen! ✨

**What you'll see:**
1. Click "Send Tokens"
2. MetaMask pops up → Click "Confirm"
3. "Processing transaction..." appears
4. After ~15 seconds → "✅ Transfer Successful!"
5. Check Etherscan to see your transaction!

**⚠️ Common Issues:**

**Problem:** "Insufficient funds"
- **Solution:** You need ETH for gas! Get some from Sepolia faucet

**Problem:** "Transaction rejected"
- **Solution:** You clicked "Reject" in MetaMask. Try again!

**Problem:** Transaction pending forever
- **Solution:** Network might be congested. Wait a bit longer or check Etherscan

---

## 🎨 Step 5: Making It Look Professional

    try {
      // Convert amount to wei (18 decimals)
      const amountInWei = parseUnits(amount, 18);

      // Execute transaction
      writeContract({
        address: SimpleTokenAddress,
        abi: SimpleTokenABI,
        functionName: 'transfer',
        args: [recipient as `0x${string}`, amountInWei],
      });
    } catch (err) {
      console.error('Transfer error:', err);
    }
  };

  return (
    <div className="p-6 bg-white rounded-lg shadow">
      <h2 className="text-2xl font-bold mb-4">Transfer Tokens</h2>
      
      <form onSubmit={handleTransfer} className="space-y-4">
        <div>
          <label className="block text-sm font-medium mb-2">
            Recipient Address
          </label>
          <input
            type="text"
            value={recipient}
            onChange={(e) => setRecipient(e.target.value)}
            placeholder="0x..."
            className="w-full p-2 border rounded"
          />
        </div>

        <div>
          <label className="block text-sm font-medium mb-2">
            Amount
          </label>
          <input
            type="number"
            value={amount}
            onChange={(e) => setAmount(e.target.value)}
            placeholder="0.0"
            step="0.01"
            className="w-full p-2 border rounded"
          />
        </div>

        <button
          type="submit"
          disabled={isPending || isConfirming}
          className="w-full bg-blue-500 text-white p-3 rounded hover:bg-blue-600 disabled:bg-gray-400"
        >
          {isPending ? 'Waiting for approval...' : 
           isConfirming ? 'Confirming transaction...' : 
           'Transfer'}
        </button>
      </form>

      {/* Transaction feedback */}
      {hash && (
        <div className="mt-4">
          <p className="text-sm">Transaction Hash:</p>
          <a 
            href={`https://sepolia.etherscan.io/tx/${hash}`}
            target="_blank"
            rel="noopener noreferrer"
            className="text-blue-500 text-sm break-all hover:underline"
          >
            {hash}
          </a>
        </div>
      )}

      {isConfirmed && (
        <div className="mt-4 p-3 bg-green-100 text-green-800 rounded">
          ✅ Transfer successful!
        </div>
      )}

      {error && (
        <div className="mt-4 p-3 bg-red-100 text-red-800 rounded">
          ❌ Error: {error.message}
        </div>
      )}
    </div>
  );
}
```

### Listening to Events

```typescript
// hooks/useTokenTransfers.ts
import { useEffect, useState } from 'use';
import { usePublicClient, useAccount } from 'wagmi';
import { SimpleTokenABI, SimpleTokenAddress } from '@/lib/contracts/SimpleToken';

export function useTokenTransfers() {
  const { address } = useAccount();
  const publicClient = usePublicClient();
  const [transfers, setTransfers] = useState<any[]>([]);

  useEffect(() => {
    if (!address || !publicClient) return;

    // Watch for Transfer events
    const unwatch = publicClient.watchContractEvent({
      address: SimpleTokenAddress,
      abi: SimpleTokenABI,
      eventName: 'Transfer',
      args: {
        // Filter for transfers TO this address
        to: address,
      },
      onLogs: (logs) => {
        console.log('New transfer:', logs);
        setTransfers((prev) => [...logs, ...prev]);
      },
    });

    return () => unwatch();
  }, [address, publicClient]);

  return transfers;
}

// Usage in component:
// const transfers = useTokenTransfers();
```

---

## 🎨 Building Complete DApp UI

### Main Page with All Features

```typescript
// app/page.tsx
'use client';

import { useAccount } from 'wagmi';
import ConnectWallet from '@/components/ConnectButton';
import TokenBalance from '@/components/TokenBalance';
import TransferTokens from '@/components/TransferTokens';
import TokenInfo from '@/components/TokenInfo';

export default function Home() {
  const { isConnected } = useAccount();

  return (
    <main className="min-h-screen bg-gray-50">
      {/* Header */}
      <header className="bg-white shadow">
        <div className="max-w-7xl mx-auto px-4 py-4 flex justify-between items-center">
          <h1 className="text-3xl font-bold">My Token DApp</h1>
          <ConnectWallet />
        </div>
      </header>

      {/* Content */}
      <div className="max-w-7xl mx-auto px-4 py-8">
        {!isConnected ? (
          <div className="text-center py-20">
            <h2 className="text-4xl font-bold mb-4">
              Welcome to My Token DApp
            </h2>
            <p className="text-xl text-gray-600 mb-8">
              Connect your wallet to get started
            </p>
          </div>
        ) : (
          <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
            <TokenInfo />
            <TokenBalance />
            <TransferTokens />
          </div>
        )}
      </div>
    </main>
  );
}
```

---

## 🔐 Web3 Authentication with Clerk

### Setup Clerk Authentication

```bash
npm install @clerk/nextjs
```

```typescript
// middleware.ts
import { clerkMiddleware } from '@clerk/nextjs/server';

export default clerkMiddleware();

export const config = {
  matcher: [
    '/((?!_next|[^?]*\\.(?:html?|css|js(?!on)|jpe?g|webp|png|gif|svg|ttf|woff2?|ico|csv|docx?|xlsx?|zip|webmanifest)).*)',
    '/(api|trpc)(.*)',
  ],
};
```

```typescript
// app/layout.tsx
import { ClerkProvider } from '@clerk/nextjs';

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <ClerkProvider>
      <html lang="en">
        <body>
          <Providers>{children}</Providers>
        </body>
      </html>
    </ClerkProvider>
  );
}
```

---

## 📱 Responsive Design Best Practices

### Tailwind CSS for Web3 UIs

```typescript
// components/TokenCard.tsx
export default function TokenCard({ token }: { token: Token }) {
  return (
    <div className="
      p-6 
      bg-gradient-to-br from-blue-500 to-purple-600 
      rounded-2xl 
      shadow-lg 
      hover:shadow-2xl 
      transition-all 
      duration-300
      transform 
      hover:-translate-y-1
    ">
      <div className="flex items-center justify-between mb-4">
        <h3 className="text-2xl font-bold text-white">{token.name}</h3>
        <span className="text-white/80 text-lg">{token.symbol}</span>
      </div>
      
      <p className="text-4xl font-bold text-white mb-2">
        {token.balance} {token.symbol}
      </p>
      
      <p className="text-white/60 text-sm">
        ≈ ${token.usdValue.toLocaleString()}
      </p>
    </div>
  );
}
```

---

## 🚀 Deployment

### Deploying to Vercel

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel

# Production deployment
vercel --prod
```

### Environment Variables

```bash
# .env.local (for development)
NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID=your_project_id
NEXT_PUBLIC_ALCHEMY_API_KEY=your_alchemy_key
NEXT_PUBLIC_CONTRACT_ADDRESS=0x...
```

**Vercel Dashboard:**
1. Go to vercel.com
2. Import Git repository
3. Add environment variables
4. Deploy

---

## � MERN Stack Integration with Blockchain

### Why Combine Traditional Backend with Web3?

**Use Cases:**
- 📊 Store off-chain data (user profiles, metadata)
- 🔍 Index blockchain events for faster queries
- 📧 Send email notifications for on-chain events
- 🖼️ Cache expensive blockchain queries
- 🔐 Implement traditional authentication alongside wallet auth
- 📈 Analytics and user tracking

---

### Architecture: MERN + Web3

```
┌─────────────────────────────────────────────────────┐
│                   Frontend (React)                   │
│  - RainbowKit/Wagmi for Web3                        │
│  - Axios for API calls                              │
└──────────────┬──────────────────────┬────────────────┘
               │                      │
               ▼                      ▼
    ┌──────────────────┐    ┌──────────────────┐
    │   Blockchain     │    │   Backend API    │
    │   (Ethereum)     │    │   (Node/Express) │
    │                  │    │   - MongoDB      │
    │  Smart Contracts │◄───┤   - Event Indexer│
    └──────────────────┘    └──────────────────┘
```

---

### Setting Up Express Backend

**Install Dependencies:**

```bash
# Create backend folder
mkdir backend
cd backend
npm init -y

# Install dependencies
npm install express mongoose ethers@6 dotenv cors
npm install --save-dev nodemon typescript @types/node @types/express
```

**Backend Structure:**

```
backend/
├── src/
│   ├── config/
│   │   ├── database.ts        # MongoDB connection
│   │   └── blockchain.ts      # Ethers.js provider
│   ├── models/
│   │   ├── User.ts            # User schema
│   │   └── Transaction.ts     # Transaction schema
│   ├── routes/
│   │   ├── auth.ts            # Authentication routes
│   │   └── blockchain.ts      # Blockchain data routes
│   ├── services/
│   │   └── eventIndexer.ts    # Listen to contract events
│   └── index.ts               # Express app entry
├── .env
└── package.json
```

---

### MongoDB Schema for Blockchain Data

**User Model (models/User.ts):**

```typescript
import mongoose, { Schema, Document } from 'mongoose';

// Define interface for TypeScript
interface IUser extends Document {
  walletAddress: string;
  email?: string;
  username?: string;
  nftIds: number[];
  totalTransactions: number;
  firstSeenAt: Date;
  lastSeenAt: Date;
}

// Create schema
const UserSchema: Schema = new Schema({
  // Wallet address as primary identifier
  walletAddress: {
    type: String,
    required: true,
    unique: true,
    lowercase: true,
    index: true  // Index for faster queries
  },
  // Optional traditional fields
  email: {
    type: String,
    unique: true,
    sparse: true  // Allow null values
  },
  username: {
    type: String,
    unique: true,
    sparse: true
  },
  // Blockchain-related data
  nftIds: [{
    type: Number
  }],
  totalTransactions: {
    type: Number,
    default: 0
  },
  // Timestamps
  firstSeenAt: {
    type: Date,
    default: Date.now
  },
  lastSeenAt: {
    type: Date,
    default: Date.now
  }
}, {
  timestamps: true  // Auto-create createdAt and updatedAt
});

export default mongoose.model<IUser>('User', UserSchema);
```

**Transaction Model (models/Transaction.ts):**

```typescript
import mongoose, { Schema, Document } from 'mongoose';

interface ITransaction extends Document {
  txHash: string;
  blockNumber: number;
  from: string;
  to: string;
  value: string;
  contractAddress?: string;
  eventName?: string;
  eventData?: any;
  timestamp: Date;
  status: 'pending' | 'confirmed' | 'failed';
}

const TransactionSchema: Schema = new Schema({
  // Unique transaction hash
  txHash: {
    type: String,
    required: true,
    unique: true,
    index: true
  },
  blockNumber: {
    type: Number,
    required: true,
    index: true  // Query by block
  },
  // Addresses
  from: {
    type: String,
    required: true,
    lowercase: true,
    index: true  // Query user's transactions
  },
  to: {
    type: String,
    required: true,
    lowercase: true,
    index: true
  },
  // Transaction value (in wei as string)
  value: {
    type: String,
    default: '0'
  },
  // Contract interaction details
  contractAddress: {
    type: String,
    lowercase: true,
    index: true
  },
  eventName: String,  // e.g., "Transfer", "Mint"
  eventData: Schema.Types.Mixed,  // Event parameters
  // Metadata
  timestamp: {
    type: Date,
    required: true,
    index: true  // Query by date range
  },
  status: {
    type: String,
    enum: ['pending', 'confirmed', 'failed'],
    default: 'confirmed'
  }
}, {
  timestamps: true
});

// Compound index for efficient user transaction queries
TransactionSchema.index({ from: 1, timestamp: -1 });
TransactionSchema.index({ to: 1, timestamp: -1 });

export default mongoose.model<ITransaction>('Transaction', TransactionSchema);
```

---

### Blockchain Provider Configuration (config/blockchain.ts)

```typescript
import { ethers } from 'ethers';

// Initialize provider (Alchemy recommended for production)
const provider = new ethers.JsonRpcProvider(
  process.env.ALCHEMY_RPC_URL || 'https://eth-sepolia.g.alchemy.com/v2/YOUR-API-KEY'
);

// Contract configuration
export const NFT_CONTRACT_ADDRESS = process.env.NFT_CONTRACT_ADDRESS!;
export const NFT_CONTRACT_ABI = [
  // Add your contract ABI here
  "event Transfer(address indexed from, address indexed to, uint256 indexed tokenId)",
  "function balanceOf(address owner) view returns (uint256)",
  "function tokenURI(uint256 tokenId) view returns (string)"
];

// Create contract instance
export const nftContract = new ethers.Contract(
  NFT_CONTRACT_ADDRESS,
  NFT_CONTRACT_ABI,
  provider
);

export default provider;
```

---

### Event Indexer Service (services/eventIndexer.ts)

**Purpose:** Listen to smart contract events in real-time and save to MongoDB

```typescript
import { ethers } from 'ethers';
import { nftContract } from '../config/blockchain';
import Transaction from '../models/Transaction';
import User from '../models/User';

class EventIndexer {
  // Start listening to contract events
  async startListening() {
    console.log('🎧 Event indexer started...');

    // Listen to Transfer events (NFT transfers)
    nftContract.on('Transfer', async (from, to, tokenId, event) => {
      try {
        console.log(`📤 Transfer detected: ${from} → ${to} | TokenID: ${tokenId}`);

        // Get transaction details
        const tx = await event.getTransaction();
        const block = await event.getBlock();

        // Save transaction to database
        await Transaction.create({
          txHash: event.transactionHash,
          blockNumber: event.blockNumber,
          from: from.toLowerCase(),
          to: to.toLowerCase(),
          value: '0',  // NFT transfers don't have ETH value
          contractAddress: nftContract.target.toLowerCase(),
          eventName: 'Transfer',
          eventData: {
            tokenId: tokenId.toString(),
            from,
            to
          },
          timestamp: new Date(block.timestamp * 1000),
          status: 'confirmed'
        });

        // Update user records
        await this.updateUserData(from, to, tokenId);

        console.log('✅ Event indexed successfully');
      } catch (error) {
        console.error('❌ Error indexing event:', error);
      }
    });

    // Handle errors
    nftContract.on('error', (error) => {
      console.error('❌ Contract listener error:', error);
    });
  }

  // Update user data when NFT is transferred
  async updateUserData(from: string, to: string, tokenId: bigint) {
    const now = new Date();

    // Update sender (remove NFT)
    if (from !== ethers.ZeroAddress) {
      await User.findOneAndUpdate(
        { walletAddress: from.toLowerCase() },
        {
          $pull: { nftIds: Number(tokenId) },  // Remove tokenId
          $inc: { totalTransactions: 1 },      // Increment counter
          lastSeenAt: now
        },
        { upsert: true, new: true }  // Create if doesn't exist
      );
    }

    // Update receiver (add NFT)
    await User.findOneAndUpdate(
      { walletAddress: to.toLowerCase() },
      {
        $addToSet: { nftIds: Number(tokenId) },  // Add tokenId (no duplicates)
        $inc: { totalTransactions: 1 },
        lastSeenAt: now,
        $setOnInsert: { firstSeenAt: now }  // Set only on creation
      },
      { upsert: true, new: true }
    );
  }

  // Index historical events (one-time setup)
  async indexHistoricalEvents(fromBlock: number, toBlock: number | string = 'latest') {
    console.log(`📚 Indexing historical events from block ${fromBlock}...`);

    const filter = nftContract.filters.Transfer();
    const events = await nftContract.queryFilter(filter, fromBlock, toBlock);

    console.log(`Found ${events.length} historical events`);

    for (const event of events) {
      // Type assertion for event arguments
      const [from, to, tokenId] = event.args as [string, string, bigint];
      
      // Check if already indexed
      const exists = await Transaction.findOne({ txHash: event.transactionHash });
      if (exists) continue;

      // Get block details
      const block = await event.getBlock();

      // Save to database
      await Transaction.create({
        txHash: event.transactionHash,
        blockNumber: event.blockNumber,
        from: from.toLowerCase(),
        to: to.toLowerCase(),
        value: '0',
        contractAddress: nftContract.target.toLowerCase(),
        eventName: 'Transfer',
        eventData: { tokenId: tokenId.toString(), from, to },
        timestamp: new Date(block.timestamp * 1000),
        status: 'confirmed'
      });

      // Update users
      await this.updateUserData(from, to, tokenId);
    }

    console.log('✅ Historical indexing complete');
  }

  // Stop listening (cleanup)
  stopListening() {
    nftContract.removeAllListeners();
    console.log('🛑 Event indexer stopped');
  }
}

export default new EventIndexer();
```

---

### Express API Routes (routes/blockchain.ts)

```typescript
import express, { Request, Response } from 'express';
import Transaction from '../models/Transaction';
import User from '../models/User';
import { nftContract } from '../config/blockchain';
import { ethers } from 'ethers';

const router = express.Router();

// GET /api/blockchain/user/:address
// Get user data by wallet address
router.get('/user/:address', async (req: Request, res: Response) => {
  try {
    const { address } = req.params;

    // Validate address
    if (!ethers.isAddress(address)) {
      return res.status(400).json({ error: 'Invalid Ethereum address' });
    }

    // Find user in database
    let user = await User.findOne({ walletAddress: address.toLowerCase() });

    // If user doesn't exist, fetch from blockchain
    if (!user) {
      const balance = await nftContract.balanceOf(address);
      
      user = await User.create({
        walletAddress: address.toLowerCase(),
        nftIds: [],
        totalTransactions: 0
      });
    }

    res.json(user);
  } catch (error) {
    console.error('Error fetching user:', error);
    res.status(500).json({ error: 'Internal server error' });
  }
});

// GET /api/blockchain/transactions/:address
// Get user's transaction history
router.get('/transactions/:address', async (req: Request, res: Response) => {
  try {
    const { address } = req.params;
    const { page = 1, limit = 20 } = req.query;

    // Validate address
    if (!ethers.isAddress(address)) {
      return res.status(400).json({ error: 'Invalid address' });
    }

    // Query transactions (sent or received)
    const transactions = await Transaction.find({
      $or: [
        { from: address.toLowerCase() },
        { to: address.toLowerCase() }
      ]
    })
    .sort({ timestamp: -1 })  // Most recent first
    .limit(Number(limit))
    .skip((Number(page) - 1) * Number(limit));

    // Get total count for pagination
    const total = await Transaction.countDocuments({
      $or: [
        { from: address.toLowerCase() },
        { to: address.toLowerCase() }
      ]
    });

    res.json({
      transactions,
      pagination: {
        page: Number(page),
        limit: Number(limit),
        total,
        pages: Math.ceil(total / Number(limit))
      }
    });
  } catch (error) {
    console.error('Error fetching transactions:', error);
    res.status(500).json({ error: 'Internal server error' });
  }
});

// GET /api/blockchain/nft/:tokenId
// Get NFT metadata
router.get('/nft/:tokenId', async (req: Request, res: Response) => {
  try {
    const { tokenId } = req.params;

    // Fetch from blockchain
    const tokenURI = await nftContract.tokenURI(tokenId);
    
    // If IPFS, convert to HTTP gateway
    let metadataURL = tokenURI;
    if (tokenURI.startsWith('ipfs://')) {
      metadataURL = tokenURI.replace('ipfs://', 'https://ipfs.io/ipfs/');
    }

    // Fetch metadata JSON
    const response = await fetch(metadataURL);
    const metadata = await response.json();

    res.json({
      tokenId,
      tokenURI,
      metadata
    });
  } catch (error) {
    console.error('Error fetching NFT:', error);
    res.status(500).json({ error: 'NFT not found' });
  }
});

// GET /api/blockchain/stats
// Get overall platform statistics
router.get('/stats', async (req: Request, res: Response) => {
  try {
    const totalUsers = await User.countDocuments();
    const totalTransactions = await Transaction.countDocuments();
    const activeUsersToday = await User.countDocuments({
      lastSeenAt: { $gte: new Date(Date.now() - 24 * 60 * 60 * 1000) }
    });

    res.json({
      totalUsers,
      totalTransactions,
      activeUsersToday
    });
  } catch (error) {
    console.error('Error fetching stats:', error);
    res.status(500).json({ error: 'Internal server error' });
  }
});

export default router;
```

---

### Main Express Server (index.ts)

```typescript
import express from 'express';
import mongoose from 'mongoose';
import cors from 'cors';
import dotenv from 'dotenv';
import blockchainRoutes from './routes/blockchain';
import eventIndexer from './services/eventIndexer';

// Load environment variables
dotenv.config();

const app = express();
const PORT = process.env.PORT || 5000;

// Middleware
app.use(cors());
app.use(express.json());

// Connect to MongoDB
mongoose.connect(process.env.MONGODB_URI!)
  .then(() => {
    console.log('✅ Connected to MongoDB');
    
    // Start event indexer after DB connection
    eventIndexer.startListening();
    
    // Optionally index historical events (run once)
    // eventIndexer.indexHistoricalEvents(5000000);  // From block number
  })
  .catch((error) => console.error('❌ MongoDB connection error:', error));

// Routes
app.use('/api/blockchain', blockchainRoutes);

// Health check
app.get('/health', (req, res) => {
  res.json({ status: 'ok', timestamp: new Date() });
});

// Start server
app.listen(PORT, () => {
  console.log(`🚀 Server running on port ${PORT}`);
});

// Graceful shutdown
process.on('SIGINT', async () => {
  console.log('\n🛑 Shutting down gracefully...');
  eventIndexer.stopListening();
  await mongoose.connection.close();
  process.exit(0);
});
```

---

### Frontend Integration with Backend

**Create API Client (frontend/lib/api.ts):**

```typescript
import axios from 'axios';

const API_BASE_URL = process.env.NEXT_PUBLIC_API_URL || 'http://localhost:5000';

const api = axios.create({
  baseURL: API_BASE_URL,
  headers: {
    'Content-Type': 'application/json'
  }
});

export const blockchainAPI = {
  // Get user data
  getUser: async (address: string) => {
    const response = await api.get(`/api/blockchain/user/${address}`);
    return response.data;
  },

  // Get transaction history
  getTransactions: async (address: string, page = 1, limit = 20) => {
    const response = await api.get(`/api/blockchain/transactions/${address}`, {
      params: { page, limit }
    });
    return response.data;
  },

  // Get NFT metadata
  getNFT: async (tokenId: number) => {
    const response = await api.get(`/api/blockchain/nft/${tokenId}`);
    return response.data;
  },

  // Get platform stats
  getStats: async () => {
    const response = await api.get('/api/blockchain/stats');
    return response.data;
  }
};
```

**Use in React Component:**

```typescript
'use client';
import { useState, useEffect } from 'react';
import { useAccount } from 'wagmi';
import { blockchainAPI } from '@/lib/api';

export default function UserProfile() {
  const { address } = useAccount();
  const [userData, setUserData] = useState(null);
  const [transactions, setTransactions] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    if (address) {
      fetchUserData();
    }
  }, [address]);

  const fetchUserData = async () => {
    try {
      setLoading(true);
      
      // Fetch from backend API (MongoDB)
      const user = await blockchainAPI.getUser(address!);
      const txHistory = await blockchainAPI.getTransactions(address!);
      
      setUserData(user);
      setTransactions(txHistory.transactions);
    } catch (error) {
      console.error('Error fetching user data:', error);
    } finally {
      setLoading(false);
    }
  };

  if (!address) return <div>Connect wallet to view profile</div>;
  if (loading) return <div>Loading...</div>;

  return (
    <div>
      <h2>Your Profile</h2>
      <p>Address: {userData?.walletAddress}</p>
      <p>NFTs Owned: {userData?.nftIds?.length}</p>
      <p>Total Transactions: {userData?.totalTransactions}</p>
      
      <h3>Recent Transactions</h3>
      {transactions.map((tx: any) => (
        <div key={tx.txHash}>
          <p>{tx.eventName}: {tx.eventData?.tokenId}</p>
          <p>{new Date(tx.timestamp).toLocaleString()}</p>
        </div>
      ))}
    </div>
  );
}
```

---

### Environment Variables

**Backend (.env):**

```env
MONGODB_URI=mongodb://localhost:27017/blockchain-dapp
ALCHEMY_RPC_URL=https://eth-sepolia.g.alchemy.com/v2/YOUR-API-KEY
NFT_CONTRACT_ADDRESS=0x...
PORT=5000
```

**Frontend (.env.local):**

```env
NEXT_PUBLIC_API_URL=http://localhost:5000
NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID=your_project_id
```

---

### Running the Full Stack

**Terminal 1 - MongoDB:**
```bash
mongod  # Start MongoDB server
```

**Terminal 2 - Backend:**
```bash
cd backend
npm run dev  # Uses nodemon for hot reload
```

**Terminal 3 - Frontend:**
```bash
cd frontend
npm run dev
```

**Access:**
- Frontend: http://localhost:3000
- Backend API: http://localhost:5000
- MongoDB: mongodb://localhost:27017

---

## �📝 Chapter Summary

**Key Takeaways:**

1. **Next.js** is the modern standard for building DApp frontends (2025)
2. **RainbowKit + Wagmi** provides seamless wallet connection and chain switching
3. **useReadContract** for reading blockchain data (view functions)
4. **useWriteContract** for sending transactions (state-changing functions)
5. **useWaitForTransactionReceipt** for waiting for transaction confirmation
6. **Event watching** enables real-time UI updates
7. **Responsive design** with Tailwind CSS creates professional UIs
8. **Vercel** makes deployment simple and free for hobby projects

---

## 🔗 Reference Links

### Official Documentation
- **Next.js 14:** https://nextjs.org/docs
- **RainbowKit:** https://www.rainbowkit.com/docs/introduction
- **Wagmi:** https://wagmi.sh
- **Viem:** https://viem.sh

### GitHub Repositories
- **RainbowKit Examples:** https://github.com/rainbow-me/rainbowkit/tree/main/examples
- **Wagmi Examples:** https://github.com/wevm/wagmi/tree/main/examples
- **Next.js DApp Template:** https://github.com/vercel/next.js/tree/canary/examples/with-rainbow-kit

### Tools
- **WalletConnect Cloud:** https://cloud.walletconnect.com
- **Alchemy:** https://www.alchemy.com
- **Vercel:** https://vercel.com
- **Tailwind CSS:** https://tailwindcss.com

### Video Tutorials
- **Build a Full Stack DApp:** https://www.youtube.com/watch?v=a0osIaAOFSE
- **RainbowKit Tutorial:** https://www.youtube.com/watch?v=Bz1jmgBfRMM

---

[← Previous](Chapter-05-Intermediate-Solidity.md) | [Index](../Index.md) | [Next Chapter →](Chapter-07-Advanced-DeFi-DePIN.md)

---

*Last Updated: October 25, 2025*
*Blockchain Learning Series for Beginners*