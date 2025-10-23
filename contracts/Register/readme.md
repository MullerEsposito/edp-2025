# 🗂️ Register Smart Contract

---

## 📖 Description

The **Register** contract is a simple example of a **Solidity smart contract** that demonstrates how to **store** and **retrieve** information on the **Ethereum blockchain**.  

It allows any user to **save a text string** (e.g., a name, message, or identifier) and later **read** it publicly.

This project is designed for **educational purposes**, introducing the basic concepts of:
- State variables  
- Reading (`view`) and writing (`public`) functions  
- String storage in Solidity  

---

## 🧱 Contract Structure

| Element | Type | Description |
|----------|------|-------------|
| `info` | `string` | Private variable that stores the text information on-chain. |

---

### Functions

| Function | Visibility | Description |
|-----------|-------------|-------------|
| `getInfo()` | `public view` | Returns the current value of the stored text (`info`). |
| `setInfo(string memory _info)` | `public` | Updates the `info` variable with a new string value. |

---

## ⚙️ How It Works

1. **Deployment**
   - Deploy the contract to an Ethereum-compatible blockchain (e.g., Remix VM, Sepolia, or Hardhat local node).
   - No constructor parameters are required.

2. **Setting Information**
   - Call `setInfo("Your text here")` to store a new string on the blockchain.
   - This transaction modifies the contract state and requires gas.

3. **Reading Information**
   - Call `getInfo()` to retrieve the last stored string.
   - This is a free, read-only operation (`view` function).

---

## 🧪 Example Usage (Remix IDE)

1. Open [Remix IDE](https://remix.ethereum.org/).  
2. Create a new file called `Register.sol` and paste the contract code.  
3. Compile it using **Solidity Compiler 0.8.19**.  
4. Go to **Deploy & Run Transactions** and click **Deploy**.  
5. Once deployed:
   - Use `setInfo` to save a message (e.g., `"Hello Blockchain!"`).
   - Call `getInfo` to view the stored message.

---

## 🔗 Example (Optional)

> Example deployment on **Sepolia Testnet**:  
> [https://sepolia.etherscan.io/address/PASTE_CONTRACT_ADDRESS_HERE](https://sepolia.etherscan.io/)

*(Replace with your real contract address after deployment.)*

---

## 👨‍💻 Author

**Müller Esposito**  
📧 Contact: [@mulleresposito](https://github.com/mulleresposito)

---

## ⚖️ License

This project is licensed under the **MIT License**.  
See the SPDX identifier at the top of the contract or read more here: [MIT License](https://opensource.org/licenses/MIT)
