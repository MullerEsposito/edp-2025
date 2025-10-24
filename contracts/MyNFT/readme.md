# 🖼️ MyNFT – ERC721 Smart Contract

---

## Overview

**MyNFT** is an ERC721-based Non-Fungible Token (NFT) smart contract built using **OpenZeppelin** libraries.  
It allows users to **mint unique NFTs**, each linked to its own **metadata URI** stored on IPFS (or any other URI-based metadata service).  
The contract also supports updating the metadata of existing NFTs, enabling dynamic or upgradable NFTs.

---

## 🧱 Features

- **ERC721-compliant:** Built on the standard ERC721 token interface.  
- **Metadata Storage:** Uses `ERC721URIStorage` to associate metadata URIs with tokens.  
- **Minting Function:** Secure minting of NFTs with automatic token ID tracking.  
- **Metadata Update:** Ability to change a token’s metadata URI after minting.  
- **OpenZeppelin Security:** Uses audited, battle-tested OpenZeppelin contracts.  

---

## 🧩 Contract Details

- **Name:** MULLER NFT EDP Online 2025  
- **Symbol:** MNEDP  
- **Solidity Version:** `0.8.19`  
- **License:** MIT  
- **Dependencies:**  
  - `@openzeppelin/contracts@4.6.0/token/ERC721/extensions/ERC721URIStorage.sol`  
  - `@openzeppelin/contracts@4.6.0/utils/Counters.sol`

---

## ⚙️ Functions

### `safeMint(address to, string memory metadataUri)`
Mints a new NFT and assigns it to the specified address.  
- **Parameters:**
  - `to`: The recipient address.
  - `metadataUri`: The metadata URI (typically an IPFS link).  
- **Behavior:**
  - Increments the token counter.
  - Mints a new token.
  - Stores and sets the metadata URI.  

---

### `updateTokenURI(uint256 tokenId, string memory metadataUri)`
Updates the metadata URI of an existing token.  
- **Parameters:**
  - `tokenId`: The ID of the NFT.
  - `metadataUri`: The new metadata URI.  

⚠️ *Note:* This function does not include access control, meaning anyone could call it.  
It's recommended to add access restrictions (e.g., only the owner or contract admin).

---

### `tokenURI(uint256 tokenId) → string`
Returns the metadata URI associated with the given token ID.

---

### `_burn(uint256 tokenId)`
Internal function to destroy a token and clear its metadata.

---

## 📦 Installation & Deployment

1. **Install Dependencies**
```bash
   npm install @openzeppelin/contracts@4.6.0
```

2. **Compile the Contract**
```bash
    npx hardhat compile
```

3. **Deploy the Contract**
```js
    const MyNFT = await ethers.getContractFactory("MyNFT");
    const myNFT = await MyNFT.deploy();
    console.log("MyNFT deployed to:", myNFT.address);
```

## 🧾 Example Usage

```js
// Mint a new NFT
await myNFT.safeMint("0xRecipientAddress", "ipfs://QmExampleMetadataHash");

// Update token metadata
await myNFT.updateTokenURI(0, "ipfs://QmUpdatedMetadataHash");

// Retrieve metadata URI
const uri = await myNFT.tokenURI(0);
console.log(uri);
```

## 🔐 Security Considerations

- The `updateTokenURI` function is **public** and **unrestricted**.  
  Add appropriate access control (e.g., `onlyOwner`) if needed.
- Always validate and sanitize metadata URIs before use.
- Ensure IPFS hashes or URLs are correct to prevent metadata loss.

---

## 🧑‍💻 Author

Developed for **MULLER ESPOSITO**  
Created using Solidity and OpenZeppelin.

---

## 📜 License

This project is licensed under the **MIT License**.  
See the `LICENSE` file for details.

