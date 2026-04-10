# Tariff Flags

Load this file only when a result appears to be direct-from-overseas
(e.g., Temu, AliExpress, Shein, a brand's Chinese storefront, or any
site where the price seems implausibly low and ships from abroad).

---

## The Core Rule

If buying from a **US-based retailer** (Amazon, Walmart, Target, brand's US site):
→ **Tariffs are already priced in.** Do not add them.

If buying **direct from an overseas seller**:
→ Tariffs may or may not be collected at checkout. Flag it.

---

## Current Situation (April 2026)

- Chinese-origin goods face significant Section 301 tariffs
- The **de minimis exemption** (previously allowed goods under $800 to enter duty-free) has been **revoked for Chinese goods** as of May 2025
- This means low-cost direct-from-China orders (Temu, Shein, etc.) now have duties applied — often 120%+ on apparel
- Search "current US tariff rate [item category] China 2026" to get the latest number before quoting

---

## By Category

**Clothing & Apparel (China-origin):**
- Base import duty: 10–32% (varies by material)
- Section 301 additional tariff: up to 145%
- Combined effective rate can exceed 100% of product value
- Practical result: a $20 Temu top may cost $40–50 after duties

**Household Goods (China-origin):**
- Base duty: 3–10%
- Section 301: 25–145% depending on HTS code
- Furniture and home textiles hit hardest

**Non-China Sources:**
- Vietnam, Bangladesh, India: standard MFN rates (5–32% for apparel), no Section 301
- Mexico/Canada (USMCA): generally duty-free
- EU: standard rates, no Section 301

---

## How to Flag in Output

When including a direct-import option, add a warning:

> ⚠️ **Import duty risk**: This seller ships direct from China. After duties
> (currently ~X%), the true cost may be significantly higher than listed.
> US retailers selling the same item have duties already included.

---

## When to Recommend Direct-Import Anyway

Only if:
1. The item is unavailable from US retailers
2. The price advantage survives after duty estimates
3. The user explicitly wants the cheapest possible option regardless
