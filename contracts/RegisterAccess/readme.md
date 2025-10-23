# 🔐 RegisterAccess Smart Contract

---

## 📖 Description

**RegisterAccess** is a Solidity smart contract designed to **store and manage text records** (`string[]`) with **access control**.  
It introduces two main permission levels:
- The **owner**, who manages user access (allowlist);
- The **authorized members**, who can add or edit stored information.

This project demonstrates key Solidity concepts such as:
- Arrays and data storage  
- Mappings and access control  
- Events and modifiers  
- Role-based permissions (owner vs allowlisted users)

---

## 🧱 Contract Structure

### State Variables

| Variable | Type | Description |
|-----------|------|-------------|
| `info` | `string[]` | Dynamic array that stores the list of text records. |
| `owner` | `address` | Address of the contract owner (creator). |
| `allowlist` | `mapping(address => bool)` | Access control list of addresses allowed to modify data. |

---

### Events

| Event | Description |
|--------|-------------|
| `InfoChange(string oldInfo, string newInfo)` | Emitted whenever an existing record is modified by an authorized user. |

---

### Modifiers

| Modifier | Description |
|-----------|-------------|
| `onlyOwner` | Restricts execution to the **owner** only. Used for managing the allowlist. |
| `onlyAllowlist` | Restricts execution to users authorized in the `allowlist` mapping. |

---

## ⚙️ Main Functions

| Function | Visibility | Description |
|-----------|-------------|-------------|
| `constructor()` | Public | Sets the deployer as the `owner` and automatically adds them to the `allowlist`. |
| `getInfo(uint index)` | `public view` | Returns the information stored at a specific array index. |
| `setInfo(uint index, string memory _info)` | `public onlyAllowlist` | Updates an existing entry and emits an `InfoChange` event. |
| `addInfo(string memory _info)` | `public onlyAllowlist` | Adds a new text entry to the array and returns its index. |
| `listInfo()` | `public view` | Returns the full list of stored strings. |
| `addMember(address _member)` | `public onlyOwner` | Adds an address to the allowlist (granting write access). |
| `delMember(address _member)` | `public onlyOwner` | Removes an address from the allowlist (revoking write access). |

---

## 🔐 Access Control

| Role | Permissions |
|-------|-------------|
| **Owner** | Can add or remove allowlisted members. Can also add/edit info. |
| **Allowlisted Members** | Can add or update text entries. |
| **Public** | Can only read data (`getInfo` or `listInfo`). |

---

## 🔁 How It Works

1. **Deployment**
   - The deployer automatically becomes the **owner** and is added to the **allowlist**.

2. **Adding Information**
   - Authorized users (allowlisted) can call `addInfo("text")` to store new data.

3. **Editing Information**
   - Authorized users can modify existing entries using `setInfo(index, "new text")`.  
   - Each edit emits the `InfoChange` event.

4. **Managing Access**
   - The owner can grant or revoke permissions using:
     - `addMember(address user)` → adds to allowlist
     - `delMember(address user)` → removes from allowlist

5. **Reading Data**
   - Any user can call:
     - `getInfo(index)` → to read a specific entry
     - `listInfo()` → to get all entries

---

## 🧪 Example Usage in Remix IDE

1. Go to [Remix IDE](https://remix.ethereum.org/).  
2. Create a new file named `RegisterAccess.sol` and paste the contract code.  
3. Compile it using **Solidity Compiler 0.8.19**.  
4. Deploy the contract using any account — the deployer becomes the **owner**.  
5. To interact:
   - Use `addInfo("Hello World")` → adds new info.  
   - Use `listInfo()` → view all stored info.  
   - As owner, call `addMember(address)` to grant access to another account.  
   - Switch to the new account and test adding or editing entries.

---

## 🔗 Example Deployment

> Contract deployed on **Sepolia Testnet** (example):  
> [https://sepolia.etherscan.io/address/0x1Af944Da8e3CA52Ae5539A779fB76bAFcA4dBe5a](https://sepolia.etherscan.io/address/0x1Af944Da8e3CA52Ae5539A779fB76bAFcA4dBe5a)


---

## 👨‍💻 Author

**Müller Esposito**  
📧 Contact: [@mulleresposito](https://github.com/mulleresposito)

---

## ⚖️ License

This project is licensed under the **MIT License**.  
See the SPDX identifier at the top of the contract or read more here: [MIT License](https://opensource.org/licenses/MIT)
