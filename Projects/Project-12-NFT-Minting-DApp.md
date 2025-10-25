# Project 12: NFT Minting DApp

**Chapter:** 06 - Full-Stack DApps
**Difficulty:** Intermediate
**Estimated Time:** 8-10 hours

---

## 📋 Project Description

Create a beautiful NFT minting website where users can connect their wallet, mint NFTs with custom metadata, and view their collection. Includes image upload to IPFS.

---

## 🎯 Learning Objectives

- Build NFT minting UI
- Upload files to IPFS (Pinata/NFT.Storage)
- Handle minting transactions
- Display NFT gallery
- Work with OpenSea metadata standards

---

## 🛠️ Technology Stack

- Next.js 14+ with TypeScript
- RainbowKit + Wagmi + Viem
- Tailwind CSS
- Pinata (IPFS)
- OpenZeppelin ERC721URIStorage
- Hardhat

---

## ✅ TODO List

### Phase 1: NFT Smart Contract
- [ ] Use OpenZeppelin ERC721URIStorage
- [ ] Add minting function with URI
- [ ] Set max supply (e.g., 10,000)
- [ ] Set mint price (e.g., 0.01 ETH)
- [ ] Owner withdrawal function
- [ ] Deploy to Sepolia

### Phase 2: IPFS Setup
- [ ] Create Pinata account (free tier)
- [ ] Get API keys
- [ ] Function to upload image to IPFS
- [ ] Function to upload metadata JSON
- [ ] Return IPFS hash (CID)

### Phase 3: Next.js Frontend
- [ ] Setup RainbowKit providers
- [ ] Create minting page
- [ ] File upload component
- [ ] Preview uploaded image
- [ ] NFT name and description inputs
- [ ] Mint button with transaction handling

### Phase 4: Minting Flow
- [ ] User uploads image
- [ ] Upload to IPFS → get image URI
- [ ] Create metadata JSON (name, description, image)
- [ ] Upload metadata to IPFS → get token URI
- [ ] Call mint function with token URI
- [ ] Show transaction status
- [ ] Display minted NFT

### Phase 5: NFT Gallery
- [ ] Fetch user's NFTs using contract
- [ ] Display tokenIds owned by user
- [ ] Fetch metadata for each token
- [ ] Show image, name, description
- [ ] Link to OpenSea testnet

### Phase 6: Polish
- [ ] Loading states
- [ ] Error handling
- [ ] Transaction confirmations
- [ ] Mint counter (X/10,000 minted)
- [ ] Responsive grid layout
- [ ] Animations and transitions

---

## 💡 Hints

**Hint 1: NFT Contract**
```solidity
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract MyNFT is ERC721URIStorage, Ownable {
    uint256 private _tokenIdCounter;
    uint256 public constant MAX_SUPPLY = 10000;
    uint256 public constant MINT_PRICE = 0.01 ether;

    constructor() ERC721("MyNFT", "MNFT") Ownable(msg.sender) {}

    function mint(string memory uri) public payable {
        require(_tokenIdCounter < MAX_SUPPLY, "Max supply reached");
        require(msg.value >= MINT_PRICE, "Insufficient payment");
        
        uint256 tokenId = _tokenIdCounter;
        _tokenIdCounter++;
        
        _safeMint(msg.sender, tokenId);
        _setTokenURI(tokenId, uri);
    }

    function withdraw() public onlyOwner {
        payable(owner()).transfer(address(this).balance);
    }
}
```

**Hint 2: Upload to Pinata**
```typescript
async function uploadToPinata(file: File) {
  const formData = new FormData();
  formData.append('file', file);

  const response = await fetch('https://api.pinata.cloud/pinning/pinFileToIPFS', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${process.env.NEXT_PUBLIC_PINATA_JWT}`,
    },
    body: formData,
  });

  const data = await response.json();
  return `ipfs://${data.IpfsHash}`;
}
```

**Hint 3: Metadata JSON**
```json
{
  "name": "Cool NFT #1",
  "description": "This is my awesome NFT",
  "image": "ipfs://QmXyz123...",
  "attributes": [
    {
      "trait_type": "Color",
      "value": "Blue"
    },
    {
      "trait_type": "Rarity",
      "value": "Rare"
    }
  ]
}
```

**Hint 4: Mint Component**
```typescript
'use client';
import { useWriteContract, useWaitForTransactionReceipt } from 'wagmi';
import { parseEther } from 'viem';

export function MintNFT() {
  const [imageFile, setImageFile] = useState<File | null>(null);
  const [name, setName] = useState('');
  const [description, setDescription] = useState('');

  const { writeContract, data: hash } = useWriteContract();
  const { isLoading, isSuccess } = useWaitForTransactionReceipt({ hash });

  const handleMint = async () => {
    // 1. Upload image to IPFS
    const imageUri = await uploadToPinata(imageFile);
    
    // 2. Create metadata
    const metadata = { name, description, image: imageUri };
    const metadataUri = await uploadJSONToPinata(metadata);
    
    // 3. Mint NFT
    writeContract({
      address: NFT_CONTRACT_ADDRESS,
      abi: nftABI,
      functionName: 'mint',
      args: [metadataUri],
      value: parseEther('0.01'),
    });
  };

  return (
    <div>
      <input type="file" onChange={(e) => setImageFile(e.target.files?.[0])} />
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <textarea value={description} onChange={(e) => setDescription(e.target.value)} />
      <button onClick={handleMint} disabled={isLoading}>
        {isLoading ? 'Minting...' : 'Mint NFT'}
      </button>
    </div>
  );
}
```

**Hint 5: Fetch User's NFTs**
```typescript
import { useAccount, useReadContract } from 'wagmi';

function MyNFTs() {
  const { address } = useAccount();
  
  const { data: balance } = useReadContract({
    address: NFT_CONTRACT_ADDRESS,
    abi: nftABI,
    functionName: 'balanceOf',
    args: [address],
  });

  const { data: tokenId } = useReadContract({
    address: NFT_CONTRACT_ADDRESS,
    abi: nftABI,
    functionName: 'tokenOfOwnerByIndex',
    args: [address, 0], // First NFT
  });

  const { data: tokenURI } = useReadContract({
    address: NFT_CONTRACT_ADDRESS,
    abi: nftABI,
    functionName: 'tokenURI',
    args: [tokenId],
  });

  // Fetch metadata from IPFS
  // Display NFT
}
```

---

## 🎨 Bonus Challenges

1. **Whitelist**: Only whitelisted addresses can mint
2. **Reveal Mechanism**: Hide metadata until reveal date
3. **Batch Minting**: Mint multiple NFTs at once
4. **Royalties**: Implement EIP-2981 for marketplace royalties
5. **Generative Art**: Generate random traits on-chain
6. **Staking**: Stake NFTs to earn tokens

---

## 📚 Reference Links

- **Pinata Docs:** https://docs.pinata.cloud/
- **NFT.Storage:** https://nft.storage/docs/
- **OpenSea Metadata:** https://docs.opensea.io/docs/metadata-standards
- **OpenZeppelin ERC721:** https://docs.openzeppelin.com/contracts/5.x/erc721

---

## ✨ Success Criteria

- ✅ Image upload to IPFS works
- ✅ Metadata correctly formatted
- ✅ Minting function executes
- ✅ NFTs visible on OpenSea testnet
- ✅ Gallery displays user's NFTs
- ✅ Beautiful, responsive UI
- ✅ Live deployment

---

*Project for Blockchain Learning Series - Chapter 06*