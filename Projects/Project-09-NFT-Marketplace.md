# Project 09: NFT Marketplace with Tests

**Chapter:** 05 - Intermediate Solidity
**Difficulty:** Advanced
**Estimated Time:** 8-10 hours

---

## 📋 Project Description

Build a complete NFT marketplace where users can list, buy, and cancel NFT sales. Implement using Hardhat framework with comprehensive testing suite and OpenZeppelin contracts.

---

## 🎯 Learning Objectives

- Work with ERC721 tokens
- Implement marketplace logic
- Write Hardhat tests
- Use OpenZeppelin libraries
- Deploy with scripts

---

## 🛠️ Technology Stack

- Hardhat
- Solidity 0.8.20+
- OpenZeppelin Contracts (ERC721, ReentrancyGuard)
- Ethers.js v6
- Chai for testing
- Hardhat Network (local blockchain)

---

## ✅ TODO List

### Phase 1: Project Setup
- [ ] Initialize Hardhat project
- [ ] Install OpenZeppelin contracts
- [ ] Create `NFTMarketplace.sol` contract
- [ ] Create simple ERC721 for testing

### Phase 2: NFT Contract
- [ ] Use OpenZeppelin ERC721
- [ ] Add URI storage
- [ ] Implement mint function
- [ ] Set royalty (optional)

### Phase 3: Marketplace Contract
- [ ] Listing struct (seller, price, active)
- [ ] `listItem(tokenId, price)` - list for sale
- [ ] `buyItem(tokenId)` - purchase NFT
- [ ] `cancelListing(tokenId)` - remove from sale
- [ ] `updateListing(tokenId, newPrice)` - change price
- [ ] Track marketplace fees (e.g., 2.5%)

### Phase 4: Security & Events
- [ ] Use ReentrancyGuard
- [ ] Checks-Effects-Interactions pattern
- [ ] ItemListed event
- [ ] ItemSold event
- [ ] ItemCanceled event
- [ ] Owner withdrawal function

### Phase 5: Testing
- [ ] Test NFT minting
- [ ] Test listing creation
- [ ] Test successful purchase
- [ ] Test insufficient payment
- [ ] Test only owner can cancel
- [ ] Test reentrancy protection
- [ ] Test fee calculation
- [ ] Test edge cases

### Phase 6: Deployment
- [ ] Write deployment script
- [ ] Deploy to Sepolia
- [ ] Verify contracts
- [ ] Test on testnet

---

## 💡 Hints

**Hint 1: Project Structure**
```
contracts/
  ├── NFTMarketplace.sol
  └── SimpleNFT.sol
test/
  └── NFTMarketplace.test.js
scripts/
  └── deploy.js
hardhat.config.js
```

**Hint 2: Listing Struct**
```solidity
struct Listing {
    address seller;
    uint256 price;
    bool active;
}

mapping(address => mapping(uint256 => Listing)) public listings;
```

**Hint 3: List Item Function**
```solidity
function listItem(address nftAddress, uint256 tokenId, uint256 price) external {
    require(price > 0, "Price must be > 0");
    
    IERC721 nft = IERC721(nftAddress);
    require(nft.ownerOf(tokenId) == msg.sender, "Not owner");
    require(nft.getApproved(tokenId) == address(this), "Not approved");
    
    listings[nftAddress][tokenId] = Listing(msg.sender, price, true);
    emit ItemListed(nftAddress, tokenId, msg.sender, price);
}
```

**Hint 4: Buy Item Function**
```solidity
function buyItem(address nftAddress, uint256 tokenId) external payable nonReentrant {
    Listing memory listing = listings[nftAddress][tokenId];
    require(listing.active, "Not for sale");
    require(msg.value >= listing.price, "Insufficient payment");
    
    // Calculate fees
    uint256 fee = (listing.price * marketplaceFee) / 10000; // 250 = 2.5%
    uint256 sellerProceeds = listing.price - fee;
    
    // Update state BEFORE external calls
    listings[nftAddress][tokenId].active = false;
    
    // Transfer NFT
    IERC721(nftAddress).safeTransferFrom(listing.seller, msg.sender, tokenId);
    
    // Send ETH
    payable(listing.seller).transfer(sellerProceeds);
    
    emit ItemSold(nftAddress, tokenId, msg.sender, listing.price);
}
```

**Hint 5: Testing Template**
```javascript
const { expect } = require("chai");
const { ethers } = require("hardhat");

describe("NFTMarketplace", function() {
  let marketplace, nft, owner, seller, buyer;
  
  beforeEach(async function() {
    [owner, seller, buyer] = await ethers.getSigners();
    
    // Deploy contracts
    const NFT = await ethers.getContractFactory("SimpleNFT");
    nft = await NFT.deploy();
    
    const Marketplace = await ethers.getContractFactory("NFTMarketplace");
    marketplace = await Marketplace.deploy(250); // 2.5% fee
    
    // Mint NFT to seller
    await nft.mint(seller.address);
  });
  
  it("Should list an item", async function() {
    // Test logic here
  });
});
```

---

## 🎨 Bonus Challenges

1. **Offer System**: Buyers can make offers below listing price
2. **Auction**: Time-based auctions with bidding
3. **Batch Listing**: List multiple NFTs at once
4. **Royalties**: Implement EIP-2981 royalty standard
5. **IPFS Integration**: Store metadata on IPFS
6. **Gas Optimization**: Use gas reporter, optimize storage

---

## 📚 Reference Links

- **OpenZeppelin ERC721:** https://docs.openzeppelin.com/contracts/5.x/erc721
- **Hardhat Tutorial:** https://hardhat.org/tutorial
- **Hardhat Testing:** https://hardhat.org/hardhat-runner/docs/guides/test-contracts
- **OpenSea Marketplace Contracts:** https://github.com/ProjectOpenSea/seaport

---

## ✨ Success Criteria

- ✅ Complete marketplace functionality
- ✅ All tests passing (100% coverage goal)
- ✅ ReentrancyGuard implemented
- ✅ Events emitted correctly
- ✅ Fee calculation accurate
- ✅ Deployed to testnet
- ✅ Gas optimized

---

*Project for Blockchain Learning Series - Chapter 05*