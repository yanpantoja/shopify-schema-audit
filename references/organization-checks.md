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

## Scoring

- Organization & Site weight: /10
- Breadcrumbs & Navigation weight: /10
- Critical issue: -10, Major: -5, Minor: -2
