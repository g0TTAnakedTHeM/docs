# Solana NFT Verification System

## Overview

The Solana NFT Verification system is a comprehensive Discord bot feature that enables role-gating based on Solana NFT ownership. Users can link their Solana wallets to their Discord accounts, and the bot automatically assigns roles based on which NFTs they hold. The system includes periodic verification to ensure users still own the required NFTs.

## Key Features

- 🔐 **Secure Wallet Verification** - Cryptographic signature verification without blockchain transactions
- 🎨 **NFT-Based Role Assignment** - Automatically assign roles based on NFT collection ownership
- 🔄 **Periodic Verification** - Automatic checks to ensure users still own their NFTs
- ⚡ **Manual Checks** - Users can trigger immediate verification after purchasing NFTs
- 📊 **Admin Dashboard** - Statistics and monitoring tools for server administrators
- 🛡️ **Anti-Fraud Protection** - Prevents wallet reuse and signature replay attacks
- 🌐 **Multi-Collection Support** - Configure multiple NFT collections with different roles
- 📱 **User-Friendly Commands** - Simple slash commands for all interactions

## Documentation

### For Users

**[User Guide](./NFT_VERIFICATION_USER_GUIDE.md)** - Complete guide for Discord users
- How to verify your wallet
- Checking your verification status
- Manual verification checks
- Viewing available collections
- Unlinking your wallet
- FAQ and common questions

### For Administrators

**[Admin Guide](./NFT_VERIFICATION_ADMIN_GUIDE.md)** - Complete guide for server administrators
- Initial setup and configuration
- Configuring NFT collections
- Managing verification settings
- Monitoring and statistics
- Admin commands reference
- Best practices
- Troubleshooting

**[RPC Setup Guide](./NFT_VERIFICATION_RPC_SETUP.md)** - Solana RPC configuration
- Understanding RPC endpoints
- RPC provider comparison (Helius, QuickNode, Alchemy)
- Step-by-step setup instructions
- API key configuration
- Rate limits and optimization
- Cost estimation

**[Troubleshooting Guide](./NFT_VERIFICATION_TROUBLESHOOTING.md)** - Comprehensive problem-solving
- Quick diagnostics
- User issues and solutions
- Admin issues and solutions
- Technical issues and solutions
- Performance optimization
- Error messages reference

## Quick Start

### For Users

1. Run `/verify` in any channel
2. Check your DMs for the verification message
3. Sign the message with your Solana wallet
4. Submit your signature
5. Receive your NFT-based roles automatically!

### For Administrators

1. Ensure bot has `MANAGE_ROLES` permission
2. Enable NFT verification: `/nft-config enable`
3. Add NFT collections: `/nft-config add-collection <address> <role>`
4. Set check interval: `/nft-config set-check-interval 24`
5. Configure RPC endpoint (see RPC Setup Guide)

## Command Reference

### User Commands

| Command | Description |
|---------|-------------|
| `/verify` | Link your Solana wallet to Discord |
| `/unverify` | Unlink your wallet and remove NFT roles |
| `/check-verification` | Manually check your NFT ownership |
| `/verification-status` | View your verification status |
| `/nft-collections` | View available NFT collections |

### Admin Commands

| Command | Description | Permission |
|---------|-------------|------------|
| `/nft-config enable` | Enable NFT verification | MANAGE_ROLES |
| `/nft-config disable` | Disable NFT verification | MANAGE_ROLES |
| `/nft-config add-collection` | Add NFT collection | MANAGE_ROLES |
| `/nft-config remove-collection` | Remove NFT collection | MANAGE_ROLES |
| `/nft-config list-collections` | List all collections | MANAGE_ROLES |
| `/nft-config set-check-interval` | Set check frequency | MANAGE_ROLES |
| `/nft-stats` | View verification statistics | MANAGE_ROLES |
| `/nft-force-check @user` | Force check for user | MANAGE_ROLES |
| `/verification-status @user` | View user's status | MANAGE_ROLES |
| `/nft-unlink @user` | Unlink user's wallet | MANAGE_ROLES |

## Architecture

### System Components

```
┌─────────────────┐
│  Discord User   │
└────────┬────────┘
         │ Commands
         ▼
┌─────────────────────────────────────────┐
│         Discord Bot (Node.js)           │
│  ┌───────────────────────────────────┐  │
│  │   NFT Verification Service        │  │
│  │   - Wallet verification           │  │
│  │   - NFT ownership checks          │  │
│  │   - Role management               │  │
│  └──────────┬────────────────────────┘  │
│             │                            │
│  ┌──────────▼────────────────────────┐  │
│  │   Solana Service                  │  │
│  │   - RPC communication             │  │
│  │   - Signature verification        │  │
│  │   - NFT metadata fetching         │  │
│  └──────────┬────────────────────────┘  │
│             │                            │
│  ┌──────────▼────────────────────────┐  │
│  │   Verification Storage            │  │
│  │   - Wallet mappings               │  │
│  │   - Collection configs            │  │
│  └───────────────────────────────────┘  │
└─────────────┬───────────────────────────┘
              │
              ▼
┌─────────────────────────────────────────┐
│         Solana Blockchain               │
│   - RPC Endpoint (Helius/QuickNode)    │
│   - NFT Metadata (Metaplex)            │
└─────────────────────────────────────────┘
```

### Verification Flow

1. **User Initiates**: User runs `/verify` command
2. **Message Generation**: Bot generates unique verification message with timestamp and nonce
3. **User Signs**: User signs message with their Solana wallet (off-chain)
4. **Signature Verification**: Bot verifies signature cryptographically
5. **NFT Query**: Bot queries Solana blockchain for user's NFTs
6. **Role Assignment**: Bot assigns roles based on NFT ownership
7. **Confirmation**: User receives confirmation with assigned roles

### Periodic Verification

1. **Scheduled Job**: Runs every N hours (configurable, default 24)
2. **User Iteration**: Bot checks all verified users in the server
3. **NFT Query**: Queries current NFT holdings for each user
4. **Role Update**: Adds/removes roles based on changes
5. **Logging**: Logs summary to admin channel

## Technical Details

### Dependencies

```json
{
  "@solana/web3.js": "^1.95.0",
  "@metaplex-foundation/js": "^0.20.0",
  "bs58": "^5.0.0",
  "node-cron": "^3.0.3"
}
```

### Environment Variables

```env
# Required
DISCORD_TOKEN=your_discord_bot_token
SOLANA_RPC_ENDPOINT=https://mainnet.helius-rpc.com/?api-key=YOUR_KEY

# Optional
HELIUS_API_KEY=your_helius_api_key
SOLANA_RPC_FALLBACK=https://backup-endpoint.com
```

### Storage Format

Verification data is stored in server configuration files:

```json
{
  "serverId": "123456789",
  "nftVerification": {
    "enabled": true,
    "checkIntervalHours": 24,
    "collections": [
      {
        "address": "ABC123...",
        "roleId": "987654321",
        "name": "Cool NFT Collection",
        "addedAt": "2025-01-01T00:00:00Z"
      }
    ],
    "verifiedUsers": [
      {
        "discordId": "111222333",
        "walletAddress": "XYZ789...",
        "verifiedAt": "2025-01-01T00:00:00Z",
        "lastChecked": "2025-01-02T00:00:00Z"
      }
    ]
  }
}
```

## Security

### Cryptographic Verification

- Uses `@solana/web3.js` for signature verification
- No blockchain transactions required
- Timestamp validation prevents replay attacks
- Nonce ensures message uniqueness

### Wallet Linking Security

- One wallet per Discord account per server
- Prevents wallet reuse across accounts
- Wallet addresses hashed in logs
- Secure storage of verification data

### Admin Controls

- Requires `MANAGE_ROLES` permission
- Audit logging of all configuration changes
- Rate limiting on verification attempts
- Monitoring for suspicious patterns

## Performance

### Caching Strategy

- Collection metadata: 24 hours
- NFT ownership: Configurable (default 1 hour)
- RPC responses: 5 minutes
- Invalidated on manual checks

### Rate Limiting

- Helius free tier: 100 req/sec
- QuickNode free tier: 25 req/sec
- Automatic request queuing
- Staggered periodic checks

### Scalability

- Supports multiple servers
- Handles 1000+ verified users per server
- Efficient database queries
- Async processing for bulk operations

## Best Practices

### For Administrators

1. **Start with Helius free tier** - Best balance of features and limits
2. **Set 24-hour check interval** - Good balance of freshness and performance
3. **Keep bot role above NFT roles** - Required for role management
4. **Monitor statistics regularly** - Use `/nft-stats` to track usage
5. **Document your collections** - Keep a list of which NFTs grant which roles
6. **Test before full deployment** - Use test roles first
7. **Communicate with users** - Share the user guide with your community

### For Users

1. **Enable DMs** - Required to receive verification messages
2. **Sign within 10 minutes** - Verification messages expire
3. **Copy signatures carefully** - Include entire signature string
4. **Use manual checks** - After purchasing new NFTs for immediate roles
5. **Keep wallet secure** - Never share private keys or seed phrases

## Troubleshooting Quick Reference

| Issue | Quick Fix |
|-------|-----------|
| Can't receive DM | Enable DMs in server privacy settings |
| Invalid signature | Start fresh with `/verify` and copy entire message |
| Wallet already linked | Unlink from other account first |
| No roles received | Check `/nft-collections` to see configured collections |
| Roles not updating | Run `/check-verification` for immediate update |
| Bot can't assign roles | Check bot role is above NFT roles |
| RPC errors | Verify API key in `.env` file |
| Slow verification | Upgrade to paid RPC tier |

See the [Troubleshooting Guide](./NFT_VERIFICATION_TROUBLESHOOTING.md) for detailed solutions.

## Support

### Documentation

- [User Guide](./NFT_VERIFICATION_USER_GUIDE.md) - For Discord users
- [Admin Guide](./NFT_VERIFICATION_ADMIN_GUIDE.md) - For server administrators
- [RPC Setup Guide](./NFT_VERIFICATION_RPC_SETUP.md) - For RPC configuration
- [Troubleshooting Guide](./NFT_VERIFICATION_TROUBLESHOOTING.md) - For problem-solving

### External Resources

- **Solana Status**: https://status.solana.com
- **Helius Docs**: https://docs.helius.dev
- **QuickNode Docs**: https://www.quicknode.com/docs/solana
- **Solana RPC API**: https://docs.solana.com/api

### Getting Help

1. Check the relevant documentation guide
2. Review the troubleshooting guide
3. Check bot logs for specific errors
4. Test with diagnostic commands
5. Contact server administrators
6. Report bugs to bot developer

## FAQ

### General Questions

**Q: Is it safe to sign the verification message?**
A: Yes! The message is just text and does NOT trigger any blockchain transaction. The bot never asks for your private key.

**Q: How often does the bot check my NFTs?**
A: Automatically every 24 hours (configurable by admins). You can also trigger manual checks anytime.

**Q: Can I link multiple wallets?**
A: No, one wallet per Discord account per server.

**Q: What happens if I sell my NFT?**
A: The bot will automatically remove the corresponding role during the next check.

**Q: Do I need SOL in my wallet?**
A: No! Verification doesn't require any SOL or create transactions.

### Technical Questions

**Q: Which RPC provider should I use?**
A: Helius is recommended for most users. Free tier is generous and includes DAS API.

**Q: How much does it cost?**
A: Free for small servers. Larger servers may need paid RPC plans ($50-200/month).

**Q: Can the bot handle multiple servers?**
A: Yes, the bot supports multiple servers with independent configurations.

**Q: What if Solana network is down?**
A: The bot has retry logic and fallback endpoints. Verification may be slower but will work.

## Changelog

### Version 1.0.0 (Current)

- ✅ Wallet verification with signature
- ✅ NFT-based role assignment
- ✅ Periodic verification checks
- ✅ Manual verification triggers
- ✅ Admin configuration commands
- ✅ Statistics and monitoring
- ✅ Multi-collection support
- ✅ Security features (anti-replay, wallet reuse prevention)
- ✅ Comprehensive error handling
- ✅ Rate limiting and optimization

### Planned Features

- 🔜 Trait-based role assignment (specific NFT attributes)
- 🔜 Minimum NFT count requirements
- 🔜 Staking verification (check if NFTs are staked)
- 🔜 Multi-wallet support per user
- 🔜 Verification badges/achievements
- 🔜 Analytics dashboard
- 🔜 Webhook notifications

## Contributing

Found a bug or have a feature request? Please report it to the bot developer with:

- Detailed description of the issue
- Steps to reproduce
- Expected vs actual behavior
- Bot logs (if applicable)
- Screenshots (if helpful)

## License

This feature is part of the Discord moderation bot. See main bot documentation for license information.

---

**Version**: 1.0.0  
**Last Updated**: November 2025  
**Maintained By**: Bot Development Team

For the latest updates and documentation, check the bot's repository or documentation site.
