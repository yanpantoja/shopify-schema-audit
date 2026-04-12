# shopify-agentic-audit

Audit Shopify Liquid themes for AI agent discovery readiness (Agentic Storefronts, ChatGPT, Google AI Mode, Perplexity, Microsoft Copilot).

An [agent skill](https://github.com/vercel-labs/skills) for Claude Code, Cursor, Codex, and other AI coding tools.

## What it does

Runs a structured 4-phase audit on your Shopify theme codebase:

1. **Discovery** - Finds all JSON-LD structured data implementations
2. **Schema Validation** - Checks 29 items across Product, AggregateRating, FAQPage, Organization, BreadcrumbList, and global patterns
3. **Agentic Storefront Readiness** - Validates store config for AI agent discovery
4. **Report Generation** - Outputs an `AGENTIC_READINESS_REPORT.md` with score (0-100), issues by severity, and fix suggestions

## Install

```bash
npx skills add yanpantoja/shopify-agentic-audit
```

## Usage

After installing, the skill activates automatically when you work on a Shopify Liquid theme project. You can also invoke it directly:

- **Full audit** (new project): "Run a full agentic readiness audit on this theme"
- **Schema check** (during dev): "Check the JSON-LD schema I just added"
- **Pre-deploy gate** (before PR): "Validate structured data before I open a PR"

## What it checks

| Category | Checks | Weight |
|----------|--------|--------|
| Product / ProductGroup schema | ProductGroup for variants, brand type, GTIN, pricing, availability | /30 |
| AggregateRating | Review app integration, numeric gating, metafield coercion | /15 |
| FAQPage | Structure, richtext handling, JSON comma logic | /15 |
| Organization & Site | Logo source, sameAs social links, WebSite + SearchAction | /10 |
| BreadcrumbList | Template coverage, position sequencing | /10 |
| Agentic Storefront Config | Policies, GTIN exposure, taxonomy, guest checkout | /20 |

## Common Mistakes it catches

Based on real bugs encountered in production Shopify themes:

- Okendo/review app metafield `contains` returning false (needs `{% capture %}` first)
- Richtext metafields outputting raw JSON in schema (use `| metafield_text` filter)
- Hardcoded logo URLs returning 404 (use `shop.brand.logo`)
- String comparison on numeric review values (normalize with `| plus: 0`)
- Organization `sameAs` with empty strings (use `shop.brand.metafields.social_links`)
- Wrong metafield keys (e.g., `custom.lrv` vs `custom.light_reflectance_value`)

## Compatibility

Works with any AI coding tool that supports the [agent skills](https://github.com/vercel-labs/skills) standard:

- Claude Code
- Cursor
- OpenAI Codex
- GitHub Copilot
- Gemini CLI
- Windsurf, Cline, and others

## License

MIT
