# Establishing a Connection

## Connecting

Once an application has [detected the provider](detecting-the-provider.md), it can then request to connect to Phantom by invoking the `requestAccounts` method. This connection request will prompt the user for permission to share their Bitcoin addresses, indicating that they are willing to interact further. Users must approve a connection request before the app can make additional requests such as [signing a message](signing-a-message.md) or [sending a transaction](../ethereum-monad-testnet-base-and-polygon/sending-a-transaction.md).\


<figure><img src="../.gitbook/assets/image (16).png" alt="" width="375"><figcaption><p>The Phantom popup that is triggered when requestAccounts is invoked for the first time</p></figcaption></figure>

The `requestAccounts` method returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) that resolves if the user approves the connection request. Once resolved, it contains an array of the user’s `BtcAccount` objects. If the user declines the request or closes the pop-up, it will reject (throw when awaited).

The array of `BtcAccount` objects represent the possible addresses that can be used when a user connects with Phantom. This is defined as:

```typescript
type BtcAccount = {
  address: string;
  addressType: "p2tr" | "p2wpkh" | "p2sh" | "p2pkh";
  publicKey: string;
  purpose: "payment" | "ordinals";
};
```

Where:

`address` - The Bitcoin address owned by the user

`addressType` - The address’s format (see: [https://bitcoin.design/guide/glossary/address/](https://bitcoin.design/guide/glossary/address/))

`publicKey` - A hex string representing the bytes of the public key of the account

`purpose` - The general purpose of the address. If `ordinals`, the user prefers to store ordinals on this address. If `payment`, the user prefers to store bitcoin on this address

{% hint style="warning" %}
The purpose fields represent user preferences and do not represent a guarantee that there will only be bitcoin on `payment` addresses or only ordinals on `ordinals` addresses. When submitting a transaction, make sure to carefully select UTXOs such that you do not cause the user to accidentally spend a rare or inscribed satoshi.
{% endhint %}

The following is an example of how to connect to Phantom:

```typescript

const phantomProvider = getProvider(); // see "Detecting the Provider"
const accounts = await phantomProvider.requestAccounts();
console.log(accounts);
/**
[
    {
        "address": "bc1phajtersv55fwud5xr0t70p5234swy396a6avqhuny0qf83zssvrsm7tl4q",
        "publicKey": "02435c68c1f20522946e46bcc39f8a19088095939db840e874e1ad2c18f0f2361c"
        "addressType": "p2tr"
        "purpose": "ordinals"
    },
    {
        "address": "bc1qqy4emmf7q623vlf2z4j3pvgfyjkaffx2u3fkpk",
        "publicKey": "0279f5b4123030accae6c4ca9a17263753a21df2142bbf0ced940d28ca613e87f9"
        "addressType": "p2wpkh"
        "purpose": "payment"
    }
]
**/
```

## Disconnecting

There is no way for dApps to programmatically disconnect a user. Once a user has established a connection, Phantom will add the website they opened a connection with to a list of "Connected Apps." When a user returns to one of these whitelisted sites, Phantom will attempt to reconnect the application automatically. At any time, the user can revoke access to the dApp through their UI by navigating to “Settings” > “Connected Apps”. If a user disconnects from a dApp, they will need to reconnect on the subsequent visit before signing a message or sending a transaction.

<figure><img src="../.gitbook/assets/image (17).png" alt="" width="375"><figcaption><p>Users can disconnect from dApps within their “Connected Apps” settings</p></figcaption></figure>

## Changing Accounts

Phantom allows users to manage multiple accounts from within a single extension or mobile app. Whenever a user switches accounts, Phantom will emit an `accountsChanged` event.

```typescript
phantomProvider.on('accountsChanged', (accounts: BtcAccount[]) => {
  console.log('Accounts changed:', accounts);
});
```

If Phantom passes an empty array on `accountsChanged`, it implies that the user has switched to an account that has not connected to the dApp before (i.e. not already in “Connected Apps”) or does not support Bitcoin (e.g. a Solana private key account). In this case, the application can either do nothing or attempt to reconnect:

```typescript
phantomProvider.on('accountsChanged', (accounts: BtcAccount[]) => {
    if (accounts.length > 0) {
      // Set new address and continue as usual
    } else {
      // Attempt to reconnect to Phantom
      phantomProvider.requestAccounts().catch((error) => {
      // handle connection failure
      });
    }
});
```
