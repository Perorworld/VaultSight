# Ducat Vault Risk & Quanta Simulator

A single-page tool that helps people understand BTC-backed borrowing on Ducat before they open a vault. It models collateral, liquidation risk, Quanta rewards and borrowing costs, then sends users to Ducat to open the real vault.

Everything runs in the browser. There is no backend, no build step and no database.

## Features

- **Vault simulator:** BTC collateral, BTC price and target ratio (160–500%) with live results for borrowed UNIT, fee, net received, debt, liquidation price, distance to liquidation and a SAFE / CAUTION / DANGER health meter.
- **Stress test:** BTC +20% to −40% presets, a custom shock slider and a collateral-ratio chart.
- **Quanta forecaster:** daily, 30-day and 90-day projections with a chart.
- **Cost comparison:** Ducat's one-time fee against hypothetical 6%, 8% and 10% APR loans over 30 to 365 days.
- **Live BTC price:** fetched automatically from several public sources, with a manual refresh button.
- **Wallet connect (read-only):** Xverse, Unisat, Leather and OKX. Shows the address and BTC balance and can use the balance as collateral. Nothing is signed or sent.
- **Position card:** 1200×630 PNG download, copy summary and share on X. No wallet address appears on the card.
- **Shareable links:** the current scenario is stored in the page address.
- **Take it to Ducat:** a button that copies the simulated position and opens Ducat.

## Project structure

```
index.html   the whole app (HTML, CSS and JavaScript)
README.md    this file
```

The charts use Chart.js, loaded from the jsDelivr CDN.

## Run locally

Serve the folder instead of opening the file directly, because some price sources reject requests from `file://` pages.

```bash
npx serve .
# or
python3 -m http.server 8000
```

Then open the address it prints.

## Deploy

1. Create a GitHub repository and upload `index.html` and `README.md`.
2. Go to vercel.com and choose **Add New → Project**.
3. Import the repository. Framework preset: **Other**. Leave the build command and output directory empty.
4. Click **Deploy**.

Vercel serves `index.html` at the site root. Every push to the main branch redeploys automatically.

## Configuration

All protocol settings are in the `CONFIG` object at the top of the script in `index.html`.

| Setting | Meaning |
| --- | --- |
| `appUrl` | Where the "Open vault on Ducat" button goes. **Set this to the exact Ducat app link.** |
| `liqRatio` | Liquidation threshold (1.35 = 135%). |
| `originationFee` | One-time fee (0.01 = 1%). |
| `ongoingInterest` | Ongoing interest (0). |
| `quantaPerUnitPerDay` | Quanta accrual per UNIT per day (100). |
| `unitUsd` | Value of 1 UNIT in USD (1). |
| `feeTreatment` | `"deducted"` takes the fee from the loan. `"added"` adds it to the debt. |
| `dangerBuf`, `cautionBuf` | Multiples of the liquidation threshold that set the DANGER and CAUTION zones. |

## Things to verify before launch

- **Fee:** the brief uses a 1% one-time fee. A Circle partner listing for Ducat states a 7% upfront minting fee and a 15% protocol take at liquidation. Confirm the current terms with Ducat and update `originationFee`.
- **Fee settlement:** whether the fee is deducted from the loan or added to the debt is an assumption.
- **UNIT value:** the simulator assumes 1 UNIT = 1 USD.
- **Health thresholds:** the CAUTION and DANGER zones are this tool's own choice, not Ducat's.
- **Liquidation:** the simulator uses a simple ratio check. Actual liquidation is handled by the Ducat protocol.

## Limits

- The site cannot open vaults. Ducat vaults use a multi-signature setup with Ducat's guardian network, so users finish on Ducat.
- Wallet connection is read-only. Wallet extensions differ between versions, so test each wallet you plan to support.
- Wallet balances come from mempool.space and show the balance of one address, not a whole wallet.
- Results are estimates and not financial advice.

## Privacy

No analytics, accounts or server storage. Wallet addresses are never placed in shared links or position cards.
