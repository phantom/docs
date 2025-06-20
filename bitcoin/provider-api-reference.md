# Provider API Reference

## Methods

### **`requestAccounts`**

#### **Description**

Connects to the user’s Phantom account.

#### **Parameters**

None

#### **Response**

<table><thead><tr><th width="258.3333333333333">Property</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>Promise&#x3C;BtcAccount[]></code></td><td><code>array</code></td><td>Array of the connected user’s <code>BtcAccount</code> objects</td></tr></tbody></table>

#### `BtcAccount` Response Object Properties

| Property      | Type                                    | Description                                                                                                                                                            |
| ------------- | --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `address`     | `string`                                | The Bitcoin address owned by the user                                                                                                                                  |
| `addressType` | `p2tr` \| `p2wpkh` \| `p2sh` \| `p2pkh` | The address's [format](https://bitcoin.design/guide/glossary/address/)                                                                                                 |
| `publicKey`   | `string`                                | A hex string representing the bytes of the public key of the account                                                                                                   |
| `purpose`     | `payment` \| `ordinals`                 | The general purpose of the address. If `ordinals`, the user prefers to store ordinals on this address. If `payment`, the user prefers to store bitcoin on this address |

### **`signMessage`**

#### Description

Signs a message with the user’s Phantom account.

#### Parameters

| Property  | Type         | Description                                                         |
| --------- | ------------ | ------------------------------------------------------------------- |
| `message` | `Uint8Array` | The message to be signed                                            |
| `address` | `string`     | One of the user’s addresses that should be used to sign the message |

#### Response

| Property                              | Type     | Description                     |
| ------------------------------------- | -------- | ------------------------------- |
| `Promise<{ signature: Uint8Array; }>` | `object` | Object containing the signature |

### **`signPSBT`**

#### **Description**

Signs a Partially-Signed Bitcoin Transaction.

#### **Parameters**

| Property  | Type         | Description                                |
| --------- | ------------ | ------------------------------------------ |
| `psbtHex` | `Uint8Array` | A serialized PSBT                          |
| `options` | `object`     | Configuration options for signing the PSBT |

#### `options` parameters

| Property       | Type                                                                           | Description                                                                                |
| -------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------ |
| `inputsToSign` | `{sigHash?: number \| undefined, address: string, signingIndexes: number[]}[]` | An array containing the indexes of which transaction inputs to sign, and how to sign them. |

#### **Response**

| Property          | Type     | Description                                                                          |
| ----------------- | -------- | ------------------------------------------------------------------------------------ |
| `Promise<string>` | `string` | A serialized PSBT where the inputs belonging to the user’s account have been signed. |

## Events

### **`accountsChanged`**

#### Description

The event that is emitted when a user changes their connected Phantom account.

```typescript
type AccountsChangedEvent = (accounts: BtcAccount[]) => void;
```

#### Properties

| Property   | Type           | Description                                                            |
| ---------- | -------------- | ---------------------------------------------------------------------- |
| `accounts` | `BtcAccount[]` | The array of `BtcAccount` objects of the newly connect Phantom account |

#### `BtcAccount` Response Object Properties

| Property      | Type                                    | Description                                                                                                                                                            |
| ------------- | --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `address`     | `string`                                | The Bitcoin address owned by the user                                                                                                                                  |
| `addressType` | `p2tr` \| `p2wpkh` \| `p2sh` \| `p2pkh` | The address’s format (see: [https://bitcoin.design/guide/glossary/address/](https://bitcoin.design/guide/glossary/address/))                                           |
| `publicKey`   | `string`                                | A hex string representing the bytes of the public key of the account                                                                                                   |
| `purpose`     | `payment` \| `ordinals`                 | The general purpose of the address. If `ordinals`, the user prefers to store ordinals on this address. If `payment`, the user prefers to store bitcoin on this address |
