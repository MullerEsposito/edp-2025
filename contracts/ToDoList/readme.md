# 📝 ToDoList Smart Contract

## 📖 Description

**ToDoList** is a smart contract developed in **Solidity** to manage tasks directly on the blockchain.  
It allows users to **add**, **list**, and **remove** tasks in a decentralized and transparent way.  
Each task contains a **description** and a **timestamp** (creation time), permanently stored on-chain.

This project was developed for educational purposes and demonstrates the use of structs, dynamic arrays, and events in Solidity.

---

## 🧱 Contract Structure

### Main Functions

| Function | Description |
|-----------|--------------|
| `setTarefa(string _descricao)` | Adds a new task to the list. |
| `deletarTarefa(string _descricao)` | Removes an existing task by its description. |
| `getTarefa()` | Returns all tasks stored in the contract. |

### Events

| Event | Description |
|--------|--------------|
| `ToDoList_TarefaAdiciona(Tarefa tarefa)` | Emitted when a new task is added. |
| `ToDoList_TarefaCompletadaERemovida(string _descricao)` | Emitted when a task is completed and removed. |

---

## ⚙️ How to Clone and Run

### 🔧 Prerequisites

- [Node.js](https://nodejs.org/)
- [Git](https://git-scm.com/)
- [Remix IDE](https://remix.ethereum.org/) or [Hardhat](https://hardhat.org/)
- MetaMask extension configured for your desired test network (optional)

### 💻 Cloning the Project

```bash
# Clone this repository
git clone https://github.com/your-username/todolist-solidity.git

# Enter the directory
cd todolist-solidity
```

### ▶️ Running on Remix IDE

1. Go to: [https://remix.ethereum.org/](https://remix.ethereum.org/)
2. Click **File Explorer → Upload Files**
3. Upload the `ToDoList.sol` file  
4. Go to **Solidity Compiler**:
   - Select version **0.8.26**
   - Click **Compile ToDoList.sol**
5. Go to **Deploy & Run Transactions**:
   - Choose your preferred environment (`Remix VM`, `Injected Provider` with MetaMask, or `Sepolia Testnet`)
   - Click **Deploy**
6. After deployment, interact with the contract:
   - Add tasks using `setTarefa`
   - List tasks using `getTarefa`
   - Delete tasks using `deletarTarefa`

---

### 🔗 Contract Access on Block Explorer

Contract deployed on: [**Etherscan / Sepolia Testnet**](https://sepolia.etherscan.io/address/0x082eee9ef1fbe5cf480335083a6b99cd9a2267b3)

---

### 👨‍💻 Author

**Müller Esposito**  
📧 Contact: [@mulleresposito](https://github.com/mulleresposito)

---

### ⚖️ License

This project is licensed under the **MIT License**.  
Refer to the SPDX header in the contract or read more here: [MIT License](https://opensource.org/licenses/MIT)