# FAQ

## Why can't I access Phantom on my website?

Phantom will only [inject its provider](../getting-started-with-solana/detecting-the-provider.md) into websites that begin with `https://`, or if the host is `localhost` or `127.0.0.1`. If your website only uses `http://`, Phantom will not inject its provider and you will not be able to access the methods found at `window.phantom`. Encrypting your web traffic and upgrading to `https://` will restore functionality.

Phantom will also not inject its provider into any [iframe](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe).

## Why isn't my token displaying properly?

Phantom supports the [Token Metadata Standard](https://docs.metaplex.com/programs/token-metadata/overview) established by [Metaplex](https://www.metaplex.com/). When displaying tokens, Phantom will first [categorize](../best-practices/tokens/#categorizing-tokens) them according to their [TokenStandard](https://docs.metaplex.com/programs/token-metadata/token-standard). If a token is considered `Fungible`, Phantom will display it on the [Home tab](../best-practices/tokens/home-tab-fungibles.md). Otherwise, Phantom will display it as a [Collectible](../best-practices/tokens/collectibles-nfts-and-semi-fungibles.md). For more information, please review:

{% content-ref url="../best-practices/tokens/" %}
[tokens](../best-practices/tokens/)
{% endcontent-ref %}

## What types of NFTs are supported?

Phantom supports a range of NFT media types including images, audio files, video files, and 3D models. At this time, Phantom does not support HTML files. For a full list of the types of NFTs that Phantom will display, please reference:

{% content-ref url="../best-practices/tokens/supported-media-types.md" %}
[supported-media-types.md](../best-practices/tokens/supported-media-types.md)
{% endcontent-ref %}

## Are hardware wallets supported?

Yes, Phantom currently supports [Ledger](https://www.ledger.com/) and requires no special treatment on the application side.

## How does Phantom import wallet addresses?

When importing addresses from an existing seed phrase, Phantom will scan for 20 addresses in each of our three supported derivation paths (`bip44change`, `bip44`, and a deprecated path), for a total of 60 addresses. For the convenience of the user, Phantom will filter this list of addresses down to wallets that have ever had signatures (i.e. have ever been used). Phantom will then sort this filtered list based on how many signatures each wallet has had plus the amount of [lamports](https://docs.solana.com/terminology#lamport) it currently owns. &#x20;

## Why does Phantom prepend an additional instruction on standard SPL token transfers?

When transferring SPL tokens, Phantom will first double check that the owner of the receiving token account is the address you expect to send to. To do this, Phantom calls a custom deployment of the [Serum Assert Owner](https://github.com/project-serum/serum-dex/tree/6138ca98280f6433deecde560f3d23cc4a749bae/assert-owner) program. The program address of this deployment is `DeJBGdMFa1uynnnKiwrVioatTuHmNLpyFKnmB5kaFdzQ` and is available on Solana's Devnet, Testnet, and Mainnet.

## Why does Phantom show "transaction reverted during simulation" on devnet?

On **Solana devnet**, Phantom may display the following warning in the transaction approval dialog before the user signs:

> "This transaction reverted during simulation. Funds may be lost if submitted."

This warning indicates that Phantom's pre-sign simulation returned an error. In some devnet integrations—especially those using [`signTransaction`](../getting-started-with-solana/sending-a-transaction.md) with a dapp-owned RPC for broadcast—the transaction may still **succeed on-chain** after the user approves. This is a **simulation warning** displayed in the approval UI. It is distinct from provider [error code `-32003` (Transaction Rejected)](../getting-started-with-solana/errors.md), which is returned when Phantom rejects the signing request programmatically.

### What developers observe

Developers integrating with Phantom on devnet have reported the following pattern:

- The warning appears on **every** transaction, regardless of instruction type.
- The same unsigned transaction **simulates successfully** when tested against the dapp's own devnet RPC.
- Other wallets (for example, Solflare or Backpack) do **not** show the warning for the same transaction.
- If the user approves despite the warning, the transaction **lands and executes successfully** on devnet.

This pattern is most common when the dapp calls `signTransaction` (or `signAllTransactions`) and then broadcasts the signed payload via its own `Connection`, rather than using `signAndSendTransaction`.

### How Phantom simulation works

Before displaying the approval dialog, Phantom simulates the transaction against Solana to predict its on-chain outcome and surface balance changes. This protects users from signing transactions that would fail after submission.

The simulation flow works as follows:

1. Your dapp calls a Phantom provider method (for example, `signTransaction`) with an unsigned transaction.
2. Phantom submits a `simulateTransaction` request to its RPC infrastructure at a specific **commitment level**.
3. If simulation reports an error, Phantom displays the reverted warning in the approval UI.
4. If the user approves, Phantom signs the transaction and returns it to your dapp.
5. Your dapp may then broadcast the signed transaction through **its own RPC endpoint**—a separate context from Phantom's simulation step.

Several properties of this pipeline explain why devnet false positives occur:

- **Pre-sign simulation is intentional.** Phantom simulates before prompting the user to sign so that users can review predicted balance changes and avoid signing transactions that would revert on-chain.
- **Simulation context may differ from your send path.** Phantom uses its own RPC infrastructure and commitment level. Your dapp's `Connection` may point to a different RPC node, slot, or fork state—particularly on devnet, where cluster stability and RPC latency vary more than on mainnet-beta.
- **`signTransaction` decouples simulation from broadcast.** When you use `signTransaction` and submit via `sendRawTransaction`, Phantom simulates once at sign time but never sends the transaction. It cannot re-simulate at broadcast time because submission happens outside Phantom.
- **Devnet cluster state is less stable.** Block production, fork choice, and RPC responsiveness on devnet can cause a transaction to simulate as failed at one slot and succeed when submitted moments later at another.

These warnings exist to protect users. A devnet false positive is a limitation of test-network simulation—not a signal that developers should ignore all Phantom warnings on mainnet-beta.

### Related warning messages

Phantom displays several distinct warnings during transaction approval. Conflating them makes debugging harder. Use the table below to identify which scenario you are encountering:

| User-facing message | Typical cause | Devnet false positive? |
| --- | --- | --- |
| "This transaction reverted during simulation. Funds may be lost if submitted." | Simulation returned a program or runtime error | **Yes — common on devnet** when your local simulation passes |
| "This dApp could be malicious. Do not proceed unless you are certain it is safe." | Phantom cannot safely predict the transaction outcome (multi-signer flows, oversized transactions, etc.) | Sometimes — see [Domain and transaction warnings](https://docs.phantom.com/developer-powertools/domain-and-transaction-warnings) |
| "Unable to simulate" or fee estimation failures | RPC timeout, stale blockhash, or network mismatch between dapp and wallet | Often devnet infrastructure; the transaction may still succeed |
| Error `4001` (User Rejected Request) without user interaction | Internal timeout before the approval dialog fully renders | Possible — a separate issue pattern from simulation reverts |

### Determine whether the warning is a false positive

Use the checklists below before assuming your transaction construction is incorrect.

**Likely false positive — verify locally, then proceed with caution on devnet:**

- Your dapp's `connection.simulateTransaction()` passes for the same unsigned transaction.
- The same transaction simulates cleanly in Solflare or Backpack on devnet.
- Phantom **Testnet Mode** is enabled (Settings → Developer Settings → Testnet Mode) **and** your dapp's RPC endpoint targets devnet (for example, `clusterApiUrl('devnet')`), not mainnet-beta.
- You fetched a fresh blockhash immediately before prompting Phantom to sign—not reused from an earlier build step.
- The transaction succeeds on-chain after the user approves despite the warning.

**Likely real failure — fix your transaction before prompting the user:**

- Your local simulation returns `simulation.value.err` with program logs indicating the root cause.
- All wallets show a failure or block the transaction.
- Required accounts are missing, program IDs are incorrect, or the fee payer lacks sufficient lamports or token balance.

### Verify a transaction before prompting Phantom to sign

Simulate the transaction locally using the same network and commitment level you will use for broadcast. The helper below refreshes the blockhash, simulates with options appropriate for unsigned transactions, and returns structured results you can log or surface in your dapp UI.

```javascript
import { Connection, clusterApiUrl } from "@solana/web3.js";

/**
 * Simulate a transaction before asking Phantom to sign.
 * Use this to distinguish Phantom devnet false positives from real on-chain failures.
 */
async function verifyTransactionBeforeSigning(connection, transaction, feePayer) {
  // Always refresh the blockhash immediately before signing. Stale blockhashes
  // are a common cause of simulation and send mismatches across RPC nodes.
  const { blockhash, lastValidBlockHeight } = await connection.getLatestBlockhash("confirmed");
  transaction.recentBlockhash = blockhash;
  transaction.feePayer = feePayer;

  const simulation = await connection.simulateTransaction(transaction, {
    sigVerify: false,             // transaction is unsigned at this point
    replaceRecentBlockhash: true, // RPC replaces blockhash if needed for simulation
    commitment: "confirmed",      // match the commitment used for getLatestBlockhash
  });

  if (simulation.value.err) {
    return {
      ok: false,
      err: simulation.value.err,
      logs: simulation.value.logs ?? [],
      unitsConsumed: simulation.value.unitsConsumed,
    };
  }

  return {
    ok: true,
    logs: simulation.value.logs ?? [],
    unitsConsumed: simulation.value.unitsConsumed,
    lastValidBlockHeight,
  };
}

// Usage before provider.signTransaction(transaction)
const connection = new Connection(clusterApiUrl("devnet"), "confirmed");
const result = await verifyTransactionBeforeSigning(connection, transaction, walletPublicKey);

if (!result.ok) {
  // Real failure — inspect result.logs and fix the transaction before prompting Phantom.
  throw new Error(`Simulation failed: ${JSON.stringify(result.err)}`);
}

// Local simulation passed but Phantom still warns on devnet?
// This may be a devnet false positive — see the checklist above.
const signed = await provider.signTransaction(transaction);
const signature = await connection.sendRawTransaction(signed.serialize(), {
  skipPreflight: false,
  preflightCommitment: "confirmed", // keep send commitment aligned with simulation
});
```

#### Commitment alignment

Mismatched commitment levels between blockhash fetch, simulation, and send are a frequent source of spurious simulation failures (including "blockhash not found" errors). Follow these rules:

- Fetch the blockhash and simulate at the **same** commitment level (for example, `"confirmed"`).
- Pass the same `preflightCommitment` to `sendRawTransaction` or `sendTransaction`.
- Avoid mixing `finalized` blockhash context with `processed` send context—the RPC node may simulate against a slot older than the blockhash embedded in your transaction.

For additional background, see Solana's guidance on [transaction confirmation and expiration](https://solana.com/developers/guides/advanced/confirmation).

### Dapp integration recommendations

Apply these recommendations in order of preference:

1. **Prefer `signAndSendTransaction`** when you do not require a custom broadcast path. Phantom signs and submits through its RPC connection, keeping simulation and send in a single context. See [Sending a Transaction](../getting-started-with-solana/sending-a-transaction.md).
2. **If you must use `signTransaction`** (multi-signature flows, relayers, or batching before submission): run local simulation first, refresh the blockhash immediately before the Phantom prompt, and broadcast with an aligned `preflightCommitment`.
3. **Align networks end-to-end.** Phantom Testnet Mode and your dapp `Connection` must both target devnet. Verify your RPC URL and confirm the user has Testnet Mode enabled before testing.
4. **Do not suppress Phantom warnings in your UI.** If your local pre-flight check passes, you may inform users that devnet simulation warnings can be conservative—but never instruct users to ignore warnings on mainnet-beta.
5. **Validate on mainnet-beta before launch.** Devnet simulation behavior does not represent mainnet-beta. Always test production flows on mainnet-beta with real (small) transactions before releasing to users.

### When to escalate

- **Local simulation fails:** Fix your transaction construction using program logs. This is not a Phantom-specific issue.
- **Local simulation passes, other wallets pass, but Phantom warns on every devnet transaction:** Open or update a GitHub issue with your Phantom version, network, provider method (`signTransaction` vs `signAndSendTransaction`), and a base64-encoded sample transaction.
- **Need integration help:** Join the [Phantom developer Discord](https://discord.gg/j5Dp7ztzvW).
