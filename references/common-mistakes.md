# Common Mistakes (from real Shopify theme projects)

## Okendo/review app metafield `contains` returns false

**Symptom:** `product.metafields.okendo.StarRatingSnippet contains '"averageRating"'` returns `false` even when the HTML has the string.

**Cause:** Metafield drops are objects, not strings. `contains` doesn't coerce.

**Fix:** Use `{% capture %}` first to render as string, then `contains`/`split`.

## Richtext metaobject outputs raw JSON in schema

**Symptom:** `acceptedAnswer.text` shows `{"type":"root","children":[...]}` instead of clean text.

**Cause:** `strip_html` on a richtext metaobject field doesn't work — the value is a structured object.

**Fix (best):** Use `{{ field | metafield_text }}` — Shopify's `metafield_text` filter returns plain text for `rich_text_field` types (per [Shopify docs](https://shopify.dev/docs/api/liquid/filters/metafield_text)).

**Fix (alternative):** `{% capture html %}{{ field | metafield_tag }}{% endcapture %}{{ html | strip_html }}`.

## Hardcoded logo URL returns 404

**Symptom:** Organization schema `logo.url` points to `/logo.png` but returns 404.

**Cause:** Theme uses inline SVG logo, no file at that path.

**Fix:** Use `shop.brand.logo | image_url: width: 300 | prepend: 'https:'` with a blank check.

## Organization sameAs uses section settings instead of brand metafields

**Symptom:** `sameAs` array contains empty strings or breaks when section settings change.

**Cause:** Social URLs pulled from section/theme settings instead of Shopify's native brand data.

**Fix:** Use `shop.brand.metafields.social_links.<platform>.value` (supports: facebook, pinterest, instagram, tiktok, tumblr, snapchat, vimeo). Always wrap in `{% if url != blank %}` guard before adding to the array.

## String comparison on numeric review values

**Symptom:** `aggregateRating` emits with `ratingValue: "0"` when no reviews exist.

**Cause:** Comparing metafield value (string) to number `0` — Liquid doesn't auto-coerce.

**Fix:** Normalize with `| plus: 0` before comparison: `assign rating_num = oke_rating | plus: 0`.

## Wrong metafield key assumed

**Symptom:** Metafield renders blank even though data exists in admin.

**Cause:** Key mismatch (e.g., `custom.lrv` vs `custom.light_reflectance_value`).

**Fix:** Always verify key by checking the admin metafield definition URL or grepping the reference project. Category metafields with hyphens need bracket notation: `product.metafields.shopify['intended-application']`.

## CSS specificity collision on injected schema-related content

**Symptom:** Metafield-driven content (e.g., PDP description) renders with wrong styles.

**Cause:** Content injected inside `metafield-rich_text_field` wrapper inherits its styles.

**Fix:** Use tag-qualified selectors (`p.your-class`) to match or beat the existing specificity.
