# 💧 Add Your Solana Token to a Liquidity Pool — Raydium 2025 Tutorial

A complete, step-by-step guide to making your custom Solana token **tradable** by adding it to a **Raydium liquidity pool**.

This tutorial builds on the previous **Token-2022 Solana Token Tutorial**.  
If you haven’t yet created your token, complete that guide first before proceeding.

All steps are verified on **Mainnet-Beta** (Raydium does not support Devnet).

---

## 🧩 Step-by-Step Tutorial

---

## ⚙️ 1️⃣ What You’ll Learn

By the end of this tutorial, you’ll know how to:

- Understand what a **liquidity pool** and **AMM** (Automated Market Maker) are  
- Mint your token on **mainnet-beta** using the Token-2022 program  
- Add a **Raydium liquidity pool** pairing your token with **SOL**  
- View your new market on **Dexscreener** and **Jupiter**  
- Give your token its **first real market value**

---

## 💡 2️⃣ What You’ll Need

Before starting, make sure you have:

- ✅ **Phantom Wallet** (set to *Mainnet-Beta*)  
- ✅ **SOL** in your wallet (~0.05 SOL or about $7 for all fees)  
- ✅ Your **mint address** for the token you created earlier  
- ✅ Optional: your token logo and metadata hosted on Storacha  

---

## 🪙 3️⃣ Mint Your Token on Mainnet-Beta

If your existing token was created on **Devnet**, it won’t appear on Raydium.  
You’ll need to mint a real token on mainnet first.

```bash
solana config set --url https://api.mainnet-beta.solana.com
```

Create or use a clean mainnet wallet:

```bash
solana-keygen new --outfile ~/.config/solana/mainnet.json
solana config set --keypair ~/.config/solana/mainnet.json
```

Fund it with a small amount of SOL from an exchange.

---

### 🧱 Mint tokens

#### Create your token mint
```bash
spl-token create-token --program-id TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb --enable-metadata --decimals 9
```

#### Create a token account for your wallet
```bash
spl-token create-account <MINT_ADDRESS>
```

#### Mint your initial supply
```bash
spl-token mint <MINT_ADDRESS> 1000000
```

---

## 🧾 4️⃣ Add Metadata (via Storacha)

You’ll now attach your metadata and image to your token using **Storacha** for decentralized IPFS hosting.

### 🅱 Option — Storacha (Fast Decentralized Gateway)

1. Visit https://storacha.network  
2. Click **Upload Folder** and select your `metadata` directory (which includes your `metadata.json` and logo image).  
3. After upload, Storacha will return a **CID** (e.g. `bafkreihxyz789...`).  
4. Your hosted files will be accessible at:  

```
https://storacha.network/ipfs/<FOLDER_CID>/metadata.json
https://storacha.network/ipfs/<FOLDER_CID>/mytoken-logo.png
```

> 🧠 Tip: You can use Pinata or Storacha interchangeably — both host content on IPFS.

Attach the metadata to your token:

```bash
spl-token initialize-metadata <MINT_ADDRESS> "MyToken Token" "MTK" "https://storacha.network/ipfs/<FOLDER_CID>/metadata.json"
```

Confirm your token appears in Phantom (Mainnet) with name and logo.

---

## 💧 5️⃣ What Is a Liquidity Pool?

A **liquidity pool** is a smart contract that holds two tokens — for example, your token + SOL.  
When traders buy or sell one token for the other, the pool automatically updates prices based on supply and demand.

Visualize it like a **bucket** with two sides:
- One side holds your token  
- The other holds SOL  
If someone buys your token, that side empties slightly and the price rises.

This system is known as an **Automated Market Maker (AMM)**.  
Instead of matching buyers and sellers like a traditional order book, an AMM uses simple math (`x × y = k`) to keep the pool balanced and determine prices automatically.

---

## ⚖️ 6️⃣ Choosing a Pairing Token

You can pair with **SOL** or **USDC**, but here’s the difference:

| Pair | Pros | Notes |
|------|------|-------|
| **SOL** | Most common  ✅ | Best visibility, simple setup, higher volume |
| **USDC** | Stable reference price | Slightly more setup, fewer pools |

> 💡 For this tutorial, we’ll use **SOL** — it’s simpler, more liquid, and widely supported.

---

## 🧭 7️⃣ Add Liquidity on Raydium

1. Visit **[https://raydium.io/liquidity](https://raydium.io/liquidity)**  
2. Connect your **Phantom wallet** (Mainnet)  
3. Click **Add Liquidity**  
4. In the first field, paste your **token mint address**  
5. In the second field, choose **SOL**  
6. Enter amounts — for example:  
   - 0.02 SOL  
   - 200 of your token  
7. Click **Add Liquidity** → Approve both transactions in Phantom  

Once confirmed, Raydium will create your new **LP token** representing your share of the pool.

---

## 📊 8️⃣ Verify Your Pool

After a minute or two, check:

### ✅ On Dexscreener
Go to:
```
https://dexscreener.com/solana/<YOUR_TOKEN_MINT>
```

You should see a live chart showing your token paired with SOL.

### ✅ On Jupiter
Visit:
```
https://jup.ag/swap/<YOUR_TOKEN_MINT>-So11111111111111111111111111111111111111112
```

Your token is now **tradable** on the Solana DEX network!

---

## 💰 9️⃣ Understanding Pool Math

Raydium uses a **constant-product AMM formula**:

```
x * y = k
```

Where:
- **x** = amount of your token  
- **y** = amount of SOL  
- **k** = constant (pool size)

If traders buy your token, *x* decreases, *y* increases, and the price rises automatically.  
If they sell, the opposite happens — price drops.

No central exchange — it’s pure on-chain math.

---

## ⚠️ 🔟 Tips & Best Practices

- 🧱 Start small — 0.01–0.02 SOL is enough for testing  
- 💸 You can remove liquidity anytime to get your tokens back  
- 🧮 The ratio you deposit sets the **starting price**  
- 🪙 Keep your supply realistic — over-minted tokens lose credibility  
- 🔍 Check your pool regularly using Raydium or Dexscreener  

---

## 💵 1️⃣1️⃣ Typical Mainnet Costs

| Step | SOL (Approx) | USD (@ $150 / SOL) |
|------|---------------|--------------------|
| Token mint + metadata | 0.007 – 0.012 | $1 – $2 |
| Raydium pool creation | 0.010 – 0.020 | $1.50 – $3 |
| Misc tx | 0.002 | $0.30 |
| **Total (fees only)** | **≈ 0.02 – 0.03 SOL** | **≈ $3 – $5** |

Your liquidity deposit (e.g., 0.02 SOL) is not a fee — it remains in the pool.

---

## 🧾 1️⃣2️⃣ Verify & Share

Once your pool is live:

- 🔗 Share your **Dexscreener** or **Jupiter** link  
- 🖼 Submit your token logo to the [Solana Token List](https://github.com/solana-labs/token-list) for wallet recognition  
- 🪙 Add your token manually in Phantom if it doesn’t auto-detect  

---

## 🚀 1️⃣3️⃣ Next Steps

Your token now has real market value!  
From here you can:

- 📈 Track your pool stats on **Dexscreener**, **Birdeye**, or **Meteora**  
- 🤖 Build a trading bot or analytics script using **Python** or **TypeScript**  
- 🧠 Learn how to create **staking** or **farming** contracts for your token  

---

## ✅ Quick Reference Summary

| Step | Command / Action | Purpose |
|------|------------------|----------|
| 1 | `solana config set --url https://api.mainnet-beta.solana.com` | Switch to Mainnet-Beta |
| 2 | `solana-keygen new` | Create mainnet wallet |
| 3 | `spl-token create-token --program-id TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb --enable-metadata --decimals 9` | Create Token-2022 mint |
| 4 | `spl-token create-account <MINT_ADDRESS>` | Create token account |
| 5 | `spl-token mint <MINT_ADDRESS> 1000000` | Mint supply |
| 6 | `spl-token initialize-metadata <MINT_ADDRESS> "MyToken Token" "MTK" "https://storacha.network/ipfs/<FOLDER_CID>/metadata.json"` | Attach metadata |
| 7 | **Raydium UI → Add Liquidity** | Create pool (token + SOL) |
| 8 | **Dexscreener / Jupiter** | Verify pool is live |

---

🧠 **Author:** BlockExplorer  
📅 **Updated:** October 2025  
📘 **Version:** Raydium Liquidity Pool Tutorial Edition
