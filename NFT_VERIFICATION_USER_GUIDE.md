# NFT Verification User Guide

## Overview

The NFT Verification system allows you to link your Solana wallet to your Discord account and automatically receive roles based on the NFTs you own. This guide will walk you through the verification process and explain how to use the available commands.

## Table of Contents

- [Getting Started](#getting-started)
- [Verifying Your Wallet](#verifying-your-wallet)
- [Checking Your Status](#checking-your-status)
- [Manual Verification Check](#manual-verification-check)
- [Viewing Available Collections](#viewing-available-collections)
- [Unlinking Your Wallet](#unlinking-your-wallet)
- [FAQ](#faq)

## Getting Started

Before you can receive NFT-based roles, you need:

1. A Solana wallet (Phantom, Solflare, or any wallet that can sign messages)
2. NFTs from collections configured by your server administrators
3. Access to the Discord server where NFT verification is enabled

## Verifying Your Wallet

### Step 1: Start Verification

Run the `/verify` command in any channel where the bot is present:

```
/verify
```

### Step 2: Receive Verification Message

The bot will send you a **Direct Message (DM)** containing:
- A unique verification message
- Instructions on how to sign the message
- A button to submit your signature

**Important:** Make sure your DMs are open to receive messages from server members.

### Step 3: Sign the Message

1. Copy the verification message from the DM
2. Open your Solana wallet (Phantom, Solflare, etc.)
3. Find the "Sign Message" feature in your wallet
4. Paste the verification message
5. Sign the message (this does NOT create a blockchain transaction)
6. Copy the signature from your wallet

### Step 4: Submit Your Signature

1. Return to the bot's DM
2. Click the "Submit Signature" button (or use the follow-up interaction)
3. Paste your wallet address and signature
4. Submit

### Step 5: Confirmation

If verification is successful:
- ✅ Your wallet is now linked to your Discord account
- ✅ The bot automatically checks your NFT holdings
- ✅ You receive roles for any NFTs you own from configured collections
- ✅ You'll see a confirmation message with your new roles

If verification fails:
- ❌ You'll receive an error message explaining what went wrong
- ❌ Common issues: invalid signature, wallet already linked to another account
- ❌ Try again or contact a server administrator

## Checking Your Status

To view your current verification status and see which NFTs are granting you roles:

```
/verification-status
```

This will show:
- Your linked wallet address (truncated for privacy)
- When you verified
- When your NFTs were last checked
- Which NFTs are granting you which roles
- How many NFTs you own from each collection

## Manual Verification Check

The bot automatically checks your NFT ownership periodically (usually every 24 hours). However, if you just purchased a new NFT and want to receive the role immediately:

```
/check-verification
```

This will:
- Immediately query the Solana blockchain for your current NFT holdings
- Add any new roles you're eligible for
- Remove roles if you no longer own the required NFTs
- Show you a summary of changes

**Note:** There may be a short cooldown between manual checks to prevent spam.

## Viewing Available Collections

To see which NFT collections grant roles in the server:

```
/nft-collections
```

This displays:
- Collection names and images
- Which role each collection grants
- Whether you own NFTs from each collection
- Collection floor prices (if available)

This helps you understand which NFTs you need to access different parts of the server.

## Unlinking Your Wallet

If you want to remove your wallet link and all NFT-based roles:

```
/unverify
```

This will:
- Remove the link between your wallet and Discord account
- Remove all NFT-based roles
- Require you to verify again if you want to re-link

**Warning:** This action cannot be undone. You'll need to go through the verification process again to re-link your wallet.

## FAQ

### Q: Is it safe to sign the verification message?

**A:** Yes! The verification message is just text and does NOT trigger any blockchain transaction. Signing it only proves you own the wallet's private key. The bot never asks for your private key or seed phrase.

### Q: Can I link multiple wallets?

**A:** No, you can only link one wallet per Discord account per server. If you want to link a different wallet, you must first unlink your current wallet.

### Q: Can I use the same wallet on multiple Discord accounts?

**A:** No, each wallet can only be linked to one Discord account per server. This prevents abuse.

### Q: How often does the bot check my NFT ownership?

**A:** The bot automatically checks periodically (usually every 24 hours). You can also trigger a manual check anytime with `/check-verification`.

### Q: What happens if I sell or transfer my NFT?

**A:** The next time the bot checks your wallet (either automatically or manually), it will detect that you no longer own the NFT and remove the corresponding role.

### Q: What happens if I buy a new NFT?

**A:** The next time the bot checks your wallet, it will detect the new NFT and automatically assign the corresponding role. Use `/check-verification` to get the role immediately.

### Q: I just verified but didn't receive any roles. Why?

**A:** This means you don't currently own any NFTs from the collections configured by the server administrators. Use `/nft-collections` to see which collections grant roles.

### Q: The bot says my wallet is already linked to another account. What do I do?

**A:** This means the wallet you're trying to verify is already linked to a different Discord account. You can only link each wallet to one account. If you believe this is an error, contact a server administrator.

### Q: I'm not receiving the DM from the bot. What should I do?

**A:** Make sure your Discord privacy settings allow DMs from server members:
1. Right-click the server icon
2. Select "Privacy Settings"
3. Enable "Direct Messages"

### Q: Can administrators see my wallet address?

**A:** Administrators can see truncated versions of wallet addresses (e.g., "ABC...XYZ") for verification purposes. Full addresses are stored securely and only used for NFT ownership checks.

### Q: What if the verification fails due to network issues?

**A:** If the Solana network is experiencing issues, the bot will inform you and suggest trying again later. The bot has retry logic and fallback RPC endpoints to minimize disruptions.

### Q: Do I need SOL in my wallet to verify?

**A:** No! Verification does not require any SOL or create any blockchain transactions. You only need to be able to sign a message with your wallet.

## Support

If you encounter issues not covered in this guide:

1. Check the troubleshooting section in the admin documentation
2. Contact a server administrator
3. Make sure you're using the latest version of your wallet software

## Privacy & Security

- Your wallet signature is verified cryptographically
- The bot queries the blockchain directly and cannot be fooled by fake NFTs
- Wallet addresses are hashed in logs for privacy
- The bot never asks for private keys or seed phrases
- Verification messages include timestamps to prevent replay attacks

---

**Remember:** Never share your private key or seed phrase with anyone, including bot administrators!
