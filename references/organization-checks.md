# Organization & Site Schema Checks

## 2D. Organization

| # | Check | How to verify | Severity |
|---|---|---|---|
| 19 | `logo` URL is dynamic (uses `shop.brand.logo`), not hardcoded path | Grep for hardcoded `/logo.png` or similar | Major |
| 20 | `logo` is conditionally omitted when brand assets not configured | Check for `shop.brand.logo != blank` guard | Minor |
| 21 | `sameAs` array has valid social URLs (not empty strings) | Check for blank guards. Prefer `shop.brand.metafields.social_links.<platform>.value` over section settings. | Major |

## 2E. BreadcrumbList

| # | Check | How to verify | Severity |
|---|---|---|---|
| 22 | Exists on product, collection, article, and page templates | Check template conditionals | Major |
| 23 | `position` values are sequential (1, 2, 3...) | Check loop counter | Minor |
| 23a | Hierarchy reflects site structure on PDP (Home → Collection → Product, not just Home → Product) | Verify the schema renders `product.collections.first.title` between Home and the product title | Major |
| 23b | Fallback logic handles products without a collection | Look for `{% if product.collections.size > 0 %}` guard or equivalent — otherwise the schema array breaks on uncategorized products | Major |
| 23c | `item` URLs are absolute (include `shop.url`), not relative paths | Grep `"item": "/"` vs `"item": "{{ shop.url }}/"` — Google and AI crawlers require fully-qualified URLs; relative paths are silently dropped | Critical |

### Why hierarchy matters

AI agents use BreadcrumbList to understand *where* a product fits within your store. A PDP that only emits `Home → Product Title` gives the agent no taxonomy context, which degrades comparison queries like "what moisturizers do they sell?" A theme that includes the first collection (`Home → Moisturizers → Product`) dramatically improves discoverability — it's the cheapest taxonomy signal a theme can provide, because the data already exists in `product.collections`.

## Scoring

- Organization & Site weight: /10
- Breadcrumbs & Navigation weight: /10
- Critical issue: -10, Major: -5, Minor: -2
