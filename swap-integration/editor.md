---
description: >-
  This section will walk you through the required steps to get quote and submit
  a swap using Mosaic Aggregator.
---

# API

## Swagger <a href="#swagger" id="swagger"></a>

{% swagger src="../.gitbook/assets/mosaic-api.yaml" path="/v1/quote" method="get" %}
[mosaic-api.yaml](../.gitbook/assets/mosaic-api.yaml)
{% endswagger %}

## Setup Authentication

{% hint style="info" %}
**All requests to Mosaic’s API must include an `X-API-Key` header for authentication. The API key is used to track your usage and ensure secure access. To obtain an API key, please contact the Mosaic team via** [_**Discord**_](https://discord.gg/mosaicagg) _**or**_ [_**Telegram**_](http://t.me/mosaicaggchat) _**or**_ [_**Email**_](https://app.gitbook.com/u/wFeDG3R5KaeybOt5PaquEpON6pM2)
{% endhint %}

## Get quote

To retrieve the best swap rate, send the following HTTP request to get a quote:

```bash
curl 'https://api.mosaic.ag/v1/quote?srcAsset=0x1%3A%3Aaptos_coin%3A%3AAptosCoin&dstAsset=0x275f508689de8756169d1ee02d889c777de1cebda3a7bbcce63ba8a27c563c6f%3A%3Atokens%3A%3AUSDC&amount=1000000000&sender=0x0000000000000000000000000000000000000000000000000000000000000000&slippage=10' \
--header 'x-api-key: xxx'
```

This will return the best available quote for swapping the source asset (AptosCoin) into the destination asset (USDC) based on current market conditions.

Here is a detailed guide using Typescript:

### 1. Setup Environment

Import the required libraries and initialize your connection to the Aptos testnet using the `AptosConfig` and `Aptos` objects from the `@aptos-labs/ts-sdk`. This sets up your environment for making requests to the Mosaic Aggregator API and interacting with Aptos.

```javascript
import {
  Account,
  Aptos,
  APTOS_COIN,
  AptosConfig,
  Ed25519PrivateKey,
} from "@aptos-labs/ts-sdk";
import axios from "axios";

// Initialize Aptos config to connect to the testnet
const aptos = new Aptos(
  new AptosConfig({
    fullnode: "https://aptos.testnet.suzuka.movementlabs.xyz/v1",
  })
);
```

### 2. Setup User Account

Here, you will create an Aptos account using the user's private key. You need to fill in your own private key when initializing the account.

```javascript
// Create user account from private key
const user = Account.fromPrivateKey({
  privateKey: new Ed25519PrivateKey(), // TODO: Fill your private key here.
});
```

### 3. Define Assets and Amount

Specify the assets to swap and the amount (in APTOS decimals):

```javascript
const srcAsset = APTOS_COIN; // Source: APTOS Coin
const dstAsset =
  "0x275f508689de8756169d1ee02d889c777de1cebda3a7bbcce63ba8a27c563c6f::tokens::USDC"; // Destination: USDC
const amount = 1_00000000; // Amount: 1 APT (APTOS uses 8 decimals)
```

### 4. Get a Quote from Mosaic Aggregator API

Retrieve the best swap rate by making a GET request to the Mosaic API.

```javascript
// Get a quote from Mosaic Aggregator API
const mosaicResponse = await axios({
  method: "GET",
  url: "https://api.mosaic.ag/v1/quote",
  params: {
    srcAsset,
    dstAsset,
    amount,
    sender: user.accountAddress.toString(),
    slippage: 100, // 100 = 1%
  },
  headers: {
    "X-API-KEY": "xxx", // TODO: Fill the API key in here.
  },
});
```

### 5. Build the transaction

Once you have the quote, you use the returned data (function, type arguments, and function arguments) to build a transaction that will perform the swap.

```javascript
// Build the transaction based on the Mosaic response
const transaction = await aptos.transaction.build.simple({
  sender: user.accountAddress,
  data: {
    function: mosaicResponse.data.data.tx.function,
    typeArguments: mosaicResponse.data.data.tx.typeArguments,
    functionArguments: Object.values(
      mosaicResponse.data.data.tx.functionArguments
    ),
  },
});
```

### 6. Sign and submit the transaction

After building the transaction, you sign it using the user's private key and submit the transaction to the Aptos blockchain. The transaction hash will be logged, which can be used to check the transaction status on the explorer.

```javascript
// Sign and submit the transaction
const pendingTransactionResponse =
  await aptos.transaction.signAndSubmitTransaction({
    signer: user,
    transaction: transaction,
  });

// Output transaction URL to the console
console.log(
  `Tx = https://explorer.movementnetwork.xyz/txn/${pendingTransactionResponse.hash}?network=testnet`
);
```

_By following this guide, you will be able to integrate the Mosaic Aggregator API for performing token swaps on the Aptos network. For further assistance or inquiries, feel free to reach out to the Mosaic support team on_ [_Discord_](https://discord.gg/mosaicagg) _or_ [_Telegram_](http://t.me/mosaicaggchat)_._\
\
