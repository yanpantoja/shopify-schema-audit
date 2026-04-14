# Return Policy Schema Checks

## 2F. MerchantReturnPolicy

AI agents use this schema to answer customer questions about returns ("do they offer free returns?", "how long do I have to return?") without routing the user to a policy page. Shopify exposes return-related data via `shop.policies` but does **not** emit `MerchantReturnPolicy` JSON-LD automatically — it must be hand-written in the theme.

This schema is also consumed by Google Shopping's free listings to surface return-policy badges in product results.

| # | Check | How to verify | Severity |
|---|---|---|---|
| 39 | `MerchantReturnPolicy` JSON-LD block exists (embedded in `Offer` or as top-level schema) | Grep `.liquid` files for `"@type": "MerchantReturnPolicy"` | Major |
| 40 | `merchantReturnDays` is present and numeric (not empty or zero) | Check hardcoded or metafield-sourced value — ensure it does not render as `"merchantReturnDays": ""` or `0` | Major |
| 41 | `returnPolicyCategory` uses a valid schema.org URL (e.g., `https://schema.org/MerchantReturnFiniteReturnWindow`, `MerchantReturnFiniteReturnWindow`, `MerchantReturnNotPermitted`) | Grep for the full schema.org URL, not just the enum suffix — short-form values are invalid | Major |
| 42 | `applicableCountry` matches the market this schema renders in | For multi-market stores, verify the value varies by locale (`localization.country.iso_code`) rather than being hardcoded per market | Minor |

### Why this matters

Adding `MerchantReturnPolicy` is low-effort — typically 4-6 lines of Liquid inside the existing `Offer` block — and directly affects how AI agents present your store in comparison results. Merchants with missing or malformed return-policy data get paraphrased or omitted from AI responses even when the policy is generous.

Common pitfall: storing the return window in a shop-level metafield as a string (`"30 days"`) instead of an integer (`30`). Schema.org requires `merchantReturnDays` to be a number.

## Scoring

- Returns & Policy weight: /10
- Critical issue: -10, Major: -5, Minor: -2
