# \_events

## window.ethereum.\_events

An object containing all of the events that the provider has emitted or logged.

```javascript
const events = window.ethereum._events
console.log(events)
// Events {chainChanged: Array(2), accountsChanged: EE}
```

This is **not** a recommended way to keep track of different events. The provider implements a Node.js `EventEmitter` API to emit different events happening within the wallet and/or dapp. See [Events](../events/) for more details.
