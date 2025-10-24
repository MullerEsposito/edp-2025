# 💵 MullerStable – Overcollateralized Stablecoin (ETH-backed)

---

## Overview

**MullerStable (`USDMUL`)** is an overcollateralized, ETH-backed stablecoin implemented as an **ERC20** with **2 decimals**.  
Users deposit ETH to mint USDMUL and can burn USDMUL to withdraw ETH. The system relies on a **Chainlink ETH/USD price feed** and includes **Chainlink Automation** hooks to liquidate under-collateralized positions.

---

## 🔧 Key Specs

- **Token Name / Symbol:** Muller Stable / **USDMUL**
- **Standard:** ERC20
- **Decimals:** **2** (e.g., `100` = $100.00)
- **Collateral:** ETH
- **Collateral Ratio:** **150%** (min)
- **Oracle:** Chainlink **ETH/USD** (8 decimals)
- **Network (default in constructor):** Ethereum **Sepolia**  
  Price Feed: `0x694AA1769357215DE4FAC081bf1f309aDC325306`
- **Liquidation Address:** set to **deployer** at construction

> ℹ️ This contract stores each user’s **collateral ETH** and **stablecoin debt** in `positions`.

---

## 🧱 How It Works

1. **Deposit & Mint**
   - Send ETH via `depositAndMint()` to mint USDMUL.
   - Minted amount ≈ value(ETH in USD) / **1.5** (i.e., 150% collateralization), rounded with token’s 2 decimals.

2. **Burn & Withdraw**
   - Call `burnAndWithdraw(burnAmount)` to burn USDMUL and receive ETH.
   - ETH returned is computed so that the position remains consistent with the **150%** CR assumptions.

3. **Liquidation (Automation)**
   - `checkUpkeep` scans users; if a user’s **collateral ratio** falls **below 150%**, `performUpkeep` can liquidate:
     - Burns all user debt.
     - **Transfers all collateral ETH** to `liquidationAddress`.
     - Resets the user’s position.

---

## 🗂 Data Structures

```solidity
struct Position {
    uint256 collateralETH;   // in wei
    uint256 stablecoinDebt;  // USDMUL amount, 2 decimals (e.g., $100 => 100)
}
mapping(address => Position) public positions;
address[] public users;
```

---

## 📦 Installation

```bash
npm install --save-dev hardhat
npm install @openzeppelin/contracts @chainlink/contracts
```

Compile:

```bash
npx hardhat compile
```

---

## 🚀 Deployment (Hardhat example)

```js
    const [deployer] = await ethers.getSigners();
    const MullerStable = await ethers.getContractFactory("MullerStable");
    const usdmul = await MullerStable.deploy();
    await usdmul.deployed();

    console.log("USDMUL deployed at:", usdmul.address);
```
> ⚠️ If you deploy to a network other than Sepolia, update the price feed address in the constructor.

---

## 🧾 Example Usage

```js

    // 1) Read token metadata
    console.log(await usdmul.name());      // "Muller Stable"
    console.log(await usdmul.symbol());    // "USDMUL"
    console.log(await usdmul.decimals());  // 2

    // 2) Estimate how much USDMUL you'd mint for 0.5 ETH
    const estimate = await usdmul.estimateMintAmount(
        ethers.utils.parseEther("0.5")
    );
    console.log("Estimated USDMUL:", estimate.toString());

    // 3) Deposit 0.5 ETH and mint
    await usdmul.connect(user).depositAndMint({ value: ethers.utils.parseEther("0.5") });

    // 4) Check user position
    const pos = await usdmul.positions(user.address);
    console.log("Collateral (wei):", pos.collateralETH.toString());
    console.log("Debt (USDMUL, 2 decimals):", pos.stablecoinDebt.toString());

    // 5) Estimate ETH to withdraw if burning 100.00 USDMUL
    const ethBack = await usdmul.estimateWithdrawETH(100); // 100 = $100.00
    console.log("ETH to receive:", ethBack.toString());

    // 6) Burn and withdraw
    await usdmul.connect(user).burnAndWithdraw(100);
```

---

## 📚 Public Functions

- `decimals() → uint8` – returns **2**.  
- `depositAndMint() payable` – deposits ETH and mints **USDMUL**.  
- `burnAndWithdraw(uint256 burnAmount)` – burns **USDMUL** and sends back ETH (computed via oracle & CR).  
- `estimateMintAmount(uint256 ethAmount) → uint256` – estimates **USDMUL** for a given ETH deposit.  
- `estimateWithdrawETH(uint256 burnAmount) → uint256` – estimates ETH for a given burn amount.  
- `getLatestPrice() → uint256` – returns **Chainlink ETH/USD** price (8 decimals).  
- `getUserCollateralUSD(address) → uint256` – user collateral value in USD (with token decimals).  
- `getCollateralRatio(address) → uint256` – `collateralUSD / debt` (returns a raw ratio, not a percentage).  
- `getUsers() → address[]` – returns tracked user addresses.  

### ⚙️ Automation

- `checkUpkeep(bytes) → (bool, bytes)` – flags any undercollateralized user.  
- `performUpkeep(bytes)` – liquidates a flagged user.  

---

## 🧮 Units & Decimals

- **USDMUL:** 2 decimals (e.g., `250` = $250.00).  
- **ETH/USD price:** 8 decimals (Chainlink).  
- **ETH amounts:** wei.  
- `getCollateralRatio` returns a **unitless ratio**: `collateralUSD / debtUSD`.  
- **Liquidation:** triggered if ratio `< 150` (since `COLLATERAL_RATIO = 150`).  

---

## ⚠️ Important Notes & Caveats

- **Comments vs. Names:** Some inline comments mention `MULST` / `SOLST`; the actual token symbol is **USDMUL**.  
- **Centralization Risk:** On liquidation, *all collateral* goes to `liquidationAddress` (set to deployer). Consider auctions or incentives instead.  
- **Access Control:**  
  - Anyone can call `performUpkeep` (standard for Automation-compatible contracts).  
  - Minting only via `depositAndMint` (no privileged minting function).  
- **Reentrancy / ETH Transfer:**  
  - Uses `transfer` when sending ETH. Consider switching to `call` for gas-stipend safety and adding a **reentrancy guard**.  
- **Oracle Dependence:** Ensure the price feed matches the deployed network (Mainnet, Sepolia, etc.).  
- **No Interest / Stability Fees:** No accrual of interest, fees, or redemption penalty.  
- **Slippage & Rounding:** 2-decimal token; rounding effects expected in estimates and redemptions.  

---

## 🔐 Security Considerations

- Validate **price feed addresses** for each network.  
- Consider implementing a **pausable** pattern and rate limits.  
- Add **ReentrancyGuard** and prefer `call` over `transfer` for ETH returns.  
- Review **liquidation logic** — currently all collateral goes to one address.  
- Monitor **oracle liveness** (round completeness, timestamp checks).  

---

## 🧪 Test Ideas

- Minting with various ETH amounts and verifying resulting debt.  
- Burning edge cases (zero, more than balance, exact dust).  
- Collateral ratio transitions across the **150** threshold.  
- Liquidation path correctness and event emission.  
- Oracle failure or negative price scenarios.  
- `users` array growth and position tracking accuracy.  

---

## 🧑‍💻 Author

Developed for **MULLER ESPOSITO**  
Created using Solidity, OpenZeppelin, and Chainlink.  

---

## 📜 License

This project is licensed under the **MIT License**.  
See the `LICENSE` file for details.
