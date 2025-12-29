# Verification & Gating

Azalea Gate's core feature is linking Discord users to their on-chain assets.

## How it Works
1.  **User Flow**:
    -   User types `/verify` or clicks a "Verify" button in a channel.
    -   They receive a unique link to the web verification page.
    -   They connect their wallet (Phantom, MetaMask, etc.) and sign a message to prove ownership.
    -   The bot checks their assets and assigns roles instantly.

2.  **Continuous Checking**:
    -   The bot periodically re-scans wallets.
    -   If an asset is sold/transferred, the role is removed automatically.

## Supported Chains
-   **Solana** (Native SPL, Metaplex NFTs)
-   **EVM** (Ethereum, Polygon, Base, Optimism, Arbitrum)
-   **TON** (The Open Network)

## Configuring Rules

### NFT Verification
Assign a role to holders of a specific NFT Collection.

**Command**:
```
/nft-config add [collection_address] [role]
```
-   `collection_address`: The Update Authority or Collection Address (Solana), or Contract Address (EVM).
-   `role`: The discord role to assign.

### Token Verification
Assign a role to holders of a fungible token (e.g., $BONK, $USDC).

**Command**:
```
/token-config add [token_address] [role] [min_amount]
```
-   `min_amount`: The minimum number of tokens required.

### Staking Verification
Assign a role to users who have staked their NFTs/Tokens in supported protocols.

**Command**:
```
/staking-config add [type] [address] [role]
```

## Verification Panel
Create a permanent "Verify Here" button in a channel.

**Command**:
```
/setup-verification-panel [channel_id]
```
This sends an embed with a button that users can click to start the verification flow without typing commands.
