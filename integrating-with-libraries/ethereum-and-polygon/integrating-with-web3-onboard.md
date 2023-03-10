# Integrating with Web3-Onboard

This tutorial is a step-by-step guide on how to integrate a wallet such as Phantom into your dapp using the [Web3-Onboard](https://onboard.blocknative.com/) library.

We will be going through step by step how to go from 0 to a fully integrated Web3-Onboard button. If you already have a dapp that you are trying to integrate Phantom in, you can use the [our project](https://github.com/phantom-labs/Web3-Onboard-Integration.git) as reference.

### Prerequisites

* Node version >=16.12.0
* A text editor/IDE (such as VSCode)
* Some Knowledge of React

### Creating The App

We recommend using [Vite](https://vitejs.dev/) to create new react applications.

To create a new React application using Vite, run the following command in your terminal:

```sh
yarn create vite@latest
```

1. This will ask you for a project name. Provide it a name here. For purposes of this tutorial I used "Web3-onboard-Sandbox".
2. It will then ask you to select a framework. Select "React" here.
3. Next it will ask for a variant. Select "Typescript" here.

Now change directory into your project and run:

```bash
yarn install
```

And make sure your app runs by running the command:

```bash
yarn dev
```

Before moving forward, we're going to configure Vite to recognize the `window.ethereum` object. Since we are in Typescript if we try to use `window.ethereum` without first doing this step, we will get a type error and be unable to compile our project.

open the `vite-env.d.ts` file in the `src` directory.

It should look like this:

```typescript
/// <reference types="vite/client" />
```

All you need to do is extend the `Window` interface. To do so modify the `vite-env.d.ts` file to look like so:

```typescript
/// <reference types="vite/client" />

interface Window {
    ethereum: any;
}
```

Now whenever we reference `window.ethereum`, we will not encounter any type errors.

With your app running, we can now move onto the Web3-Onboard specific pieces.

### Installing Web3-Onboard

To install the appropriate package, run the following:

```bash
yarn add @web3-onboard/react @web3-onboard/phantom
```

Now you can use the web3-onboard package in your project and integrate Phantom as a wallet in the modal.

### Initializing Web3-Onboard

First we will need to import the packages into our project. At the top of the `App.tsx` file add these two lines:

```typescript
import { init, useConnectWallet } from "@web3-onboard/react";
import phantomModule from "@web3-onboard/phantom";
```

Next, we will can initialize the modal by calling the init hook. You can add this code to the project between your imports and your `App` component.

```typescript
const phantom = phantomModule();

init({
  wallets: [phantom],
  chains: [
    {
      id: "0x1",
      token: "ETH",
      label: "Ethereum Mainnet",
      rpcUrl:
        "https://eth-mainnet.g.alchemy.com/v2/YOUR_API_KEY_HERE",
    },
  ]
});
```

Here you can see that we call the `init` hook that we imported, and provide the `phantomModule` to an array of `wallets`. Here you can also specify which chains your dapp supports.

Each chain requires an `id`, `token`, `label`, and `rpcUrl`.  Phantom currently supports Polygon, Ethereum Mainnet, Goerli, and Mumbai.

You can add as many chains as you would like. There are many configuration options that Web3-Onboard provides you. To see a full list of the different options you can check out their documentation [here](https://onboard.blocknative.com/docs/modules/core#quick-start). You can customize your theme, i18n options, app metadata, realtime app notifications, and more.

With this bit of configuration out of the way we only need to add the button that will call the modal to the forefront of the screen to connect our wallet.

### Adding A Connect Wallet Button

In your `App` component, you will utilize the `useConnectWallet` hook that you imported earlier.

Add this to the first line inside your component:

```typescript
  const [{ wallet, connecting }, connect, disconnect] = useConnectWallet();
```

This gives you the `connect` and `disconnect` functions that we will attach to our button, as well as the `wallet` and `connecting` keys in an object that allow us to track state of various wallets and connection statuses.

We now have everything we need to add the actual UI. You can place this block in place of the basic counter that comes in the Vite starter.

```jsx
<button disabled={connecting} onClick={() => (wallet ? disconnect(wallet) : connect())}>
    {connecting ? 'connecting' : wallet ? 'disconnect' : 'connect'}
</button>
```

If you save your project you should now have working connect button that displays the Phantom wallet button. Clicking on it will establish a connection to the dapp.

Here's what it will look like:

<img src="../../.gitbook/assets/Screen Shot 2023-03-10 at 1.23.44 PM.png" alt="" data-size="original">![](<../../.gitbook/assets/Screen Shot 2023-03-10 at 1.23.46 PM.png>)

### Conclusion

Web3-Onboard provides quite a lot out of the gate with very little work on your end as a developer. We hope that you enjoyed this guide.

If you got lost along the way do not worry. You can find the source code [here](https://github.com/phantom-labs/Web3-Onboard-Integration.git) to double check your work.
