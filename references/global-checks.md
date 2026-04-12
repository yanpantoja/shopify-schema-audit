# Global Schema Checks

These apply to ALL JSON-LD blocks in the theme.

## 2F. Global Checks

| # | Check | How to verify | Severity |
|---|---|---|---|
| 24 | `@context` uses `https://schema.org` consistently (not `http://`) | Grep for `http://schema.org` | Minor |
| 25 | No hardcoded URLs (use `shop.url`, `request.origin`, Liquid objects) | Grep for hardcoded domains | Major |
| 26 | JSON-LD is rendered before `content_for_layout` (in `<head>` or top of `<body>`) | Check include placement in layout/theme.liquid | Minor |
| 27 | JSON output is valid (no unescaped quotes, proper `| json` filter usage) | Check for `| json` on dynamic string values | Critical |
| 28 | Date fields use `| date` filter (not raw `created_at` output) | Check Article/Blog date fields in schema | Major |
| 29 | Variant selection dynamically updates schema (or ProductGroup covers all) | Check for JS-driven schema updates or ProductGroup pattern | Minor |
