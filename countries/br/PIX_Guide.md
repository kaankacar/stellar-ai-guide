# PIX Guide: Brazil Rails on Stellar

This guide explains the practical PIX path for this Brazil hackathon repo.

The recommended self-service route is:

```text
BRL via PIX -> Etherfuse on-ramp -> TESOURO on Stellar
TESOURO on Stellar -> Etherfuse off-ramp -> BRL via PIX
```

Use this guide when you want a Brazilian real payment flow in a Stellar app without waiting for custom sandbox access from every local provider.


## 1. What PIX Means Here

PIX is Brazil's instant payment system. In this repo, PIX is not a Stellar protocol by itself. It is the fiat rail used by a local anchor/provider to move BRL in or out of a user's bank account.

For this guide:

| Layer | What to use |
|---|---|
| Fiat rail | PIX |
| Anchor / API provider | Etherfuse |
| Stellar asset | TESOURO |
| Testnet acquisition path | Etherfuse sandbox on-ramp |
| Production-style flow | BRL via PIX <-> TESOURO on Stellar |

Alfred Pay, Abroad Finance, and Transfero are relevant Brazil ecosystem references, but do not assume the same self-service API flow unless you have current credentials and docs from those teams.


## 2. Quick Setup

Get these values before writing code:

```bash
ETHERFUSE_API_BASE_URL="https://api.sand.etherfuse.com"
ETHERFUSE_API_KEY="your-etherfuse-sandbox-key"
STELLAR_NETWORK="testnet"
STELLAR_RPC_URL="https://soroban-testnet.stellar.org"
TESOURO_ASSET_CODE="TESOURO"
TESOURO_TESTNET_ISSUER="GC3CW7EDYRTWQ635VDIGY6S4ZUF5L6TQ7AA4MWS7LEQDBLUSZXV7UPS4"
```

Get an Etherfuse sandbox key at:

```text
https://devnet.etherfuse.com/ramp
```

Etherfuse auth is unusual:

```http
Authorization: your-api-key
```

Do not use `Bearer`.


## 3. Before Calling the Ramp API

Each end-user needs stable IDs:

| ID | Who creates it | How to use it |
|---|---|---|
| `customerId` | Your app | Generate once per user, store permanently, reuse forever |
| `bankAccountId` | Your app | Generate once per user/bank account, store permanently, reuse forever |
| Stellar wallet `G...` address | Your app or user's wallet | Use a classic Stellar address, not a Soroban `C...` contract address |

Do not generate a new `customerId` or `bankAccountId` on every session. Etherfuse binds these IDs on first use. Re-registering the same Stellar public key under a different customer can fail, even in sandbox.

If your app uses passkey smart wallets with `C...` contract addresses, use a fee-payer or linked classic `G...` address as the Etherfuse wallet identity.


## 4. Onboard the Customer

Before a quote can become an order, the user needs an Etherfuse customer record. The hosted onboarding path creates a presigned URL for the user to complete the sandbox flow.

```http
POST /ramp/onboarding-url
```

Live-tested sandbox request shape:

```typescript
{
  customerId,        // UUID you generate
  bankAccountId,    // UUID you generate
  publicKey,        // G... Stellar address
  blockchain: "stellar",
  accountType: "business"
}
```

The response includes `presigned_url`. Send the user to that URL. In sandbox, fake data is accepted, but the hosted flow still has to be completed before order creation works reliably. If you skip this step, quotes can still work, but `POST /ramp/order` can fail with `Proxy account not found`.


## 5. Discover the TESOURO Asset

Testnet TESOURO:

```text
TESOURO:GC3CW7EDYRTWQ635VDIGY6S4ZUF5L6TQ7AA4MWS7LEQDBLUSZXV7UPS4
```

You can also discover it through the assets endpoint. In live sandbox testing, this request returned TESOURO:

```http
GET /ramp/assets?blockchain=stellar&currency=brl&wallet=<G_ADDRESS>
```

The response shape was:

```json
{
  "assets": [
    {
      "symbol": "TESOURO",
      "identifier": "TESOURO:GC3CW7EDYRTWQ635VDIGY6S4ZUF5L6TQ7AA4MWS7LEQDBLUSZXV7UPS4",
      "currency": "brl"
    }
  ]
}
```

Do not rely on testnet DEX liquidity to acquire TESOURO. The reliable testnet path is the Etherfuse on-ramp flow. Treat the on-ramp as the TESOURO faucet.

For Stellar onramps, Etherfuse docs describe a claim flow: pass `walletAddress` in the quote request, and if the wallet lacks the trustline or does not exist on-chain yet, the quote fee can include a one-time onboarding cost. After the order completes, the response can include a `stellarClaimTransaction` unsigned XDR for the user to sign. For offramps and swaps, assume the wallet must already be funded and have the required trustline.


## 6. On-Ramp Flow: BRL via PIX -> TESOURO

Use this when a user wants to deposit BRL and receive TESOURO on Stellar.

### Step 1: Create or Reuse User Records

Your app should have:

```text
user.id
user.customerId
user.bankAccountId
user.stellarPublicKey
```

Persist these values in your database.

### Step 2: Request an On-Ramp Quote

Use `POST /ramp/quote`.

Live-tested request shape:

```typescript
{
  quoteId: crypto.randomUUID(),
  customerId: user.customerId,
  blockchain: "stellar",
  walletAddress: user.stellarPublicKey,
  quoteAssets: {
    type: "onramp",
    sourceAsset: "BRL",
    targetAsset: "TESOURO:GC3CW7EDYRTWQ635VDIGY6S4ZUF5L6TQ7AA4MWS7LEQDBLUSZXV7UPS4"
  },
  sourceAmount: "10"
}
```

The quote response uses fields such as `sourceAmount`, `destinationAmount`, `exchangeRate`, `feeBps`, `feeAmount`, `expiresAt`, and `requiresSwap`.

Quotes expire quickly. Design the UI so users create the order soon after accepting the quote.

### Step 3: Create the Order

Use the singular endpoint:

```http
POST /ramp/order
```

Do not accidentally use `POST /ramp/orders`. That plural endpoint returns the order list and can make it look like the API ignored your request body.

The create-order call requires the IDs from your quote and user setup, including:

```typescript
{
  orderId: crypto.randomUUID(),
  bankAccountId: user.bankAccountId,
  publicKey: user.stellarPublicKey,
  quoteId
}
```

For normal hackathon wallets, use `publicKey`. Do not put a `G...` address in `cryptoWalletId`; that field is for a UUID wallet record ID.

### Step 4: Show PIX Instructions

Brazil on-ramp responses include PIX payment instructions, such as:

```text
pixKey
pixCode
key type
```

Show these values to the user. In production, the user pays via their bank app. In sandbox, orders do not progress automatically.

### Step 5: Simulate Fiat Received in Sandbox

For sandbox testing:

```http
POST /ramp/order/fiat_received
```

Then poll the order status. Wait 3-10 seconds after creating an order before polling because Etherfuse can return 404 while the order is being indexed.


## 7. Off-Ramp Flow: TESOURO -> BRL via PIX

Use this when a user wants to redeem TESOURO and receive BRL through PIX.

### Step 1: Request an Off-Ramp Quote

Use `POST /ramp/quote`.

Live-tested request shape:

```typescript
{
  quoteId: crypto.randomUUID(),
  customerId: user.customerId,
  blockchain: "stellar",
  walletAddress: user.stellarPublicKey,
  quoteAssets: {
    type: "offramp",
    sourceAsset: "TESOURO:GC3CW7EDYRTWQ635VDIGY6S4ZUF5L6TQ7AA4MWS7LEQDBLUSZXV7UPS4",
    targetAsset: "BRL"
  },
  sourceAmount: "1"
}
```

Use `sourceAmount`; in live sandbox testing, an off-ramp quote with only `destinationAmount` was rejected as missing `sourceAmount`.

### Step 2: Create the Off-Ramp Order

Use:

```http
POST /ramp/order
```

Off-ramp order responses differ from on-ramp responses. Do not expect final fiat transfer details in the creation response.

### Step 3: Get the Burn Transaction

Etherfuse provides, or later exposes, a pre-built unsigned `burnTransaction` XDR.

Important:

- Do not build a custom burn transaction.
- Poll `GET /ramp/order/{id}` if the `burnTransaction` is not present immediately.
- The user's wallet must sign the XDR.
- Submit the signed transaction to Stellar.

If the `burnTransaction` expires before the user signs it, call:

```http
POST /ramp/order/{order_id}/regenerate_tx
```

The refreshed transaction may arrive asynchronously through the `order_updated` webhook.

### Step 4: Simulate Crypto Received in Sandbox

For sandbox testing:

```http
POST /ramp/order/crypto_received
```

Then poll the order status until it reaches the expected completed or settled state.


## 8. Suggested App Architecture

Keep provider credentials server-side.

```text
frontend
  -> your backend route
    -> Etherfuse client
      -> Etherfuse sandbox/production API
```

Recommended environment switch:

```bash
# sandbox
ETHERFUSE_API_BASE_URL="https://api.sand.etherfuse.com"

# production
ETHERFUSE_API_BASE_URL="https://api.etherfuse.com"
```

Do not expose `ETHERFUSE_API_KEY` in browser code.

Use one small adapter in your backend:

```typescript
type PixRampProvider = {
  quoteOnramp(input: QuoteOnrampInput): Promise<Quote>;
  createOnrampOrder(input: CreateOrderInput): Promise<Order>;
  quoteOfframp(input: QuoteOfframpInput): Promise<Quote>;
  createOfframpOrder(input: CreateOrderInput): Promise<Order>;
  getOrder(orderId: string): Promise<Order>;
};
```

This lets you keep the app stable if another Brazil provider later exposes a compatible sandbox or if you add a mock fallback.


## 9. Common Gotchas

| Problem | Likely cause | Fix |
|---|---|---|
| `401` or auth failure | Used `Bearer` prefix | Send raw API key in `Authorization` |
| Assets endpoint returns missing field errors | Missing required query params | Use `blockchain`, `currency`, and `wallet` |
| Quote returns missing field errors | Missing `quoteId`, `customerId`, `walletAddress`, `blockchain`, or `sourceAmount` | Generate UUIDs and send the full quote body |
| Order status returns `404` right after creation | Etherfuse indexing delay | Wait 3-10 seconds before polling |
| Order says `Proxy account not found` | Hosted onboarding/KYC/bank flow not completed or IDs do not match | Complete onboarding and reuse the same `customerId`, `bankAccountId`, and `publicKey` |
| UUID parsing error for wallet | Sent `G...` address as `cryptoWalletId` | Use `publicKey` for Stellar addresses |
| Trustline/claim issue | Stellar onramp claim flow not completed | Sign the `stellarClaimTransaction` if returned; pre-create trustlines for offramps/swaps |
| Duplicate customer or wallet error | New IDs generated for same user | Reuse stored `customerId` and `bankAccountId` |
| Order creation seems ignored | Used `POST /ramp/orders` | Use singular `POST /ramp/order` |
| Sandbox order never changes state | Sandbox does not auto-progress | Call `fiat_received` or `crypto_received` simulation endpoint |
| Passkey wallet rejected | Sent `C...` address | Use a classic `G...` address for Etherfuse |
| No DEX path for TESOURO | Testnet liquidity is missing | Use Etherfuse on-ramp as the testnet TESOURO faucet |
| Off-ramp transaction missing | `burnTransaction` not ready yet | Poll `GET /ramp/order/{id}` |


## 10. Provider Notes

### Etherfuse

Use Etherfuse as the primary self-service Brazil path in this repo:

```text
BRL via PIX <-> TESOURO on Stellar
```

### Transfero

Transfero issues BRZ, a BRL-denominated Stellar asset.

```text
BRZ issuer:
GABMA6FPH3OJXNTGWO7PROF7I5WPQUZOB4BLTBTP4FK6QV7HWISLIEO2
```

Use BRZ as ecosystem context unless Transfero gives you current sandbox/API access. Do not assume mint/redeem API behavior from the asset issuer address alone.

### Abroad Finance

Abroad Finance is relevant for Brazil off-ramp and USDC-to-BRL-via-PIX concepts. Treat it as a reference unless you have credentials and current docs.

### Alfred Pay

Alfred Pay has sandbox material and is relevant to Latin America. Use it as an ecosystem reference for Brazil unless your team has confirmed the specific API flow you need.


## 11. When to Ask for Help

Ask a DevRel mentor or provider contact if:

- You need production PIX behavior, not sandbox simulation.
- You need provider-specific KYC/KYB behavior.
- You want to use Transfero, Abroad, or Alfred Pay as the primary provider.
- You need a BRZ mint/redeem flow rather than TESOURO.
- You are building a passkey smart wallet and need to map `C...` smart accounts to `G...` ramp identities.

For most hackathon builds, use Etherfuse sandbox + TESOURO first, then layer other Brazil providers only after you have confirmed API access.
