# 💰 Token MULLER - 2025 (MUL)

---

## 📖 Description

**Token MULLER - 2025 (MUL)** is an ERC-20–compatible smart contract developed in **Solidity** using **OpenZeppelin** libraries.  
It implements **role-based access control** (`AccessControl`) to securely manage minting permissions and sets a **custom decimal precision of 2**.

This contract is designed for **educational and demonstration purposes**, showing how to:
- Deploy an ERC-20 token;
- Assign administrative and minting roles;
- Control who can mint new tokens;
- Customize the token’s decimal format.

---

## 🧱 Contract Overview

| Element | Type | Description |
|----------|------|-------------|
| **Standard** | [ERC-20](https://docs.openzeppelin.com/contracts/4.x/api/token/erc20) | Fungible token standard used by Ethereum and EVM chains. |
| **Access Control** | [AccessControl](https://docs.openzeppelin.com/contracts/4.x/access-control) | Role-based permission system for minting and administration. |
| **Token Name** | `TOKEN MULLER - 2025` | The full name of the token. |
| **Symbol** | `MUL` | The ticker symbol. |
| **Decimals** | `2` | Two decimal places (1 MUL = 100 base units). |

---

## ⚙️ Main Features

| Feature | Description |
|----------|-------------|
| **Mintable** | New tokens can be minted by addresses with the `MINTER_ROLE`. |
| **Role Management** | Roles are controlled through the `AccessControl` module. |
| **Fixed Decimals** | Overrides the standard 18-decimals to use 2 (like fiat currencies). |
| **Admin Control** | The deployer automatically receives both the admin and minter roles. |

---

## 🔐 Roles and Permissions

| Role | Description | Granted To |
|-------|--------------|-------------|
| `DEFAULT_ADMIN_ROLE` | Can assign or revoke roles. | Deployer (msg.sender) |
| `MINTER_ROLE` | Can mint new tokens using the `mint()` function. | Deployer (msg.sender) by default |

You can delegate minting to other addresses using the admin role:  
```solidity
grantRole(MINTER_ROLE, 0x123...abc);
```

## 🧰 Main Functions

| Function | Visibility | Description |
|-----------|-------------|-------------|
| `constructor()` | Public | Initializes the token, assigns admin and minter roles to the deployer. |
| `mint(address to, uint256 amount)` | `public onlyRole(MINTER_ROLE)` | Mints new tokens to the specified address. |
| `decimals()` | `public pure override` | Returns **2** instead of the default **18**. |

---

## 💸 Example of Usage

### 1️⃣ Deploy the Token

1. Open [Remix IDE](https://remix.ethereum.org/).  
2. Create a new file named `Token.sol` and paste the contract code.  
3. Compile with **Solidity 0.8.20**.  
4. Deploy the contract — the deployer automatically becomes the **admin** and **minter**.

---

### 2️⃣ Mint New Tokens

After deployment, in Remix:

```solidity
mint(0xYourAddress, 10000)
```

➡️ This will mint **100.00 MUL**, since the token uses **2 decimals**.

---

### 3️⃣ Check Token Info

You can call the following functions:

| Function | Returns |
|-----------|----------|
| `name()` | `TOKEN MULLER - 2025` |
| `symbol()` | `MUL` |
| `decimals()` | `2` |
| `totalSupply()` | Current total minted supply |

---

## 🔗 Example Deployment

> **Example (Sepolia Testnet):**  
> [https://sepolia.etherscan.io/address/0x3c3C4d421F06109E3030604EA6D0795A189aD341](https://sepolia.etherscan.io/address/0x3c3C4d421F06109E3030604EA6D0795A189aD341)  


---

## 👨‍💻 Author

**Müller Esposito**  
Blockchain Developer | 77 Innovation Labs  
📧 Contact: [@mulleresposito](https://github.com/mulleresposito)

---

## ⚖️ License

This project is licensed under the **MIT License**.  
See the SPDX identifier at the top of the contract or read more here: [MIT License](https://opensource.org/licenses/MIT)
