# \_eventsCount

## window.phantom.ethereum.\_eventsCount

An object containing the number of events that have happened

```javascript
const eventsCount = window.phantom.ethereum._eventsCount;
console.log(eventsCount);
// 2
```

This is **not** a recommended way to keep track of different events. The provider implements a Node.js `EventEmitter` API to emit different events happening within the wallet and/or dapp.&#x20;
