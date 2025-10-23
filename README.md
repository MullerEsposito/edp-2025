# ⚡ Ethereum Developer Pack (EDP) — Bootcamp Projects

---

## 📖 Overview

This repository contains all the projects developed during the **Ethereum Developer Pack (EDP)** bootcamp — a complete learning journey focused on **blockchain development** using **Solidity**, **smart contracts**, and **Ethereum-based tools**.

Each project within this repository represents a milestone in the learning process, covering topics such as:
- Smart contract design and security
- Decentralized application (dApp) development
- Deployment to Ethereum test networks (e.g., Sepolia, Goerli)
- Usage of Remix IDE, Hardhat, and MetaMask
- CCIP (Cross-Chain Interoperability Protocol)

---

## 🧭 Repository Structure

Each folder in the root directory corresponds to a specific project developed during the bootcamp.

```
edp-bootcamp-projects/
│
├── Register/
├── RegisterAccess/
├── Token/
├── TokenShop/
├── StableCoin/
├── MyNFT/
├── CrossChainPriceNFT/
├── CrossDestinationMinter/
├── CrossSourceMinter/
└── README.md # This file
```

---

## 🧱 Project Summaries

| Project | Description | Key Topics |
|----------|--------------|-------------|
| **Register** | Simple smart contract to register an information on-chain | storage |
| **RegisterAccess** | Simple smart contract to register an information on-chain with control access | access control |
| **Token** | Smart contract token ERC20 | Fungible Token, Transfer Logic |
| **TokenShop** | Smart contract to shop the ERC20 tokens | Oracle |
| **StableCoin** | Smart contract that create an USD stable coin  | Oracle, Colateral, Liquidation |
| **MyNFT** | Smart contract NFT | Non Fungible Token, Minting |
| **CrossChainPriceNFT** | A dynamic NFT smart contract that generate SVG images based on the last price of the Bitcoin | CCIP |
| **CrossDestinationMinter** | A CCIP receptor that listen for calls of another chains and forward these calls for the CrossChainPriceNFT | CCIP |
| **CrossSourceMinter** | A CCIP sender that transmits calls to a CrossChainPriceNFT CCIP receptor | CCIP |


---

## 🧰 Technologies Used

| Technology | Purpose |
|-------------|----------|
| **Solidity** | Smart contract programming |
| **Remix IDE** | Writing and testing contracts directly in the browser |
| **Hardhat** | Local blockchain simulation and contract deployment |
| **MetaMask** | Wallet for blockchain interactions |
| **Ethers.js / Web3.js** | Libraries for interacting with Ethereum |
| **Sepolia Testnet** | Ethereum test network for deployment and verification |

---

## 🚀 Getting Started

To clone this repository and explore the projects locally:

```bash
# Clone the repository
git clone https://github.com/your-username/edp-bootcamp-projects.git

# Navigate into the repository
cd edp-bootcamp-projects

# Open any project folder
cd ToDoList
