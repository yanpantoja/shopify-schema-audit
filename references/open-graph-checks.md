# Open Graph Meta Tag Checks

## Scope note

Open Graph tags are **not** JSON-LD — they're `<meta>` tags in the `<head>`. They're included in this audit because AI agents, social platforms, and search engines use them as a **parallel structured signal** when the JSON-LD is incomplete or absent. A product page without OG tags loses previews in link shares and is less visible to agents that crawl lightweight metadata first (ChatGPT browsing, Slack/Discord link previews, Facebook/LinkedIn shares).

Shopify themes typically inherit OG tags via `{{ content_for_header }}` in `layout/theme.liquid`, but customizations, app blocks, and custom pages frequently break or omit them. That's where bugs live.

## 2G. Open Graph Tags

| # | Check | How to verify | Severity |
|---|---|---|---|
| 43 | `og:title` is present and uses the page-specific title (not the shop name) on product and article pages | View page source or grep `.liquid` for `og:title` — ensure content resolves to `product.title` / `article.title` / `page.title`, not `shop.name` on every page | Major |
| 44 | `og:description` is present and uses product description or page excerpt (not a single default shop tagline repeated on every page) | Grep for `og:description` — the Liquid expression must differentiate by template (PDP → product, blog → article, etc.) | Major |
| 45 | `og:image` is present on product and article pages, with an absolute URL (full `https://`) | Grep for `og:image` — verify `image_url` filter with explicit `width:` and absolute path (not protocol-relative `//cdn.shopify.com/...`) | Major |
| 46 | `og:url` is set to the canonical URL for the current page | Check `og:url` uses `canonical_url` or `shop.url + request.path` — not a hardcoded domain | Minor |
| 47 | `og:type` is `product` on PDP, `article` on blog posts, `website` elsewhere | Grep for conditional on `request.page_type` or template-specific overrides | Minor |

### Common failures

- Default Shopify OG tags work on PDP but break on custom sections/pages where the theme overrides `<head>` without including `{{ content_for_header }}`.
- Apps (live chat, review widgets, analytics) injecting their own `<meta>` tags that duplicate or override OG tags — causing different crawlers to read different data.
- Using `product.featured_image.src` directly instead of `product.featured_image | image_url: width: 1200` — resulting in protocol-relative or sized-down URLs rejected by some crawlers.
- Missing `og:image` on article/blog templates because the theme only implements it for products.

## Scoring

- Open Graph weight: /10
- Critical issue: -10, Major: -5, Minor: -2
