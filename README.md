## Getting Started with WheelTracker

### What is WheelTracker?

WheelTracker is an iOS app built for options traders running the **wheel strategy** — selling Cash-Secured Puts (CSPs) and Covered Calls (CCs) to generate consistent premium income. It tracks your open positions, manages the full assignment lifecycle, and gives you a real-time view of your portfolio performance.

---

### Your First Position

#### 1. Open the App

When you first launch WheelTracker, you'll land on the **Positions** tab. This is your main workspace. It shows every active option leg you're currently managing.

#### 2. Add a Cash-Secured Put

Tap the **+** button to open the New Position form. Fill in:

|Field|What to Enter|
|---|---|
|**Ticker**|The underlying symbol (e.g. `AAPL`)|
|**Strategy**|Cash-Secured Put|
|**Transaction Date**|The date you opened the trade|
|**Expiry**|Option expiration date|
|**Strike**|Strike price (e.g. `175.00`)|
|**Contracts**|Number of contracts|
|**Premium**|Premium received per share (e.g. `2.35`)|
|**Fees**|Total commissions paid|
|**Delta**|_(Optional)_ Delta at entry|

Tap **Save**. Your position appears in the Positions list with a live DTE countdown ring.

#### 3. Understand the DTE Ring

Each position row shows a circular ring indicating days to expiration:

- **Green** — more than 21 days remaining
- **Orange** — 8 to 21 days remaining
- **Red** — 7 days or fewer remaining

---

### Managing a Position


Tap any position to open its detail view. From here you can take the following actions:

#### Roll

The position is expiring and you want to extend it. Tap **Roll** to open a pre-filled form for the new leg. The original leg is preserved in the chain history, and the new leg becomes the active position.

#### Close

You're buying back the option before expiration. Tap **Close** and enter the premium paid to buy it back. The position is marked terminated and moves out of your active list. The buyback cost is factored into your chain P&L.

#### Expire

The option expired worthless — you keep the full premium with no buyback cost. Tap **Expire** to record this. No form is needed; the position is immediately marked as terminated. This is distinct from Close: with Expire, your net contribution from this leg is $0 (the profit was already captured when you opened the position).

#### Assign

The option was assigned. Tap **Assign** to record the assignment. A confirmation dialog will appear before the action executes.

- **CSP Assignment** — You now own 100 shares per contract at the strike price. The position moves to the **Shares Held** tab and a **Sell Covered Call** button becomes available.
- **CC Assignment ("Called Away")** — Your shares were sold at the strike price. The position is terminated and the full chain P&L is realized.

---

> **Tip:** Once a position is terminated (closed, expired, or assigned), the DTE ring and Annualized ROI are hidden in the detail view. The chain summary switches from "Chain Premiums" to "Realized Chain P&L."

---

### The Wheel in WheelTracker


WheelTracker models the wheel strategy as a **chain**, a linked sequence of legs from the first CSP open through to final termination. Every leg is connected, giving you a complete picture of P&L across the entire trade.

A chain ends when any leg reaches a **terminal action**: Close, Expire, or Assign.

```
CSP (Open)
  ├── Roll → CSP (Roll)        ← extend the position
  │             └── ...
  ├── Close                    ← bought back before expiry
  ├── Expire                   ← expired worthless, full premium kept
  └── Assigned                 ← shares received at strike
        ├── CC (Open)
        │     ├── Roll → CC (Roll)
        │     │             └── ...
        │     ├── Close
        │     ├── Expire       ← CC expired worthless, shares still held
        │     └── Assigned     ← shares called away at strike
        └── (hold shares indefinitely)
```

**Close vs. Expire** — both terminate the chain, but they're recorded differently. Close means you paid a premium to buy back the option; that cost is subtracted from your chain P&L. Expire means the option expired worthless; your net contribution from that leg is $0, and you keep 100% of the premium collected when you opened it.

Every leg in a chain is linked and visible in the timeline section of the detail view. You can review or edit any leg at any time. Deleting a position removes the **entire chain**.

---

### The Three Tabs

|Tab|What It Shows|
|---|---|
|**Positions**|Active option legs — the current open leg of each chain|
|**Shares Held**|CSP assignments where you're holding shares (not yet called away)|
|**Summary**|Portfolio-wide metrics: total premium, capital at risk, annualized ROI, and more|

---

### Tips for Getting the Most Out of WheelTracker

- **Enter fees accurately** — they're factored into every ROI and P&L calculation.
- **Use the Delta field** — it's optional, but useful for reviewing your entry discipline over time.
- **Roll before closing** — if you're extending a position, use Roll rather than closing and opening separately. This keeps the chain intact and gives you accurate chain-level P&L.
- **Back up your data** — use the **↑↓** button in the Summary tab to export your positions as CSV or XLSX at any time.

---

## Key Screens

---

### Positions Tab

The Positions tab is your main dashboard. It shows the **current active leg** of every chain — meaning if you've rolled a position multiple times, you only see the most recent leg here, not every roll. Tap any row to open the full chain detail.

#### The Position Row

Each row shows:

- **Ticker and strategy** (CSP or CC)
- **Strike and expiry date**
- **Premium collected** for that leg
- **DTE countdown ring** — a circular indicator that changes color as expiration approaches:
    - Green — more than 21 days remaining
    - Orange — 8 to 21 days remaining
    - Red — 7 days or fewer remaining

#### Filtering

Use the filter controls at the top of the list to narrow your view:

|Filter|Options|
|---|---|
|**Strategy**|All, CSP, CC|
|**Status**|All, Active, Closed|
|**Date Range**|All, Last 7 Days, Last 3 Months, Last 1 Year|

#### Swipe to Delete

Swipe left on any row to delete it. This removes the **entire chain** — all legs, rolls, and linked history. This action cannot be undone.

#### Position Detail View

Tapping a row opens the detail view, which shows:

- Position metrics (strike, expiry, premium, fees, DTE, annualized ROI)
- Available actions: Roll, Close, Expire, Assign, Edit
- **Chain timeline** — every leg in the chain with pencil icons to edit earlier legs
- **Chain P&L** — "Chain Premiums" for active positions; "Realized Chain P&L" once terminated

> DTE and Annualized ROI are hidden for terminated positions (closed, expired, or assigned).

---

### Shares Held Tab

The Shares Held tab shows positions where you've been **assigned shares via a CSP** and are currently holding them. A position appears here after you record a CSP assignment and disappears if those shares are subsequently called away by a CC assignment.

#### What You See

Each row shows the assigned position's ticker, strike (your cost basis entry point), number of contracts (× 100 = shares held), and the assignment date.

Tapping a row opens the detail view for that CSP assignment, where you can see:

- **Adjusted cost basis** — your effective per-share cost after subtracting all premiums collected across the CSP chain and any linked CC chains
- **Chain P&L** for the full CSP leg history
- **Sell Covered Call** button — tap this to open a new CC position linked to these shares

#### What Doesn't Appear Here

- Positions where a CC assignment has already occurred (shares called away)
- Active CSPs that haven't been assigned yet — those remain in the Positions tab

---

### Summary Tab

The Summary tab provides a **real-time portfolio overview**, calculated from all your transactions at runtime. There are no stored totals — every number recalculates automatically as you add or modify positions.

#### Key Metrics

|Metric|What It Means|
|---|---|
|**Total Premium Collected**|Net premium across all closed, expired, and active legs|
|**Capital at Risk**|Strike × 100 × contracts for all active CSP positions|
|**Capital in Shares**|Strike × 100 × contracts for all held share positions|
|**Realized P&L**|Net premium from all terminated chains (closed + expired + assigned)|
|**Annualized ROI**|Weighted return across active positions|

#### Import / Export _(Pro)_

The **↑↓** button in the top toolbar opens the Import/Export sheet. This is a Pro-only feature. From here you can:

- **Export** your full transaction history as CSV or XLSX
- **Import** a previously exported file in Append mode (adds to existing data) or Replace mode (wipes existing data first — requires confirmation)

---

## FAQs & Troubleshooting

---

### General

**What is the wheel strategy?** The wheel is an options income strategy where you repeatedly sell Cash-Secured Puts on a stock you're willing to own. If assigned, you sell Covered Calls against those shares. The cycle generates premium income at each step. WheelTracker is purpose-built to track this entire lifecycle.

**What's the difference between Close and Expire?** Both terminate a position, but they're different events. **Close** means you bought back the option before expiration — you paid a premium to exit, and that cost reduces your chain P&L. **Expire** means the option expired worthless at expiration — you pay nothing and keep 100% of the premium you originally collected.

**Why do I only see one row per position, even though I've rolled it several times?** The Positions tab shows only the **current active leg** of each chain. This keeps the list focused on what needs your attention right now. To see the full history of rolls, tap the row to open the detail view and scroll to the chain timeline.

**Can I edit a position after saving it?** Yes. The current leg can be edited via the **Edit** button in the toolbar of the detail view. Earlier legs in the chain can be edited via the **pencil icons** in the chain timeline section.

**Why are DTE and Annualized ROI missing from the detail view?** These metrics are only meaningful for active positions. Once a position is terminated (closed, expired, or assigned) they're hidden automatically.

---

### The Wheel Flow

**How do I record a Covered Call after being assigned shares?** Open the detail view for the CSP assignment (find it in the **Shares Held** tab). Tap the **Sell Covered Call** button. This opens a pre-filled form and links the new CC chain back to your assignment so premiums are tracked together.

**What's the difference between "Chain Premiums" and "Realized Chain P&L"?** "Chain Premiums" appears on active multi-leg positions and shows the running total of net premium collected so far across all legs. "Realized Chain P&L" appears once the chain is fully terminated and represents the final net result.

**How is Adjusted Cost Basis calculated?** For CSP assignments, the adjusted cost basis is your strike price minus all net premiums collected across the entire CSP chain and any linked CC chains, divided by shares held. It reflects your true effective entry price after accounting for all premium income.

**Does WheelTracker track stock gain/loss between CSP and CC strikes?** No. WheelTracker captures options premium income only. The difference in share price between your CSP assignment strike and CC sale strike is out of scope and not calculated automatically.

---

### Shares Held Tab

**A position I was assigned isn't showing in the Shares Held tab.** Check that you recorded the assignment using the **Assign** action in the position detail view, not Close or Expire. Only positions with a `.assigned` CSP action appear in the Shares tab.

**My shares are still showing in the Shares Held tab after a CC assignment.** Make sure you recorded the CC assignment using the **Assign** action on the Covered Call position. The app detects called-away shares by looking for a CC assigned record with a later date for the same ticker. If the CC assignment wasn't recorded, the shares will continue to appear.

---

### Data & Import/Export

**How do I back up my data?** Go to the **Summary tab** and tap the **↑↓** button (Pro required). Export as CSV or XLSX and save or share the file via the iOS share sheet. Keep a copy somewhere safe — iCloud Drive, email, or your computer.

**What's the difference between Append and Replace import modes?**

- **Append** adds the imported transactions alongside your existing data. Use this to merge records from another device.
- **Replace** deletes all existing data first, then imports. Use this to restore from a backup. A confirmation dialog appears before any data is deleted.

**I'm getting an error when importing an XLSX file.** WheelTracker's XLSX importer supports STORED (uncompressed) ZIP archives only. If your file was saved by Excel or another app using compression (DEFLATE), the import will fail with an unsupported compression error. Try exporting the file from WheelTracker itself, or re-save it using a tool that supports uncompressed XLSX output.

**Can I import data from another app or a custom spreadsheet?** Yes, as long as the file matches WheelTracker's 14-column schema. Download an export from the app first and use it as a template. Pay close attention to the `exportId` and `parentExportId` columns — these are required for chain relationships to be reconstructed correctly on import.

---

### Subscription

**What's included in the free tier?** The free tier supports up to **5 active positions**. All core tracking features — adding positions, rolling, closing, expiring, assigning, and viewing chain history — are available on the free tier.

**What does Pro unlock?** Pro removes the 5-position limit (unlimited active positions) and unlocks **Import/Export** (CSV and XLSX).

**I purchased Pro but features are still locked.** Tap **Restore Purchases** on the paywall screen. If the issue persists, make sure you're signed into the same Apple ID used for the original purchase.

---

## Contact & Support

Have a question that isn't answered here, or found a bug? I'd love to hear from you.

---

### Report a Bug or Request a Feature

The best way to report an issue or suggest a feature is via GitHub Issues:

**[github.com/dhhh9575/wheeltracker-support/issues](https://github.com/dhhh9575/wheeltracker-support/issues)**

When reporting a bug, it helps to include:

- What you were doing when the issue occurred
- What you expected to happen vs. what actually happened
- Your iOS version and device model

---

### Get in Touch

For general questions or support, reach out by email:

**[support@introbirds.com](mailto:support@introbirds.com)**

---

### Stay Updated

- **App Store:** [WheelTracker on the App Store](https://apps.apple.com/app/YOUR_APP_ID)
- **Release notes** are published with every App Store update


