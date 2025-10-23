# 🌤️ WeatherFunctionsArbitrum Smart Contract

---

## 📖 Description

**WeatherFunctionsArbitrum** is a smart contract built in **Solidity** that uses **Chainlink Functions** to retrieve **real-time weather information** from an external API and store it on-chain.  
It demonstrates how to make **off-chain HTTP requests** via **JavaScript code executed by the Chainlink DON (Decentralized Oracle Network)** and integrate the results directly into a smart contract deployed on **Arbitrum Sepolia**.

This project is designed for **educational and experimental purposes**, illustrating how to:
- Use **Chainlink Functions** to fetch external data (off-chain computation),
- Manage asynchronous requests and responses,
- Handle mappings and structs for data persistence.

> ⚠️ **Warning:** This code uses **hardcoded configuration values** and is **not audited**.  
> It is **not intended for production use**.

---

## 🧱 Contract Overview

| Element | Type | Description |
|----------|------|-------------|
| **Base contract** | `FunctionsClient` | Provides Chainlink Functions communication utilities. |
| **Library** | `FunctionsRequest` | Helps construct and encode Function requests. |
| **Network** | Arbitrum Sepolia | Testnet used for deployment. |
| **External data source** | [wttr.in](https://wttr.in/) | Free weather API providing current temperature per city. |

---

## ⚙️ How It Works

1. **User requests weather data**
   - A user calls `getTemperature("cityName")`.
   - The contract builds a **Chainlink Functions request** containing a JavaScript snippet that queries `https://wttr.in/${city}?format=3&m`.

2. **Chainlink Functions executes the request**
   - The request is sent to the **Chainlink DON** (`donID`) configured for **Arbitrum Sepolia**.
   - The oracle network runs the JavaScript code, fetches the weather data, and returns the result to the contract.

3. **Contract receives the result**
   - The `fulfillRequest()` callback is triggered automatically.
   - The temperature string returned from the API is stored in the blockchain (in the `cities` array).
   - The result is emitted via the `Response` event.

4. **Data storage**
   - Each request is associated with a `CityStruct` containing:
     - Sender’s address
     - City name
     - Latest temperature string
     - Timestamp of last update

---

## 🧩 Key Components

### State Variables

| Variable | Type | Description |
|-----------|------|-------------|
| `router` | `address` | Chainlink Functions router for Arbitrum Sepolia. |
| `donID` | `bytes32` | DON identifier for Arbitrum Sepolia. |
| `subscriptionId` | `uint64` | Chainlink Functions subscription ID (funded with LINK). |
| `gasLimit` | `uint32` | Callback gas limit for request fulfillment. |
| `source` | `string` | Inline JavaScript source code executed off-chain. |
| `cities` | `CityStruct[]` | Array storing all requested cities and their weather data. |
| `requests` | `mapping(bytes32 => RequestStatus)` | Stores metadata for each request (response, error, fulfilled status). |

---

### Structs

#### `CityStruct`
| Field | Type | Description |
|--------|------|-------------|
| `sender` | `address` | User who requested the data. |
| `timestamp` | `uint` | Time when the temperature was last updated. |
| `name` | `string` | City name. |
| `temperature` | `string` | Retrieved temperature string. |

#### `RequestStatus`
| Field | Type | Description |
|--------|------|-------------|
| `exists` | `bool` | Indicates if the request exists. |
| `fulfilled` | `bool` | True when the request is completed. |
| `response` | `bytes` | Encoded response returned from Chainlink Functions. |
| `err` | `bytes` | Error message if the request failed. |

---

### Events

| Event | Description |
|--------|-------------|
| `Response(bytes32 requestId, string temperature, bytes response, bytes err)` | Emitted when a Chainlink Functions request is fulfilled, showing the result or error. |

---

## 🧰 Main Functions

| Function | Visibility | Description |
|-----------|-------------|-------------|
| `constructor(uint64 functionsSubscriptionId)` | Public | Initializes the contract with the Chainlink Functions router and subscription ID. |
| `getTemperature(string memory _city)` | `external` | Sends a request to Chainlink Functions to fetch the current temperature for `_city`. |
| `fulfillRequest(bytes32 requestId, bytes memory response, bytes memory err)` | `internal override` | Callback function called by the Chainlink DON once the request is completed. |
| `getCity(string memory city)` | `public view` | Returns the stored data for a given city. |
| `listAllCities()` | `public view` | Returns the full list of city weather records. |
| `listCities(uint start, uint end)` | `public view` | Returns a range of city records (useful for pagination). |

---

## 🌐 JavaScript Source (Off-Chain Code)

The contract includes this inline JS snippet executed by the Chainlink Functions DON:

```javascript
const city = args[0];
const apiResponse = await Functions.makeHttpRequest({
  url: `https://wttr.in/${city}?format=3&m`,
  responseType: 'text'
});
if (apiResponse.error) {
  throw Error('Request failed');
}
const { data } = apiResponse;
return Functions.encodeString(data);
```

## 🧪 Example Usage (Remix + Chainlink Functions)

1. Deploy the contract on Arbitrum Sepolia with your subscription ID.
(Make sure your Chainlink Functions subscription is funded with LINK.)

2. Call getTemperature("London") or any other city.

3. Wait a few seconds — the oracle network processes the request.

4. Call getCity("London") to retrieve the stored weather data.

5. You can also view all requests with listAllCities().

## 🔗 Network Configuration
| Parameter | Value |
|-----------|-------|
| Router (Arbitrum Sepolia) |	`0x234a5fb5Bd614a7AA2FfAB244D603abFA0Ac5C5C` |
| DON ID |	`0x66756e2d617262697472756d2d7365706f6c69612d3100000000000000000000` |
| Functions Documentation |	`Chainlink Functions Docs` |

## 👨‍💻 Author

Müller Esposito
Blockchain Developer | 77 Innovation Labs
📧 Contact: @mulleresposito

## ⚖️ License

This project is licensed under the MIT License.
See the SPDX identifier at the top of the contract or read more here: MIT License