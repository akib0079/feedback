# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

This is a Shopify theme. Use the [Shopify CLI](https://shopify.dev/docs/themes/tools/cli) for all theme operations:

```bash
# Serve the theme locally against a development store
shopify theme dev --store <store-url>

# Push theme to Shopify (production)
shopify theme push

# Pull latest theme from Shopify
shopify theme pull

# Check theme for errors
shopify theme check
```

The Python scripts (`update_*.py`, `fix_css.py`, `resolve.py`) are one-off migration utilities for patching Liquid/JSON files programmatically. Run them directly with `python3 <script>.py` from the repo root when needed.

## Architecture

**Shopify Online Store 2.0 theme** customized for Ziener, a German outdoor/sports brand.

### Directory Structure

| Path | Purpose |
|------|---------|
| `layout/theme.liquid` | Root HTML shell — loads meta-tags, stylesheets, fonts, scripts |
| `sections/` | Reusable page sections (header, footer, product, collection, custom brand pages) |
| `snippets/` | Partial renders called via `{% render %}` from sections/templates |
| `templates/` | JSON templates that wire sections to page types (product, collection, page, etc.) |
| `assets/` | JS modules and CSS files loaded by the theme |
| `config/settings_schema.json` | Theme Editor schema (global settings) |
| `config/settings_data.json` | Active theme settings values |
| `locales/` | Translation strings; `en.default.json` is the base, `de.json` is primary for Ziener |

### Custom Brand Sections

These are Ziener-specific sections not found in a standard theme:

- **`sections/page-teamwear.liquid`** — Tab-based page (Catalog PDF flipbook, Color Overviews image grid, Impregnation Service steps, Repair Service steps). Controlled by blocks in the Shopify theme editor.
- **`sections/page-corporate.liquid`** — Corporate/B2B information page
- **`sections/page-declaration.liquid`** — Declaration/compliance page
- **`sections/page-partners.liquid`** — Partner portal page

These sections are assigned to pages via `templates/page.teamwear.json`, `templates/page.corporate.json`, etc.

### Section Block Pattern

Custom sections use Shopify's block system for editor-configurable content. Blocks are iterated in Liquid with `{% for block in section.blocks %}` and typed by `block.type`. The tab content type (`block.settings.tab_content_type`) drives conditional rendering within a single tab block.

### Assets Architecture

JS is organized as ES modules per component (e.g., `assets/cart-drawer.js`, `assets/accordion-custom.js`). Global styles are in `assets/base.css`; overrides in `assets/custom.css`.

### Localization

Primary languages: German (`de.json`) and English (`en.default.json`). Schema translation keys are in `*.schema.json` files alongside each locale.
