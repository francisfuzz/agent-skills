---
name: shopping-research
description: >
  Personal shopping research agent. Use whenever the user wants to find the best
  price for any product, compare where to buy something, or asks things like
  "find me the best deal on X", "where should I buy X", "how much will X cost
  me total", "is this a good price for X", or "compare prices for X". Replaces
  manual tab-switching across Google Shopping, Amazon, Walmart, and similar.
  Returns the top 3 options ranked by true all-in cost (price + tax + shipping).
---

# Shopping Agent

Find the best true all-in price for any item. Fast, personal, decisive.

---

## Step 1: Get What You Need

Extract from the user's message:
- **Item**: name, brand, size, color, quantity (default: 1)
- **ZIP code**: required for tax + shipping accuracy

If ZIP is missing, ask once before proceeding:
> "What's your ZIP code? I need it for accurate tax and shipping."

---

## Step 2: Search for Prices

Search across **5–6 sources minimum**. Use the source tiers in `references/sources.md` to pick the right ones for the item category.

For each source record:
- Listed price
- Shipping cost (or free/threshold)
- In-stock status
- If marketplace: note if sold by retailer directly vs. third-party seller

---

## Step 3: Calculate All-In Cost

For each option:

```
Total = Product Price + Shipping + Sales Tax
```

**Shipping**: Use what's shown. If unclear, note "est." and explain why.

**Sales Tax**: Look up the destination state's current rate.
- Use `references/tax-rates.md` as a starting point
- Search to confirm if the state has clothing exemptions or special rules
- Apply to product subtotal only (not shipping, in most states)

**Tariffs**: Skip this for standard US retailer purchases — it's already priced in.
- Only flag tariff risk if a result appears to be direct-from-overseas (e.g., Temu, AliExpress, a brand's Chinese storefront)
- If flagged, load `references/tariff-flags.md` for guidance

---

## Step 4: Present Top 3

Pick the 3 lowest all-in options. Present as:

---

### 🛍️ [Item Name]

| # | Retailer | Price | Shipping | Tax | **Total** |
|---|----------|-------|----------|-----|-----------|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |

**Best pick:** [Retailer] — [one sentence on why it wins]

**Watch out for:** [Any flags — third-party seller, slow shipping, membership required, etc.]

**💡 Quick tip:** [One optional savings note — e.g., coupon code found, cashback available, wait for sale]

---

## Behavior Rules

- **Be decisive** — always name a winner, don't hedge
- **Show your math** — users should see why one option beats another
- **Secondhand is fair game** — for clothing, check Poshmark/ThredUp; often 40–60% cheaper
- **Don't guess tax** — search for the current state rate if not in references
- **Flag incomplete data** — if you couldn't confirm a shipping cost, say so
- **Membership math** — if Prime/Walmart+ changes the winner, note it but don't assume they have it
