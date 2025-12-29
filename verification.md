# Verification & Gating

Azalea Gate connects Discord identities to blockchain wallets, enabling role-based access based on asset ownership.

## Supported Networks

| Network | Command | Wallets |
| :--- | :--- | :--- |
| **Solana** | `/verify` | Phantom, Solflare, Glow, Backpack |
| **Ethereum** | `/verify-evm` | MetaMask, Rainbow, Coinbase Wallet |
| **Polygon** | `/verify-evm` | MetaMask, Rainbow, WalletConnect |
| **Base** | `/verify-evm` | MetaMask, Coinbase Wallet |
| **Optimism** | `/verify-evm` | MetaMask, Rainbow |
| **Arbitrum** | `/verify-evm` | MetaMask, Rainbow |
| **TON** | `/verify-ton` | Tonkeeper, TON Space |

---

## How Verification Works

### 1. User Initiates Verification
User types `/verify` (or `/verify-evm`, `/verify-ton`) in Discord.

### 2. Verification Portal
User receives a DM with a unique link to the verification portal:
- **Portal URL**: [https://azaleagate.com/verify](https://azaleagate.com/verify)

### 3. Wallet Connection
User connects their wallet and signs a message (no transaction, no gas fees).

### 4. Role Assignment
Bot checks the wallet's assets and assigns configured roles instantly.

### 5. Continuous Monitoring
Bot periodically re-checks wallets. If an asset is sold, the role is removed.

---

## Solana Verification

### Setup (Admin)
```
/nft-config add-collection [collection_address] [role]
```
- `collection_address`: The Metaplex Collection Address or Update Authority.
- `role`: The Discord role to assign to holders.

### Example
```
/nft-config add-collection DRiP2Pn2K6fuMLKQmt5rZWyHiUZ6WK3GChEySUpHSS4x @Verified
```
Holders of the DRiP collection get the `@Verified` role.

### User Commands
| Command | Description |
| :--- | :--- |
| `/verify` | Start Solana verification |
| `/unverify` | Remove your wallet link |
| `/check-verification` | Check if you're verified |
| `/verification-status` | View detailed wallet info |

---

## EVM Verification (Ethereum, Polygon, Base, etc.)

### Setup (Admin)
```
/evm-config add-collection [chain] [contract_address] [role]
```
- `chain`: `eth`, `polygon`, `base`, `optimism`, or `arbitrum`
- `contract_address`: ERC-721 NFT contract address
- `role`: The Discord role to assign

### Example
```
/evm-config add-collection eth 0x...abc @ETH-Holder
```

### User Command
```
/verify-evm
```
User selects the chain and connects their EVM wallet.

---

## TON Verification

### Setup (Admin)
TON verification uses the same configuration pattern:
```
/ton-config add-collection [collection_address] [role]
```

### User Command
```
/verify-ton
```
User connects Tonkeeper or TON Space to verify.

---

## Token Gating (SPL Tokens)

Gate roles by fungible token balance (e.g., $BONK, $USDC).

### Setup (Admin)
```
/token-config add [mint_address] [min_amount] [role]
```

### Example
```
/token-config add EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v 100 @USDC-Holder
```
Users holding 100+ USDC get the role.

---

## Staking Verification

Assign roles to users who stake their assets.

### Setup (Admin)
```
/staking-config add [protocol] [min_stake] [role]
```

Supported staking protocols vary. Contact support for integrations.

---

## Verification Panel

Create a permanent "Verify" button in a channel:
```
/setup-verification-panel [channel]
```

This creates an embed with a button that users can click to start verification.

---

## Troubleshooting

### "I can't receive the verification DM"
Enable **Direct Messages from Server Members** in Discord Privacy Settings.

### "My role wasn't assigned"
1. Check if your wallet holds the required asset.
2. Run `/check-verification` to see your status.
3. Ask an admin to run `/nft-force-check @you`.

### "I sold my NFT but still have the role"
The bot checks periodically. Wait for the next check or ask an admin to force-check.

---

## Quick Links

- [**Add Bot to Server**](https://discord.com/oauth2/authorize?client_id=1432727411698700378&permissions=275683508224&scope=bot%20applications.commands)
- [**Dashboard**](https://azaleagate.com/dashboard)
- [**Support Server**](https://discord.gg/azaleagate)
