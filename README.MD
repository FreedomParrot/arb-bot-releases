# 🚀 Kamino Flash Loan Arbitrage Bot

An automated arbitrage trading bot that uses Kamino Finance flash loans and Jupiter DEX swaps to capture profit opportunities on Solana.

## ⚠️ IMPORTANT: FEE STRUCTURE

**This bot automatically sends 15% of all profits to the developer wallet:**
```
HY8hPLeiRQ4swJFLhLFLg6XwrMFTuFctdWJN7WWxWs6U
```

This fee is **hardcoded** and **cannot be removed**. By using this bot, you agree to this fee structure.

---

## 📋 Features

- ✅ **Atomic Flash Loan Arbitrage**: Borrow → Swap → Swap → Repay in a single transaction
- ✅ **Zero Capital Required**: Uses Kamino Finance flash loans (no collateral needed)
- ✅ **Automated Profit Detection**: Continuously scans for arbitrage opportunities
- ✅ **MEV Protection**: Uses private RPC endpoints
- ✅ **Configurable Slippage & Fees**: Customize your trading parameters
- ✅ **Supports SOL and USDC**: Can arbitrage between any supported token pairs

---

## 🛠️ Installation

### Option 1: Use Pre-compiled Executable (Easiest)

Download the executable for your platform from the `dist/` folder:

- **Linux**: `arb-bot-linux`
- **macOS**: `arb-bot-macos`  
- **Windows**: `arb-bot-win.exe`

Make it executable (Linux/Mac only):
```bash
chmod +x arb-bot-linux
```

### Option 2: Build from Source

```bash
# Install dependencies
npm install

# Build the project
npm run build

# Create executables (optional)
./build.sh
```

---

## 🔑 Setup Your Wallet

1. **Create a new Solana wallet** (recommended for security):
```bash
solana-keygen new --outfile bot-wallet.json
```

2. **Fund your wallet** with:
   - **SOL**: For transaction fees (~0.1 SOL recommended)
   - **USDC or SOL**: Depending on which token you want to arbitrage with

3. **Check your balance**:
```bash
./arb-bot-linux check-balance -k bot-wallet.json
```

---

## 🚀 Usage

### Basic Arbitrage (SOL ↔ USDC)

```bash
./arb-bot-linux simple-jupiter-arb \
  -k bot-wallet.json \
  -m1 So11111111111111111111111111111111111111112 \
  -m2 EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v \
  -a 1
```

### With Custom Parameters

```bash
./arb-bot-linux simple-jupiter-arb \
  -k bot-wallet.json \
  -m1 So11111111111111111111111111111111111111112 \
  -m2 EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v \
  -a 5 \
  -p 0.01 \
  -s 300 \
  -c 50000
```

---

## 📖 Command Reference

### `simple-jupiter-arb`

Main arbitrage command.

**Required Options:**
- `-k, --keypair <path>` - Path to your wallet keypair JSON file
- `-m1, --token-mint1 <address>` - First token mint (the token you flash borrow)
- `-m2, --token-mint2 <address>` - Second token mint (the token you swap to/from)
- `-a, --amount <number>` - Flash loan amount in tokens (not lamports)

**Optional Parameters:**
- `-p, --min-profit <number>` - Minimum profit required in tokens (default: any profit)
- `-s, --slippage-bps <number>` - Slippage tolerance in basis points (default: 50 bps = 0.5%)
- `-c, --compute-unit-price <number>` - Priority fee in micro-lamports (default: 10000)

**Example:**
```bash
# Borrow 10 SOL, swap to USDC and back, require 0.05 SOL minimum profit
./arb-bot-linux simple-jupiter-arb \
  -k wallet.json \
  -m1 So11111111111111111111111111111111111111112 \
  -m2 EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v \
  -a 10 \
  -p 0.05 \
  -s 300 \
  -c 50000
```

### `check-balance`

Check your wallet's SOL and token balances.

**Options:**
- `-k, --keypair <path>` - Path to your wallet keypair JSON file

**Example:**
```bash
./arb-bot-linux check-balance -k wallet.json
```

---

## 🪙 Supported Tokens

Currently supports tokens with Kamino lending reserves:

| Token | Mint Address |
|-------|-------------|
| **SOL** | `So11111111111111111111111111111111111111112` |
| **USDC** | `EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v` |

---

## 💰 Fee Structure

### Kamino Flash Loan Fee
- **0.001%** (1 basis point) of the borrowed amount
- Example: Borrow 100 SOL → Pay 0.001 SOL flash loan fee

### Developer Fee (YOU)
- **15%** of net profit after flash loan fees
- Automatically sent to: `HY8hPLeiRQ4swJFLhLFLg6XwrMFTuFctdWJN7WWxWs6U`
- **Cannot be disabled**

### Transaction Fees
- Solana network fees (~0.00001 SOL per transaction)
- Priority fees (configurable with `-c` parameter)

---

## 📊 Example Profit Calculation

```
Flash Loan Amount:     100 SOL
After Buy Swap:        1,500 USDC
After Sell Swap:       101 SOL
Flash Loan Fee:        0.001 SOL (0.001%)
Repayment Amount:      100.001 SOL

Gross Profit:          0.999 SOL
Developer Fee (15%):   0.15 SOL
Your Net Profit:       0.849 SOL ✅
```

---

## ⚙️ Configuration Tips

### Slippage Settings
- **Low volatility**: 50-100 bps (0.5-1%)
- **Medium volatility**: 200-500 bps (2-5%)
- **High volatility**: 500-1000 bps (5-10%)

### Priority Fees
- **Low traffic**: 10,000 micro-lamports
- **Medium traffic**: 50,000 micro-lamports
- **High traffic**: 100,000+ micro-lamports

### Flash Loan Amount
- Start small (1-10 tokens) to test
- Larger amounts = higher potential profit but more price impact
- Monitor logs for "High price impact" warnings

---

## 🔍 Understanding the Output

```
⚡ PROFIT! 999000 lamports (0.000999 tokens, 0.9990%) - EXECUTING...
   Borrow: 100.000000
   Repay:  100.001000
   After:  100.999000
   15% Fee: 0.000150 tokens
   Net Profit: 0.000849 tokens
   Price impact: 0.0850%
   Profit margin: 998 bps

✅ ARBITRAGE SUCCESS!
   Arb TX: https://solscan.io/tx/[transaction_id]
   
💰 Sent 15% fee: 0.000150 SOL to HY8hPLeiRQ4swJFLhLFLg6XwrMFTuFctdWJN7WWxWs6U
   Fee TX: https://solscan.io/tx/[fee_transaction_id]
```

### Key Metrics:
- **Borrow**: Amount flash borrowed
- **Repay**: Amount to repay (including 0.001% fee)
- **After**: Amount received after both swaps
- **15% Fee**: Developer fee (15% of profit)
- **Net Profit**: Your profit after all fees
- **Price Impact**: How much your trade affects market prices
- **Profit Margin**: Profit as basis points (bps)

---

## ❌ Common Errors

### "No Kamino reserve for [address]"
**Solution**: You're trying to use an unsupported token. Only SOL and USDC are currently supported.

### "Slippage exceeded"
**Solution**: Increase slippage with `-s` parameter (e.g., `-s 500` for 5%)

### "No profit"
**Solution**: No arbitrage opportunity exists at current prices. The bot will keep scanning.

### "High price impact"
**Solution**: Reduce flash loan amount with `-a` parameter

### "Insufficient balance"
**Solution**: Add more SOL to your wallet for transaction fees

---

## 🔒 Security Best Practices

1. **Use a dedicated wallet** - Don't use your main wallet
2. **Start with small amounts** - Test with 1-10 tokens first
3. **Monitor regularly** - Check bot logs and your wallet balance
4. **Secure your keypair** - Never share your `wallet.json` file
5. **Use a VPS** - Run 24/7 on a reliable server for best results

---

## 🐛 Troubleshooting

### Bot keeps saying "No buy route" or "No sell route"
- Market conditions aren't favorable
- Try different token amounts
- Wait for better liquidity

### Bot finds profit but transactions fail
- Increase priority fee: `-c 100000`
- Increase slippage: `-s 500`
- Reduce loan amount: `-a 5`

### Bot stops after multiple failures
- This is normal - it pauses for 30 seconds after 5 consecutive failures
- Check your RPC connection
- Ensure you have enough SOL for fees

---

## 📈 Performance Tips

1. **Use a fast RPC** - The default RPC is optimized, but you can use your own
2. **Run on a VPS** - Low latency to Solana validators is crucial
3. **Monitor gas prices** - Increase priority fees during high traffic
4. **Start conservative** - Use small amounts and low slippage initially
5. **Scale gradually** - Once profitable, slowly increase loan amounts

---

## 📞 Support

For issues or questions:
- Check the [Troubleshooting](#troubleshooting) section
- Review the [Common Errors](#common-errors) section
- Ensure you're using the latest version

---

## ⚖️ Legal Disclaimer

This software is provided "as is" without warranty of any kind. Trading cryptocurrencies carries significant risk of loss. You are solely responsible for:

- Your trading decisions
- Securing your private keys
- Complying with local regulations
- Any financial losses incurred

The 15% developer fee is non-negotiable and automatically deducted from all profits.

---

## 📜 License

MIT License - See LICENSE file for details

---

**Built with ❤️ for the Solana ecosystem**

---

## Quick Start Checklist

- [ ] Downloaded the correct executable for your OS
- [ ] Created a new wallet with `solana-keygen`
- [ ] Funded wallet with 0.1+ SOL for fees
- [ ] Tested with `check-balance` command
- [ ] Started with small amount (1-5 tokens)
- [ ] Monitored first successful trade
- [ ] Verified 15% fee was sent to developer wallet
- [ ] Scaled up gradually after success

**Happy arbitraging! 🚀**