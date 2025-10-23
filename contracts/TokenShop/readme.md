# 🏪 TokenShop Smart Contract

---

## 📖 Description

**TokenShop** is a Solidity smart contract that allows users to **purchase tokens using ETH**, with the **price determined in real-time using Chainlink’s ETH/USD price feed**.  
It interacts with an external ERC-20 token contract that exposes a `mint()` function, minting new tokens for the buyer upon payment.

This project demonstrates how to:
- Integrate **Chainlink Data Feeds** to fetch the latest ETH/USD exchange rate;
- Implement a **token shop** mechanism that mints tokens dynamically based on live market prices;
- Manage ownership and withdrawal of collected ETH safely.

> ⚠️ **Note:** This contract is for **educational purposes only** and **should not be used in production** without a proper security audit.

---

## 🧱 Contract Overview

| Element | Description |
|----------|-------------|
| **Token Interaction** | Uses an external ERC-20 token contract with a `mint()` function. |
| **Price Feed** | Fetches real-time ETH/USD price using Chainlink Oracle. |
| **Payment Method** | Users send ETH directly to the contract (`receive()` function). |
| **Network** | Designed for **Ethereum Sepolia Testnet**. |
| **Ownership** | Only the contract deployer (owner) can withdraw ETH. |

---

## ⚙️ How It Works

1. **Initialization**
   - The contract deployer passes the address of the mintable token contract.
   - The contract connects to the **Chainlink ETH/USD price feed** on **Sepolia**:  
     `0x694AA1769357215DE4FAC081bf1f309aDC325306`.

2. **Buying Tokens**
   - A user sends ETH directly to the contract address.
   - The contract calculates how many tokens to mint based on:
     - The **current ETH/USD rate** from Chainlink;
     - The **token price** (default: `$2.00` per token, stored as `200` with 2 decimals).
   - Tokens are **minted to the sender’s address** via the external token contract.

3. **Owner Withdrawal**
   - The contract owner can withdraw accumulated ETH using the `withdraw()` function.

---

## 🔢 Conversion Formula

| Step | Description | Formula |
|------|--------------|----------|
| 1️⃣ | Get ETH/USD price | `ethUsd = Chainlink.latestRoundData()` (8 decimals) |
| 2️⃣ | Convert ETH to USD | `amountUSD = amountETH * ethUsd / 10**18` |
| 3️⃣ | Convert USD to Token Amount | `amountToken = amountUSD / tokenPrice / 10**(8/2)` |

🧮 **Example:**  
If 1 ETH = $3,000 and token price = $2.00,  
sending 0.01 ETH (~$30) gives ≈ 15 tokens.

---

## 🧰 Main Functions

| Function | Visibility | Description |
|-----------|-------------|-------------|
| `constructor(address tokenAddress)` | Public | Initializes the token shop, sets up Chainlink price feed and assigns owner. |
| `getChainlinkDataFeedLatestAnswer()` | `public view` | Returns the latest ETH/USD price (8 decimal places). |
| `tokenAmount(uint256 amountETH)` | `public view` | Calculates how many tokens correspond to the given ETH amount. |
| `receive()` | `external payable` | Automatically called when ETH is sent; mints tokens to the sender. |
| `withdraw()` | `external onlyOwner` | Allows the owner to withdraw accumulated ETH. |

---

## 🧪 Example Usage (Remix IDE)

1. Deploy your **mintable ERC-20 token** (e.g., `Token.sol` from the previous example).
2. Copy the deployed token’s address.
3. Deploy `TokenShop` passing the token address as a constructor parameter.
4. Fund your wallet with **Sepolia ETH**.
5. Send ETH directly to the TokenShop contract address — you’ll automatically receive minted tokens.

---

## 🔗 Network Details

| Parameter | Value |
|------------|--------|
| **Network** | Ethereum Sepolia Testnet |
| **Chainlink Price Feed (ETH/USD)** | `0x694AA1769357215DE4FAC081bf1f309aDC325306` |
| **Token Price** | 200 (represents `$2.00` with 2 decimals) |

---

## 🪙 Example Workflow

```solidity
// Assume the token contract has address 0x123...abc
// Deploy TokenShop with:
constructor(0x123...abc)

// Send 0.01 ETH directly to TokenShop
// Contract calls mint(msg.sender, calculatedAmount)

// Owner withdraws collected ETH later:
withdraw()
```

## 🧑‍💼 Ownership & Security

| Function | Access | Description |
|-----------|---------|-------------|
| `withdraw()` | Owner only | Transfers all ETH from the contract to the owner’s address. |
| `onlyOwner` modifier | Internal | Restricts functions to the contract deployer. |

---

## 🔗 Example Deployment

> **Example (Sepolia Testnet):**  
> [https://sepolia.etherscan.io/address/0x1C74CD2a48023d2814A0c8fA8340e48Ce81f9E0F](https://sepolia.etherscan.io/address/0x1C74CD2a48023d2814A0c8fA8340e48Ce81f9E0F)  


---

## 👨‍💻 Author

**Müller Esposito**  
Blockchain Developer | 77 Innovation Labs  
📧 Contact: [@mulleresposito](https://github.com/mulleresposito)

---

## ⚖️ License

This project is licensed under the **MIT License**.  
See the SPDX identifier at the top of the contract or read more here: [MIT License](https://opensource.org/licenses/MIT)
