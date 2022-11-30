# Accounts Changed

Event emitted upon changing accounts within your Phantom wallet.

````typescript
window.ethereum.on('accountsChanged', (newAccounts: String[]) => {
      // "newAccounts" will always be an array, but it can be empty.      
      if (newAccounts) {
        console.log(`switched to new account: ${newAccounts}`);
        accounts = newAccounts;
      } else {
        /**
         * In this case dApps could...
         *
         * 1. Not do anything
         * 2. Only re-connect to the new account if it is trusted
         *
         * ```
         * provider.send('eth_requestAccounts', []).catch((err) => {
         *  // fail silently
         * });
         * ```
         *
         * 3. Always attempt to reconnect
         */
  }
})
````

You can see an example of [hooking into this event](https://github.com/phantom-labs/eth\_sandbox/blob/main/src/App.tsx#L114-L152) in our sandbox.
