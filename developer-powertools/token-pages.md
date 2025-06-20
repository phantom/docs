# Token Pages

## Phantom Token Pages

Phantom provides shareable, web-accessible token pages that display detailed token information. These pages provide a seamless experience whether accessed through the Phantom wallet app or via web browsers.

### Generating Token Page URLs

To create a shareable token page URL:

1. Open the desired token page in Phantom
2. Tap the share button
3. Copy or share the generated URL

The URLs follow this format:

```
https://phantom.com/tokens/<chain>/<token_address>
```

For example, the WIF token on Solana can be accessed at:

```
https://phantom.com/tokens/solana/EKpQGSJtjMFqKZ9KQanSqYXRcF8fBopzLHYxdM65zcjm
```

### Key Features

#### Universal Deep Linking

When accessed on mobile devices with Phantom installed, token page URLs automatically open the corresponding details page within the Phantom app. For users without Phantom installed, the links gracefully fallback to the web view.

#### Quick Access to Swapper

The web view includes a QR code that, when scanned, deep links directly to the token's swap screen within the Phantom app. This enables quick and convenient token swaps for mobile users.

<figure><img src="../.gitbook/assets/Screenshot 2025-02-05 at 11.44.32 AM.png" alt=""><figcaption><p>Token Page Web View</p></figcaption></figure>

<figure><img src="../.gitbook/assets/IMG_6005 (1).PNG" alt="" width="188"><figcaption><p>Token Page Mobile App View</p></figcaption></figure>

#### Enhanced Social Sharing

Each token page generates dynamic OpenGraph images that display essential token information and price charts. When shared on social media platforms, these rich previews provide immediate context about the token's performance and status.

<figure><img src="../.gitbook/assets/Screenshot 2025-02-05 at 11.58.40 AM.png" alt=""><figcaption><p>Token Page Image from X.com</p></figcaption></figure>



