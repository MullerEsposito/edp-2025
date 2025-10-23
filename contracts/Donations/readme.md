# 💝 Donations Smart Contract

---

## 📖 Description

**Donations** is a smart contract written in **Solidity**, developed for **educational purposes** to demonstrate how decentralized donations work on **Ethereum**.  
It allows **users to send donations in Ether (ETH)** and ensures that only an **authorized beneficiary** can **withdraw** the donated funds.

> ⚠️ **Warning:** This contract **has not been audited** and **should not be used in production**.  
> It is part of an educational study developed within **77 Innovation Labs** by **i3arba**.

---

## 🧱 Contract Structure

### Variables

| Variable | Type | Description |
|-----------|------|-------------|
| `i_beneficiario` | `address` | Immutable address of the beneficiary who can withdraw the donations. |
| `s_doacoes` | `mapping(address => uint256)` | Stores the total ETH donated by each user. |

---

### Events

| Event | Description |
|--------|-------------|
| `Donations_DoacaoRecebida(address doador, uint256 valor)` | Emitted when a new donation is received. |
| `Donations_SaqueRealizado(address recebedor, uint256 valor)` | Emitted when the beneficiary performs a withdrawal. |

---

### Errors

| Error | Description |
|--------|-------------|
| `Donations_TrasacaoFalhou(bytes erro)` | Triggered when an ETH transfer fails. |
| `Donations_SacadorNaoPermitido(address chamador, address beneficiario)` | Triggered when someone other than the **beneficiary** attempts to withdraw funds. |

---

## ⚙️ Main Functions

| Function | Type | Description |
|-----------|------|-------------|
| `constructor(address _beneficiario)` | Initializer | Defines the immutable address of the beneficiary who will be allowed to withdraw donations. |
| `receive()` | `external payable` | Allows the contract to receive ETH directly. |
| `fallback()` | `external` | Empty fallback function for compatibility with unknown calls. |
| `doe()` | `external payable` | Allows any user to send a donation in ETH. The amount is added to the donor’s total. |
| `saque(uint256 _valor)` | `external` | Allows the **beneficiary** to withdraw a specific amount. Only the beneficiary set in the constructor can call it. |
| `_transferirEth(uint256 _valor)` | `private` | Executes the ETH transfer and reverts if it fails. |

---

## 🔁 How It Works

1. **Deployment**
   - The contract is deployed by passing the **beneficiary’s address** in the constructor.

2. **Donation**
   - Any user can call `doe()` and send ETH.
   - The donated amount is stored in `s_doacoes[msg.sender]`.
   - The event `Donations_DoacaoRecebida` is emitted.

3. **Withdrawal**
   - Only the **beneficiary** can call `saque(_valor)`.
   - The contract transfers the specified `_valor` (in ETH) to the beneficiary.
   - If the transaction fails, the `Donations_TrasacaoFalhou` error is triggered.

---

## 🧪 Example Usage in Remix IDE

1. Go to [Remix IDE](https://remix.ethereum.org/).
2. Create a new file named `Donations.sol` and paste the contract code.
3. Compile using **Solidity Compiler 0.8.26**.
4. Go to **Deploy & Run Transactions**:
   - Enter the beneficiary address in the deploy field.
   - Click **Deploy**.
5. To donate:
   - Enter an ETH amount in the “Value” field (e.g., `0.05`).
   - Click `doe()`.
6. To withdraw:
   - Connect with the beneficiary’s wallet address.
   - Call `saque(_valor)` with the desired value in wei.

---

## 🔗 Deployment Example

> Contract deployed on the **Sepolia Testnet** (example):  
> [https://sepolia.etherscan.io/address/0xe0ce07806b6d4c67f74333256e59338f8ca73359](https://sepolia.etherscan.io/address/0xe0ce07806b6d4c67f74333256e59338f8ca73359)

---

## 👨‍💻 Author

**Müller Esposito**  
📧 Contact: [@mulleresposito](https://github.com/mulleresposito)

---

## ⚖️ License

This project is licensed under the **MIT License**.  
See the SPDX identifier at the top of the contract or read more here: [MIT License](https://opensource.org/licenses/MIT)
