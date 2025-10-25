# Project 11: Full-Stack Voting DApp

**Chapter:** 06 - Full-Stack DApps
**Difficulty:** Advanced
**Estimated Time:** 10-12 hours

---

## 📋 Project Description

Build a complete decentralized voting application with a Solidity smart contract backend and a Next.js frontend. Users connect their wallets, create proposals, and vote on-chain.

---

## 🎯 Learning Objectives

- Integrate smart contracts with React
- Use RainbowKit for wallet connection
- Read and write blockchain data with Wagmi
- Handle transaction states
- Create responsive UI

---

## 🛠️ Technology Stack

- Next.js 14+ (App Router)
- TypeScript
- RainbowKit 2.x
- Wagmi 2.x
- Viem 2.x
- Tailwind CSS
- Solidity 0.8.20+
- Hardhat

---

## ✅ TODO List

### Phase 1: Smart Contract
- [ ] Voting.sol contract with proposals
- [ ] Struct: Proposal (description, voteCount, deadline, executed)
- [ ] `createProposal(description, duration)` function
- [ ] `vote(proposalId)` function
- [ ] Track who voted (prevent double voting)
- [ ] Events: ProposalCreated, Voted
- [ ] Deploy to Sepolia

### Phase 2: Next.js Setup
- [ ] Create Next.js app with TypeScript
- [ ] Install RainbowKit, Wagmi, Viem
- [ ] Setup providers in layout.tsx
- [ ] Configure chains (Sepolia)
- [ ] Add ConnectButton component

### Phase 3: Contract Integration
- [ ] Create contract ABI file
- [ ] Setup contract address constants
- [ ] Use `useReadContract` for reading proposals
- [ ] Use `useWriteContract` for voting
- [ ] Handle transaction confirmations

### Phase 4: UI Components
- [ ] Navbar with ConnectButton
- [ ] ProposalList component
- [ ] ProposalCard (show votes, deadline, vote button)
- [ ] CreateProposal form
- [ ] VotingStats (total proposals, votes)
- [ ] Loading states and error handling

### Phase 5: Features
- [ ] Display all active proposals
- [ ] Show vote counts in real-time
- [ ] Disable vote button if already voted
- [ ] Show countdown timer for deadlines
- [ ] Toast notifications for transactions
- [ ] Refresh data after voting

### Phase 6: Polish
- [ ] Responsive design (mobile-friendly)
- [ ] Dark/light mode toggle
- [ ] Transaction history
- [ ] Filter proposals (active/ended)
- [ ] Search proposals
- [ ] Deploy frontend to Vercel

---

## 💡 Hints

**Hint 1: RainbowKit Setup**
```typescript
// app/providers.tsx
'use client';
import '@rainbow-me/rainbowkit/styles.css';
import { RainbowKitProvider, getDefaultConfig } from '@rainbow-me/rainbowkit';
import { WagmiProvider } from 'wagmi';
import { sepolia } from 'wagmi/chains';
import { QueryClientProvider, QueryClient } from '@tanstack/react-query';

const config = getDefaultConfig({
  appName: 'Voting DApp',
  projectId: 'YOUR_PROJECT_ID', // Get from WalletConnect Cloud
  chains: [sepolia],
});

const queryClient = new QueryClient();

export function Providers({ children }: { children: React.ReactNode }) {
  return (
    <WagmiProvider config={config}>
      <QueryClientProvider client={queryClient}>
        <RainbowKitProvider>{children}</RainbowKitProvider>
      </QueryClientProvider>
    </WagmiProvider>
  );
}
```

**Hint 2: Reading Proposals**
```typescript
import { useReadContract } from 'wagmi';
import votingABI from './abi/Voting.json';

const CONTRACT_ADDRESS = '0x...';

function ProposalList() {
  const { data: proposalCount } = useReadContract({
    address: CONTRACT_ADDRESS,
    abi: votingABI,
    functionName: 'proposalCount',
  });

  const { data: proposal } = useReadContract({
    address: CONTRACT_ADDRESS,
    abi: votingABI,
    functionName: 'proposals',
    args: [0], // Proposal ID
  });

  return (
    <div>
      <p>Total Proposals: {proposalCount?.toString()}</p>
      <p>Description: {proposal?.description}</p>
      <p>Votes: {proposal?.voteCount?.toString()}</p>
    </div>
  );
}
```

**Hint 3: Voting Function**
```typescript
import { useWriteContract, useWaitForTransactionReceipt } from 'wagmi';

function VoteButton({ proposalId }: { proposalId: number }) {
  const { writeContract, data: hash } = useWriteContract();
  
  const { isLoading, isSuccess } = useWaitForTransactionReceipt({
    hash,
  });

  const handleVote = () => {
    writeContract({
      address: CONTRACT_ADDRESS,
      abi: votingABI,
      functionName: 'vote',
      args: [proposalId],
    });
  };

  return (
    <button onClick={handleVote} disabled={isLoading}>
      {isLoading ? 'Voting...' : 'Vote'}
    </button>
  );
}
```

**Hint 4: Smart Contract Structure**
```solidity
contract Voting {
    struct Proposal {
        string description;
        uint256 voteCount;
        uint256 deadline;
        bool executed;
    }
    
    Proposal[] public proposals;
    mapping(uint256 => mapping(address => bool)) public hasVoted;
    
    event ProposalCreated(uint256 indexed proposalId, string description);
    event Voted(uint256 indexed proposalId, address indexed voter);
}
```

**Hint 5: UI Best Practices**
- Show wallet connection status
- Display user's address (truncated)
- Loading spinners during transactions
- Error messages if transaction fails
- Success toast after voting
- Disable buttons when not connected
- Show gas estimates before transaction

---

## 🎨 Bonus Challenges

1. **Token-Based Voting**: Voting power based on token holdings
2. **Delegation**: Delegate your vote to another address
3. **Quadratic Voting**: Prevent whale dominance
4. **Multi-Choice**: More than yes/no votes
5. **IPFS Storage**: Store proposal details on IPFS
6. **Governance**: Execute proposals automatically when passed

---

## 📚 Reference Links

- **RainbowKit Docs:** https://www.rainbowkit.com/docs/introduction
- **Wagmi Docs:** https://wagmi.sh/react/getting-started
- **Viem Docs:** https://viem.sh/
- **Next.js App Router:** https://nextjs.org/docs/app
- **Example DApp:** https://github.com/rainbow-me/rainbowkit/tree/main/examples

---

## ✨ Success Criteria

- ✅ Wallet connection works
- ✅ Can read proposals from contract
- ✅ Can vote on proposals
- ✅ Prevents double voting
- ✅ Real-time updates
- ✅ Responsive design
- ✅ Deployed live

---

*Project for Blockchain Learning Series - Chapter 06*