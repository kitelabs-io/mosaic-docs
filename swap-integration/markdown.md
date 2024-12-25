---
hidden: true
---

# Typescript SDK

### [https://www.npmjs.com/package/@mosaic-ag/ts-sdk](https://www.npmjs.com/package/@mosaic-ag/ts-sdk)

### 1. Importing Required Modules

First, we need to import the necessary modules and dependencies for the Aptos Movement blockchain transactions and swap data retrieval:

```javascript
import { getSwapData } from '@mosaic-ag/ts-sdk';
import { Aptos, AptosConfig, Network, APTOS_COIN, Ed25519PublicKey, Ed25519PrivateKey, Account } from '@aptos-labs/ts-sdk';
import invariant from 'tiny-invariant';
```

#### 2. Setting Up Constants

Next, define the constants that will be used throughout the script. These include wallet addresses, keys, and asset identifiers:

```javascript
export const TEST_WALLET_ADDRESS = ''; // change me
export const TEST_PRIVATE_KEY = ''; // change me
export const TEST_PUBLIC_KEY = ''; // change me
export const USDC = '0x275f508689de8756169d1ee02d889c777de1cebda3a7bbcce63ba8a27c563c6f::tokens::USDC';
export const SLIPPAGE_BPS = 50;
```

#### 3. Configuring Aptos Movement

Set up the configuration for the Aptos Movement blockchain network:

```javascript
const aptosConfig = new AptosConfig({ network: Network.CUSTOM, fullnode: "https://aptos.testnet.suzuka.movementlabs.xyz/v1" })
export const aptos = new Aptos(aptosConfig);
```

#### 4. Retrieving Swap Data

Retrieve the necessary swap data for the transaction using the `getSwapData` function:

```javascript
const swapData = await getSwapData({
  tokenIn: APTOS_COIN,
  tokenOut: USDC,
  amountIn: '1000000',
  slippageBps: 50, // 0.5%
  feeRecipient: '0x24570782d195e458b6e67c52e373ad1c54e18b4ac41e7b4cd2cddec255e42ffb',
  feeBps: 50, // 0.5%
  chargeFeeBy: 'token_in', // token_in
});
```

#### 5. Building the Transaction

Build a simple transaction with the swap data retrieved:

```javascript
const transaction = await aptos.transaction.build.simple({
  sender: TEST_WALLET_ADDRESS,
  data: {
    function: swapData.function,
    functionArguments: swapData.functionArguments,
    typeArguments: swapData.typeArguments,
  },
});
```

#### 6. Simulating the Transaction

Simulate the transaction to ensure it will succeed:

```javascript
const simulateResponse = await aptos.transaction.simulate.simple({
  signerPublicKey: new Ed25519PublicKey(TEST_PUBLIC_KEY),
  transaction,
});

invariant(simulateResponse.length === 1, `unexpected error, simulateResponse = ${JSON.stringify(simulateResponse)}`);
invariant(simulateResponse[0].success, 'simulate failed');
```

#### 7. Signing and Submitting the Transaction

Sign and submit the transaction using the private key:

```javascript
const pendingTxResponse = await aptos.transaction.signAndSubmitTransaction({
  signer: Account.fromPrivateKey({ privateKey: new Ed25519PrivateKey(TEST_PRIVATE_KEY) }),
  transaction,
});
```

#### 8. Waiting for Transaction Confirmation

Wait for the transaction to be confirmed on the blockchain:

```javascript
const committedTxResponse = await aptos.transaction.waitForTransaction({ transactionHash: pendingTxResponse.hash });
invariant(committedTxResponse.success, 'transaction failed');
```

#### 9. Logging the Transaction Result

Finally, log the success message with the transaction details:

```javascript
console.log(`Success. https://explorer.movementnetwork.xyz/txn/${committedTxResponse.version}?network=testnet`);
```

Each block of code corresponds to a specific step in the process of setting up, building, simulating, signing, submitting, and confirming a transaction on the Aptos blockchain. This structure helps in understanding the flow and individual responsibilities of each part of the script.
