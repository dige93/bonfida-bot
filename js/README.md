[![npm (scoped)](https://img.shields.io/npm/v/bonfida-bot)](https://www.npmjs.com/package/bonfida-bot)

# Bonfida-bot JS library

A JavaScript client library for interacting with the bonfida-bot on-chain program. This library can be used for :

- Creating pools.
- Trading on behalf of pools by sending signals.
- Depositing assets into a pool in exchange for pool tokens.
- Redeeming pool tokens to retrieve an investement from the pool.

## Installation

This library provides bindings for different pool operations. Adding it to a project is quite easy.
Using npm:

```
npm install bonfida-bot
```

Using yarn:

```
yarn add bonfida-bot
```

## Usage

To create a pool :

```ts
import { Connection, Account } from '@solana/web3.js';
import { createPool, } from 'bonfida-bot/instructions';
import { signAndSendTransactionInstructions, } from 'bonfida-bot/utils';



const connection = new Connection("https://mainnet-beta.solana.com");

const sourceOwnerAccount = Account(<private_key_array>);
const signalProviderAccount = Account(<private_key_array>);

// Creating a pool means making the initial deposit.
// Any number of assets can be added. For each asset, the public key of the payer token account should be provided.
// The deposit amounts are also given
const sourceAssetKey = [
    new PublicKey("<First asset Key>"),
    new PublicKey("<Second asset Key>"),
]
const deposit_amounts = [1, 1];

// Maximum number of assets that can be held by the pool at any given time.
// A higher number means more flexibility but increases the initial pool account allocation fee.
const maxNumberOfAsset = 10; 

// It is necessary for the purpose of security to hardcode all Serum markets that will be usable by the pool.
// It is advised to put here as many trusted markets with enough liquidity as required.
const allowedMarkets = [marketInfo.address];

// Interval of time in seconds between two fee collections. This must be greater or equal to a week.
// @ts-ignore
const feeCollectionPeriod = new Numberu64(604800); 

// Total ratio of pool to be collected as fees at an interval defined by feeCollectionPeriod
// @ts-ignore
const feeRatio = 0.001; 


// Create pool
let [poolSeed, createInstructions] = await createPool(
    connection,
    sourceOwnerAccount.publicKey
    sourceAssetKeys,
    signalProviderAccount.publicKey,
    deposit_amounts,
    maxNumberOfAsset,
    allowedMarkets,
    payerAccount.publicKey,
    // @ts-ignore
    feeCollectionPeriod,
    // @ts-ignore
    feeRatio
)

await signAndSendTransactionInstructions(
    connection,
    [sourceOwnerAccount], // The owner of the source asset accounts must sign this transaction.
    payerAccount,
    createInstructions
);
console.log("Created Pool")

```
