# Dev Setup Guide

> Get everything in this file sorted BEFORE writing code. API keys, testnet addresses, auth patterns, and critical gotchas, all in one place.


## Section 1: API Keys and Credentials

**Get your Etherfuse key before writing any code.** For Brazil, Etherfuse is the primary self-service path for BRL/PIX/TESOURO flows. Sign up at https://devnet.etherfuse.com/ramp — it takes a few minutes.

| Protocol | Key Required | How to Get | Wait | Contact |
|---|---|---|---|---|
| Etherfuse | Yes | Self-service at https://devnet.etherfuse.com/ramp | Minutes | -- |
| DeFindex | Yes | Self-service via docs | Instant | https://docs.defindex.io/api-integration-guide/api#generate-your-api-key |
| Soroswap | No | -- | -- | -- |
| Aquarius | No | -- | -- | -- |
| Blend | No | -- | -- | -- |
| Reflector Oracle | No | -- | -- | -- |
| BlindPay | Yes (sandbox) | Self-service at app.blindpay.com | Instant | https://app.blindpay.com |
| AlfredPay | Yes (sandbox) | Public sandbox keys in docs | Instant | See below |
| Abroad Finance | Contact required | Use as Brazil ecosystem reference unless you have credentials | Unknown | https://abroad.finance |
| Transfero | Contact required | Issuer of BRZ; sandbox/API access may require support | Unknown | https://transfero.com |
| Trustless Work | Yes | Self-service via docs | Instant | https://docs.trustlesswork.com/trustless-work/introduction/developer-resources/request-api-key |


### Etherfuse

- **Sandbox base URL:** `https://api.sand.etherfuse.com`
- **Production base URL:** `https://api.etherfuse.com`
- **Validate your key:** `GET /ramp/me` returns org name + KYB timestamp
- **Auth header:** `Authorization: your-api-key` with no "Bearer" prefix (see Section 4)
- **customerId:** UUID you generate yourself, store permanently, reuse forever (see Section 5)
- **bankAccountId:** same pattern: you assign it, Etherfuse binds it on first use
- **Brazil flow:** BRL via PIX ↔ TESOURO on Stellar
- **Mexico flow also supported:** MXN via SPEI ↔ CETES on Stellar, but this Brazil guide defaults to BRL/PIX/TESOURO
- Sandbox auto-approves bank accounts; production requires KYB review

### DeFindex

- **SDK env var:** `DEFINDEX_API_KEY`
- **Auth:** `Authorization: Bearer your-api-key` (standard Bearer, opposite of Etherfuse)
- **Self-service signup:** https://docs.defindex.io/api-integration-guide/api#generate-your-api-key


### AlfredPay

Public sandbox credentials from the AlfredPay docs — shared and intended for anyone reading the docs.

- **api-key:** `alfredpay.T+QdhCoMofy8.gGKO5KdB7M+B/9jLFef`
- **api-secret:** `PwF+cDGZd37eHYZ.CVvF1/zbHCBosSo1`
- **Auth headers:** `api-key` and `api-secret` (see Section 4)
- **Reference:** https://alfredpay.readme.io/reference/post_customers-create-1

Use Alfred Pay as an ecosystem reference for Brazil unless you already have working credentials and current API docs for your flow. Etherfuse is the safer self-service path for this guide.

### Abroad Finance

Abroad Finance is relevant for Brazil off-ramp/payment flows, especially USDC to BRL via PIX. Treat it as an ecosystem reference unless you have direct sandbox/API access.

### Transfero

Transfero is the issuer of BRZ, a BRL-denominated stablecoin available on Stellar. For this guide, BRZ is useful as a local-asset reference; do not assume self-service mint/redeem API access unless Transfero has provisioned you.


### BlindPay

1. Sign up at https://app.blindpay.com
2. Create a free "Development instance"
3. Generate API keys for that instance


### Trustless Work

- **React SDK env var (client):** `NEXT_PUBLIC_TRUSTLESS_WORK_API_KEY`
- **Server env var (Node / headers):** `TRUSTLESS_WORK_API_KEY`
- **Auth:** `x-api-key: your-api-key`
- **Self-service signup:** https://docs.trustlesswork.com/trustless-work/introduction/developer-resources/request-api-key


## Section 2: Testnet Contract Addresses

These are the actual deployed addresses on Stellar testnet. Testnet contracts get redeployed periodically. If something stops working, check the protocol's GitHub for updated addresses.

### Stellar Testnet Infrastructure

| Resource | Value |
|---|---|
| Soroban RPC | `https://soroban-testnet.stellar.org` |
| Friendbot | `https://friendbot.stellar.org` |

### Stellar Testnet USDC & XLM SACs

| Asset | Address |
|---|---|
| Circle USDC | `CBIELTK6YBZJU5UP2WWQEUCYKLPU6AUNZ2BQ4WWFEIE3USCIHMXQDAMA` |
| XLM | `CDLZFC3SYJYDZT7K67VZ75HPJVIEUVNIXF47ZG2FB2RMQQVU2HHGCYSC` |

### DeFindex (testnet)

Verified from the paltalabs registry.

| Contract | Address | Verified |
|---|---|---|
| Factory | `CDSCWE4GLNBYYTES2OCYDFQA2LLY4RBIAX6ZI32VSUXD7GO6HRPO4A32` | Yes |
| USDC vault | `CBMVK2JK6NTOT2O4HNQAIQFJY232BHKGLIMXDVQVHIIZKDACXDFZDWHN` | Yes |
| XLM vault | `CCLV4H7WTLJQ7ATLHBBQV2WW3OINF3FOY5XZ7VPHZO7NH3D2ZS4GFSF6` | Yes |
| CETES vault | `CBIS5TEMTNNOTBE3WXPQUAGUEDYZZVIWAKTXEQCOUJ34OJJ3FJ5NLF2P` | Mexico reference |
| CETES Blend strategy | `CCP4RBDWPRNO2LWO23XFU4BBLGA73J5N3BK7EHRJUHVN33YEMMFB2MBE` | Mexico reference |

**Verification note:** Run 1 (wrong addresses) got `Error(Storage, MissingValue)`. Run 2 (paltalabs addresses above) got `Error(WasmVm, InvalidAction)`, meaning the contract IS deployed and running; the error was a call-level issue, not an address issue.

**API note:** The DeFindex testnet API layer is currently offline. Contract calls still work; the SDK's HTTP layer does not. Use mainnet for full API functionality — source vault addresses from https://app.defindex.io.


### Soroswap (testnet)

| Contract | Address |
|---|---|
| Router | `CCJUD55AG6W5HAI5LRVNKAE5WDP5XGZBUDS5WNTIVDU7O264UZZE7BRD` |
| Factory | `CDP3HMUH6SMS3S7NPGNDJLULCOXXEPSHY4JKUKMBNQMATHDHWXRRJTBY` |

Source: https://github.com/soroswap/core/blob/main/public/testnet.contracts.json

### Blend (testnet)

| Contract | Address |
|---|---|
| Pool Factory V2 | `CDV6RX4CGPCOKGTBFS52V3LMWQGZN3LCQTXF5RVPOOCG4XVMHXQ4NTF6` |
| Backstop V2 | `CBDVWXT433PRVTUNM56C3JREF3HIZHRBA64NB2C3B2UNCKIS65ZYCLZA` |
| TestnetV2 Pool | `CCEBVDYM32YNYCVNRXQKDFFPISJJCV557CDZEIRBEE4NCV4KHPQ44HGF` |
| CETES Pool (kaankacar) | `CAPBMXIQTICKWFPWFDJWMAKBXBPJZUKLNONQH3MLPLLBKQ643CYN5PRW` |

The CETES pool is listed on https://testnet.blend.capital and is kept here as a Mexico reference. For Brazil/TESOURO, verify current pools before building a swap or yield route.

Source: https://github.com/blend-capital/blend-utils/blob/main/testnet.contracts.json


### Aquarius (testnet)

Aquarius does not publish a static contract address list. Use the testnet app to find current pool addresses: https://testnet.aqua.network/pools


### Reflector Oracle (testnet)

Contract addresses are listed at https://reflector.network/oracles — use the mainnet/testnet toggle in the upper-right.


### Trustless Work (testnet)

Verified from the Trustless Work registry.

| Contract | Address | Verified |
|---|---|---|
| multi-release Escrow | `CB7EYMEHZI3UWS3EHNOUI55OD6X5FLMV537NEUQ6EWO677N6B6XSBP25` | Yes |
| single-release Escrow | `CDHAZ2RTE2MDHYQQ7NATF5IVKIFVGLX6FHJ66OPK6MUXBSTRRXFXJ6QB` | Yes |

> Testnet contracts get redeployed periodically. If an address stops working, check the source links above for updated values.


## Section 3: Testnet Asset Registry

**The single most expensive gotcha (90+ min in experiments):** testnet USDC has multiple issuers that do NOT share liquidity. Pick the wrong one and swaps silently fail.

### USDC Issuers on Testnet

| Source | Issuer Address | Used by |
|---|---|---|
| Circle (standard testnet) | `GBBD47IF6LWK7P7MDEVSCWR7DPUWV3NY3DTQEVFL4NAT4AQH3ZLLFLA5` | Soroswap, SDEX, most protocols |
| Blend | `GATALTGTWIOT6BUDBCZM3Q4OQ4BO2COLOAZ7IYSKPLC2PMSOPPGF5V56` | Blend pools |
| Etherfuse | TBD — Etherfuse feeds their own USDC issuer to testnet SDEX liquidity; verify before using | Etherfuse flows |

**Rule:** Check which issuer your protocol expects. Do NOT assume USDC is USDC on testnet.


### Etherfuse Assets

**Testnet issuer:** `GC3CW7EDYRTWQ635VDIGY6S4ZUF5L6TQ7AA4MWS7LEQDBLUSZXV7UPS4`
**Mainnet / production issuer:** `GCRYUGD5NVARGXT56XEZI5CIFCQETYHAPQQTHO2O3IQZTHDH4LATMYWC`

| Asset Code | Name | Issuer / Contract Reference | Notes |
|---|---|---|---|
| TESOURO | Etherfuse TESOURO | issuer `GC3CW7EDYRTWQ635VDIGY6S4ZUF5L6TQ7AA4MWS7LEQDBLUSZXV7UPS4` on testnet | Primary Brazil asset: BRL via PIX ↔ TESOURO |
| CETES | Etherfuse CETES | `CC72F57YTPX76HAA64JQOEGHQAPSADQWSY5DWVBR66JINPFDLNCQYHIC` | Mexico reference: MXN via SPEI ↔ CETES |
| CZERO | CETES Zero (0% APY) | TBD | Mexico reference |

> SAC contract IDs can be derived from the issuer + asset code using the Stellar SDK: `Contract.fromAsset(asset, networkPassphrase)`. Ask a mentor or check Stellar Lab for pre-computed addresses.

**Testnet limitation:** Do not assume TESOURO/USDC or CETES/USDC liquidity on the testnet SDEX. If `pathPaymentStrictSend` fails with `op_no_path`, the issue is probably missing testnet liquidity, not your transaction builder. You can test the trustline flow, but the reliable acquisition path is the anchor onramp.

> To get testnet TESOURO: use the Etherfuse API onramp process — the same flow you'd build in production. The sandbox auto-approves, minting TESOURO directly to your testnet wallet. This IS the faucet. Swapping via Soroswap, Aquarius, or SDEX may or may not work on testnet; onramp is the reliable path.

### Brazil Assets

| Asset Code | Name | Stellar Issuer | Notes |
|---|---|---|---|
| BRZ | Brazilian Digital Token by Transfero | `GABMA6FPH3OJXNTGWO7PROF7I5WPQUZOB4BLTBTP4FK6QV7HWISLIEO2` | Mainnet BRL stablecoin reference. Verified on Stellar Expert. |
| TESOURO | Etherfuse TESOURO | `GCRYUGD5NVARGXT56XEZI5CIFCQETYHAPQQTHO2O3IQZTHDH4LATMYWC` on mainnet / `GC3CW7EDYRTWQ635VDIGY6S4ZUF5L6TQ7AA4MWS7LEQDBLUSZXV7UPS4` on testnet | Primary Etherfuse Brazil ramp asset. |


## Section 4: Authentication Patterns

Auth format varies per protocol in ways that are not documented anywhere. This table is the reference.

| Protocol | Header Format | Notes |
|---|---|---|
| Etherfuse | `Authorization: your-api-key` | NO "Bearer" prefix. Raw key only. |
| DeFindex | `Authorization: Bearer your-api-key` | Standard Bearer. Opposite of Etherfuse. |
| AlfredPay | `api-key: <key>` + `api-secret: <secret>` | Two separate headers, not Bearer. |
| Soroban RPC | Varies by provider | Some use Bearer, some use `X-API-Key` |
| Soroswap | None | No auth required |
| Phoenix | None | No auth required |
| Aquarius | None | No auth required |
| Trustless Work | `x-api-key: your-api-key` | NO "Bearer" prefix. Raw key only. |

### Etherfuse (correct)

```typescript
headers: {
  'Authorization': process.env.ETHERFUSE_API_KEY,
  'Content-Type': 'application/json'
}
```

### DeFindex (correct)

```typescript
headers: {
  'Authorization': `Bearer ${process.env.DEFINDEX_API_KEY}`,
  'Content-Type': 'application/json'
}
```

### Trustless Work (correct)

```typescript
headers: {
  'x-api-key': process.env.TRUSTLESS_WORK_API_KEY,
  'Content-Type': 'application/json'
}
```



## Section 5: Critical Setup Gotchas

These are the issues that blocked developers for 30-120 minutes each across 60 build runs. Know them upfront.


### Etherfuse: customerId and bankAccountId are per end-user

Each end-user of your app needs their own `customerId` and `bankAccountId`. These are permanent per user — generate once, store in your database, reuse forever. Never generate a new one per request or per session. If you generate duplicates, you'll get "Bank account not found" errors with no useful debug info.

You assign both IDs yourself (they're UUIDs you generate). Etherfuse binds them on first use.


### Etherfuse: a public key can only be registered to one customer

The `create customer` and `create bank account` requests will fail if the public key (G... address) was already registered in a previous call — even in sandbox. If you're re-running your setup flow, reuse the same `customerId` and `bankAccountId` rather than trying to re-register.


### Etherfuse: quote endpoint is POST with three direction types

`POST /ramp/quote` accepts three directions via `quoteAssets.type`: `onramp` (BRL → crypto), `offramp` (crypto → BRL), and `swap` (crypto → crypto, Stellar and Solana only). Swap target must be a stablebond; source is typically USDC.

For Brazil onramp/offramp quotes, the sandbox expects the request to include a UUID `quoteId`, the onboarded `customerId`, `blockchain: "stellar"`, the user's classic Stellar `walletAddress`, a `quoteAssets` object, and `sourceAmount`. In live sandbox testing, `destinationAmount` was not accepted for the TESOURO → BRL off-ramp quote; use `sourceAmount`.

```typescript
// Onramp quote: BRL -> TESOURO
{
  quoteId: crypto.randomUUID(),
  customerId,
  blockchain: "stellar",
  walletAddress: userPublicKey, // G... address
  quoteAssets: {
    type: "onramp",
    sourceAsset: "BRL",
    targetAsset: "TESOURO:GC3CW7EDYRTWQ635VDIGY6S4ZUF5L6TQ7AA4MWS7LEQDBLUSZXV7UPS4"
  },
  sourceAmount: "10"
}

// Offramp quote: TESOURO -> BRL
{
  quoteId: crypto.randomUUID(),
  customerId,
  blockchain: "stellar",
  walletAddress: userPublicKey, // G... address
  quoteAssets: {
    type: "offramp",
    sourceAsset: "TESOURO:GC3CW7EDYRTWQ635VDIGY6S4ZUF5L6TQ7AA4MWS7LEQDBLUSZXV7UPS4",
    targetAsset: "BRL"
  },
  sourceAmount: "1"
}
```


### Etherfuse: response bodies are snake_case, request bodies are camelCase

API responses use `snake_case` (`presigned_url`, `bank_account_id`, `exchange_rate`). Request bodies use `camelCase` (`bankAccountId`, `quoteId`, `sourceAmount`). This is consistent throughout the API but easy to miss when reading responses and building requests side by side.

### Etherfuse: hosted onboarding is required before order creation

Live sandbox testing showed that a quote can succeed for a generated `customerId`, but `POST /ramp/order` can still fail with `Proxy account not found` if the hosted onboarding flow has not been completed for the matching `customerId`, `bankAccountId`, and `publicKey`.

Hosted onboarding URL generation succeeded with this request shape:

```typescript
{
  customerId,
  bankAccountId,
  publicKey: userPublicKey, // G... address
  blockchain: "stellar",
  accountType: "business"
}
```

In live testing, `accountType: "business"` returned `200`; `individual`, `company`, and uppercase values were rejected.


### Etherfuse: onramp and offramp order response shapes differ

- Onramp: Brazil responses include PIX payment instructions (`pixKey`, `pixCode`, and key type). Mexico responses include `depositClabe` for SPEI.
- Offramp: response does NOT include the final fiat transfer details. It includes or later exposes a `burnTransaction` (pre-built unsigned XDR) and `statusPage`. The burn transaction must be signed with the user's wallet and submitted to Stellar. Do not build a custom burn transaction — always use the one Etherfuse provides.

In sandbox, the offramp `burnTransaction` may not appear immediately in the creation response. Poll `GET /ramp/order/{id}` until it appears.


### Etherfuse sandbox: simulate fiat manually

Orders don't progress automatically in sandbox. You must trigger state transitions yourself:

```
POST /ramp/order/fiat_received    # simulate fiat arriving (onramp)
POST /ramp/order/crypto_received  # simulate crypto arriving (offramp)
```


### Etherfuse: 3-10 second indexing delay

After creating an order, wait 3-10 seconds before querying its status. Immediate queries return 404. This is not an error; the order is being indexed.


### Etherfuse: rejects Soroban contract addresses (C... format)

Etherfuse only accepts classic Stellar addresses (G...). Passkey smart wallets use Soroban contract addresses (C...); if you're building with one, use a fee-payer G... address as the Etherfuse wallet identity instead.


### Etherfuse: quoteAssets is an object with a type discriminator

```typescript
// Onramp (BRL → crypto)
quoteAssets: { type: 'onramp', sourceAsset: 'BRL', targetAsset: 'TESOURO:GC3CW7...' }

// Offramp (crypto → BRL)
quoteAssets: { type: 'offramp', sourceAsset: 'TESOURO:GC3CW7...', targetAsset: 'BRL' }

// Swap (crypto → crypto, Stellar/Solana only)
quoteAssets: { type: 'swap', sourceAsset: 'USDC:GA5ZS...', targetAsset: 'TESOURO:GC3CW7...' }
```

Stellar assets use `CODE:ISSUER` format. The `blockchain` field is also required in the quote request.


### Etherfuse: order creation is POST /ramp/order (singular), not /ramp/orders

The list endpoint (`GET /ramp/orders`, plural) also accepts POST and always returns the paginated order list regardless of the request body. Hitting `POST /ramp/orders` by mistake looks like the sandbox is ignoring your request — it isn't. The create endpoint is `POST /ramp/order` (singular).

```
POST /ramp/order   ← creates a new order (singular)
GET  /ramp/orders  ← paginated list (plural)
POST /ramp/orders  ← also returns paginated list, ignores body — wrong endpoint
```

The create endpoint requires `orderId`, `bankAccountId`, `quoteId`, and a wallet reference. For hackathon builders using a normal Stellar wallet address, pass `publicKey`, not `cryptoWalletId`:

```typescript
{
  orderId: crypto.randomUUID(),
  bankAccountId,
  publicKey: userPublicKey, // G... address
  quoteId
}
```

`cryptoWalletId` is a UUID wallet record ID. Do not put a `G...` public key in `cryptoWalletId`; the sandbox parses that field as a UUID.


### Etherfuse: quote response uses sourceAmount/destinationAmount

The quote response uses `sourceAmount` and `destinationAmount` (not `fromAmount`/`toAmount`). It also includes `exchangeRate`, `feeBps`, `feeAmount`, `expiresAt`, and `requiresSwap`. Normalize on read if you're abstracting multiple providers:

```typescript
fromAmount: Number(raw.fromAmount) || Number(raw.sourceAmount),
toAmount: Number(raw.toAmount) || Number(raw.destinationAmount),
```

### Etherfuse: exchange rates are available via a public endpoint

`GET /lookup/exchange_rate` (no auth required) returns USD conversion rates for all supported currencies. The response has a multi-source structure — each rate is aggregated from several providers and the top-level `rate` field is the final aggregated value.

### Etherfuse: Stellar onramp trustlines can use the claim flow

For Stellar onramps, pass `walletAddress` in the quote request. Etherfuse docs describe an automatic wallet setup path: if the wallet lacks the trustline or does not exist on-chain yet, the quote fee can include the one-time onboarding cost, and the completed order can include a `stellarClaimTransaction` unsigned XDR for the user to sign.

For offramps and swaps, assume the wallet must already be funded and have the relevant trustline before the order can complete.

### Etherfuse: sandbox onramp limits are currency-specific

Mexico sandbox quotes above 500 MXN have returned `SandboxAmountExceeded` in prior testing. For Brazil, start with small BRL test amounts and check the current sandbox response before wiring a fixed max into your UI.

### Etherfuse: duplicate order constraint

Only one open order is allowed per bank account + amount combination. Creating a duplicate returns an error. Cancel the existing order first via `POST /ramp/order/{id}/cancel` (only works on orders in `created` status).

### Etherfuse: expired transactions can be regenerated

If an offramp `burnTransaction` expires before the user signs it, call `POST /ramp/order/{order_id}/regenerate_tx`. It returns HTTP 202 immediately — the new transaction is delivered asynchronously via the `order_updated` webhook.

### Etherfuse: KYC sandbox auto-approve requires signing all three agreements

Submitting KYC data programmatically puts the customer in `proposed` state, not `approved`. In sandbox, auto-approval only triggers after the customer signs all three agreements via the presigned onboarding URL: electronic signature → terms and conditions → customer agreement. If you're testing the full flow, complete the onboarding URL step.

For integration testing without full KYC, register a wallet with `claimOwnership: true` — this links the wallet's KYC to your organization's KYB status and auto-approves it immediately.


### Etherfuse: assets endpoint requires wallet-aware query params

`GET /ramp/assets` requires query parameters in the current sandbox. For Brazil on Stellar, this live-tested form returned TESOURO:

```http
GET /ramp/assets?blockchain=stellar&currency=brl&wallet=<G_ADDRESS>
```

The response may be either a bare array or `{ assets: [...] }` depending on query parameters and API version. Normalize on read:

```typescript
const list = Array.isArray(data) ? data : (data.assets || data.data || []);
```

The TESOURO entry should include:

```text
TESOURO:GC3CW7EDYRTWQ635VDIGY6S4ZUF5L6TQ7AA4MWS7LEQDBLUSZXV7UPS4
```


### Soroban: always simulate before sending

```typescript
const simulation = await server.simulateTransaction(tx);
const assembled = SorobanRpc.assembleTransaction(tx, simulation);
await server.sendTransaction(assembled);
```

Skipping simulation causes confusing failures. This is different from classic Stellar and easy to miss.


### Soroban: wrong network passphrase gives tx_bad_auth

A network passphrase mismatch (e.g. using mainnet passphrase against testnet)
produces `tx_bad_auth`, not a network error. Always derive passphrase from
`Networks.TESTNET` or `Networks.PUBLIC` rather than hardcoding the string.


### Soroban: storage entries expire — extend TTLs proactively

Soroban storage entries (persistent and instance) have a TTL measured in ledgers.
When they expire, the data is archived and reads return a missing-value error with
no warning. Extend proactively:

- Check TTL threshold: ~100 ledgers remaining
- Extend target: ~518400 ledgers (~30 days)
- Use `bump_instance_storage` / `bump_persistent_storage` in your contract, or
  `server.restoreFootprint()` from the JS SDK if the entry is already archived.


### sendTransaction returns PENDING, not success

`sendTransaction` returning without error does NOT mean the transaction succeeded. Poll with a max retry ceiling:

```typescript
for (let i = 0; i < 60; i++) {
  await sleep(1000);
  const result = await server.getTransaction(hash);
  if (result.status !== 'NOT_FOUND') return result;
}
throw new Error('Transaction timeout after 60s');
```

Alternatively, the JS SDK provides `rpc.Server.pollTransaction(hash, options)` which wraps this loop: https://stellar.github.io/js-stellar-sdk/module-rpc.Server.html#pollTransaction


### DeFindex: classic assets must be SAC-deployed

Classic Stellar assets (XLM, USDC, etc.) cannot be deposited into Soroban-based protocols like DeFindex directly — they must first be deployed as Stellar Asset Contracts (SACs). This is a one-time, network-wide action per asset. In practice, all major testnet assets (including native XLM and Circle USDC) have already been SAC-deployed. You should not need to deploy one yourself. If you hit a "contract not found" error on deposit, verify the SAC address for that asset using `Contract.fromAsset(asset, networkPassphrase)`.


### DeFindex: API quirks

- Endpoint is `/vault/` not `/vaults/` (returns 404 if you use the plural)
- Query param is `?from=<address>` not `?user=<address>`
- Amount format is always an array: `{"amounts": [1000000]}` not `{"amount": 1000000}`
- Success response is HTTP 201, not 200
- Mark `@defindex/sdk` as `serverExternalPackages` in `next.config.ts` to prevent client-side bundling (the SDK makes authenticated HTTP calls; the API key must stay server-side)


### DeFindex: SDK errors are plain objects, not Error instances

The SDK rejects with `error.response.data` — a plain object. Accessing `.message` directly returns `undefined`. Write a normalizer:

```typescript
function extractErrorMessage(error: unknown): string {
  if (error && typeof error === 'object') {
    const e = error as Record<string, unknown>;
    return String(e.message ?? e.error ?? e.detail ?? e.msg ?? JSON.stringify(error));
  }
  return String(error);
}
```


### Soroswap SDK: amounts must be BigInt, slippage must be a string

Passing regular numbers produces silent failures or validation errors. Always convert:

```typescript
amount: BigInt(userInput),  // not parseFloat(userInput)
slippage: '50',             // basis points as string, not a number
```

Also: the correct npm package is `@soroswap/sdk` (scoped). The GitHub docs reference an outdated unscoped name (`soroswap-sdk`) that will fail to install.


### Trustlines before assets

Establish trustlines before any operation that will receive an asset. Skipping this produces `op_no_destination` or a silent no-op.


### Freighter: API calls hang forever without the extension installed

`isConnected()` and other Freighter API calls return promises that never resolve if the extension is not installed. Wrap all Freighter calls with a timeout:

```typescript
function withTimeout<T>(promise: Promise<T>, ms = 2000): Promise<T | null> {
  return Promise.race([
    promise,
    new Promise<null>(res => setTimeout(() => res(null), ms))
  ]);
}
```

Treat a `null` result as "extension not installed."


### Freighter: do not statically import in Next.js (SSR crash)

`@stellar/freighter-api` uses browser globals. Static top-level imports crash server-side rendering. Import dynamically inside async functions instead:

```typescript
// Crashes SSR
import { isConnected } from '@stellar/freighter-api';

// Correct
const { isConnected } = await import('@stellar/freighter-api');
```


### Freighter API v6: signTransaction() returns an object, not a string

In v6, `signTransaction()` returns `{ signedTxXdr: string, signerAddress: string }`, not a raw XDR string. Old v5 examples that use the return value directly will silently pass an object where a string is expected:

```typescript
const result = await signTransaction(xdr, { networkPassphrase, address });
const signedXdr = result.signedTxXdr; // required in v6
```


### Stellar: use Soroban RPC for everything

Use `rpc.Server` (Soroban RPC) as your primary server for all operations — smart contract calls, classic payments, ChangeTrust, account creation, and sequence numbers. SDK v14 supports submitting classic transactions through Soroban RPC directly. Routing operations to the wrong server silently fails, so default to RPC and stay there.

| Need | Use |
|---|---|
| Smart contract calls (invoke, simulate, send) | Soroban RPC (`rpc.Server`) |
| Classic payments, ChangeTrust, account creation | Soroban RPC (`rpc.Server`) |
| Account sequence number | Soroban RPC (`rpc.Server`) |
| Historical transaction lookup (if unavoidable) | Horizon (`Horizon.Server`) — last resort only |


### WebAuthn passkeys: rpId must be a domain, not an IP address

WebAuthn rejects IP addresses as the `rpId`. If your browser connects via `127.0.0.1`, explicitly force `rpId` to `'localhost'`. Never use `192.168.x.x` or bare IP addresses for local development.


### Trustless Work: Role Trustline Requirement

Each role involved must establish a trustline for the asset being used to create the escrow before the escrow is initialized.


### Trustless Work: Restrictions when escrow is funded

The platform role is the only role authorized to modify the escrow at any time. However, once the escrow holds a balance, modifications are restricted exclusively to adding milestones, and no other properties can be updated.


### Trustless Work: Trustline Requirement

The trustline address must be the issuer address (starting with 'G'), not the contract ID.


### Trustless Work: Dispute Resolver Restriction

The dispute resolver address must be unique.


## Section 6: Known Limitations, and What to Ask Your DevRel Mentor

These are things that don't work smoothly yet or aren't documented. If you hit any of them during the hackathon, don't spin your wheels. Ask a DevRel mentor directly. They can unblock you faster than debugging alone.

### Things that still need manual help

- **Etherfuse sandbox key:** self-service at https://devnet.etherfuse.com/ramp. Takes a few minutes.
- **Testnet TESOURO balance:** use the Etherfuse onramp flow to mint testnet TESOURO. The sandbox auto-approves and mints directly to your wallet. This IS the faucet. See Section 3.

### Things the docs don't cover (save yourself the debugging time)

- **Classic asset SAC deployment:** required before depositing into Soroban-based protocols like DeFindex, not mentioned in the docs. In practice, all common assets are already deployed. See Section 5.
- **DeFindex: rate limits:** undocumented. If you're getting throttled, ask a mentor what the current limits are.
- **DeFindex: webhook retry policy:** undocumented. If you need reliable webhook delivery for your project, ask before building around it.
- **Etherfuse: quote expiration:** quotes expire after ~2 minutes. Not documented prominently. Design your flow accordingly.

### Known API quirks (not bugs you can fix, just reality)

- **Etherfuse response envelope inconsistency:** `orderId` vs `order_id` vs `id` varies by endpoint; onramp/offramp responses are wrapped differently. The normalizer in Section 5 handles this.
- **Phoenix rate limiting returns 403:** looks like an auth error but it's actually a rate limit. If you get a sudden 403 from Phoenix after things were working, wait and retry.
- **Stellar Wallets Kit v1 vs v2:** breaking change between versions where instance methods became static methods. If you're copying examples from the web and they don't work, check which version you're running.

  ```typescript
  // v1 (broken — do not use)
  const kit = new StellarWalletsKit({ ... });

  // v2 (correct)
  const kit = StellarWalletsKit.build({
    network: WalletNetwork.TESTNET,
    selectedWalletId: FREIGHTER_ID,
    modules: allowAllModules(),
  });
  ```
