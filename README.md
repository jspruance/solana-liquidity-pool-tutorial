# 💧 Add Your Solana Token to a Liquidity Pool — Raydium 2025 Tutorial

A complete, step-by-step guide to making your custom Solana token **tradable** by adding it to a **Raydium liquidity pool**.

This tutorial builds on the previous **Solana Token Tutorial** (non–Token-2022 version).  
If you haven’t yet created your token, complete that guide first before proceeding.

All steps are verified on **Mainnet-Beta** (Raydium does not support Devnet).

---

## 🧩 Step-by-Step Tutorial

---

## ⚙️ 1️⃣ What You’ll Learn

By the end of this tutorial, you’ll know how to:

- Understand what a **liquidity pool** and **AMM** (Automated Market Maker) are  
- Mint your token on **mainnet-beta**  
- Add a **Raydium liquidity pool** pairing your token with **SOL**  
- View your new market on **Dexscreener** and **Jupiter**  
- Give your token its **first real market value**

---

## 💡 2️⃣ What You’ll Need

Before starting, make sure you have:

- ✅ **Phantom Wallet** (set to *Mainnet-Beta*)  
- ✅ **SOL** in your wallet (~0.05 SOL or about $7 for all fees)  
- ✅ Your **mint address** for the token you created earlier  
- ✅ Optional: your token logo and metadata hosted on **Storacha**

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

## 🔍 4️⃣ Verify Your Token on Explorer

Before attaching metadata, let’s make sure your token was created successfully and that your wallet holds the minted supply.

### ✅ Check on Solana Explorer
Visit:
```
https://explorer.solana.com/address/<MINT_ADDRESS>?cluster=mainnet
```

You should see your **Token Mint Account** details, including:
- Total supply (e.g., `1,000,000`)
- Mint authority (your wallet address)
- Decimals (`9`)

Click **“Token Accounts”** and confirm your wallet address appears there as the **owner** with your full balance.

---

### 🧠 Optional CLI Verification
You can also confirm it via Solana CLI:
```bash
spl-token accounts
```

This shows all tokens your wallet holds.  
Look for your mint address and verify the correct balance.

Example output:
```
Token                                         Balance
------------------------------------------------------------
5G2Jf9jP...xyz (MyToken Token)               1000000
```

✅ That confirms your token exists and is in your wallet.

---

### 💡 Tip
If you open Phantom and don’t see your token yet:
- Click “+” → “Import Token” → paste your mint address.
- Or use **Solflare**, which often displays metadata faster.

---

## 🧾 5️⃣ Add Metadata (via Storacha)

You’ll now attach your metadata and image to your token using **Storacha** for decentralized IPFS hosting.

---

### ⚙️ Step 1 — Upload Your Image

1. Visit **[https://storacha.network](https://storacha.network)**.  
2. Upload your **token image** (for example `token.png`).  
3. Copy the **direct image URL**, which will look like this:

```
https://storacha.network/ipfs/<IMAGE_CID>
```

---

### ⚙️ Step 2 — Create `metadata.json`

Create or edit your `metadata.json` file to include the new image URL:

```json
{
  "name": "MyToken Token",
  "symbol": "MTK",
  "description": "Example token created on Solana.",
  "image": "https://storacha.network/ipfs/<IMAGE_CID>",
  "attributes": [
    { "trait_type": "Type", "value": "Utility" }
  ],
  "properties": {
    "files": [
      {
        "uri": "https://storacha.network/ipfs/<IMAGE_CID>",
        "type": "image/png"
      }
    ]
  }
}
```

Save this as `metadata.json`.

---

### ⚙️ Step 3 — Upload the JSON File

Upload your `metadata.json` file to **Storacha** and copy the resulting URL:

```
https://storacha.network/ipfs/<NEW_METADATA_CID>
```

---

### ⚙️ Step 4 — Initialize Metadata On-Chain

Finally, initialize your token’s metadata account on-chain (the first time setup):

```bash
spl-token initialize-metadata <MINT_ADDRESS> "MyToken Token" "MTK" "https://storacha.network/ipfs/<NEW_METADATA_CID>"
```

This creates the metadata account for your token.  
It only needs to be done **once**.

> 💡 **Need to make a change later?**  
> Use:
> ```bash
> spl-token update-metadata <MINT_ADDRESS> "MyToken Token" "MTK" "https://storacha.network/ipfs/<UPDATED_METADATA_CID>"
> ```
> if you upload a new image or JSON in the future.

---

## 💧 6️⃣ What Is a Liquidity Pool?

A **liquidity pool** is a smart contract that holds two tokens — for example, your token + SOL.  
When traders buy or sell one token for the other, the pool automatically updates prices based on supply and demand.

Visualize it like a **bucket** with two sides:
- One side holds your token  
- The other holds SOL  
If someone buys your token, that side empties slightly and the price rises.

This system is known as an **Automated Market Maker (AMM)**.  
Instead of matching buyers and sellers like a traditional order book, an AMM uses simple math (`x × y = k`) to keep the pool balanced and determine prices automatically.

---

## ⚖️ 7️⃣ Choosing a Pairing Token

You can pair with **SOL** or **USDC**, but here’s the difference:

| Pair | Pros | Notes |
|------|------|-------|
| **SOL** | Most common ✅ | Best visibility, simple setup, higher volume |
| **USDC** | Stable reference price | Slightly more setup, fewer pools |

> 💡 For this tutorial, we’ll use **SOL** — it’s simpler, more liquid, and widely supported.

---

## 🧭 8️⃣ Add Liquidity on Raydium

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

## 📊 9️⃣ Verify Your Pool

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

🧠 **Author:** BlockExplorer  
📅 **Updated:** October 2025  
📘 **Version:** Raydium Liquidity Pool Tutorial Edition
