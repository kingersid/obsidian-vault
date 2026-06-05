---
tags: [woocommerce, vps, infrastructure, label-ethnic-vogue, technical]
source: raw/woocommerce-setup-analysis.md
ingested: 2026-05-24
type: technical-reference
---

# WooCommerce Setup — Label Ethnic Vogue

## Server
| Key | Value |
|-----|-------|
| VPS IP | 103.194.228.56 |
| OS | Ubuntu 24.04 |
| Stack | WordPress + Nginx + MariaDB (Docker) |
| Port | 8080 |
| PHP | 8.1.34 |
| Nginx | 1.27.5 |

## Database
| Key | Value |
|-----|-------|
| DB Name | wordpress |
| DB User | wordpress |
| DB Host | db (MariaDB container) |

## Plugins
**Active:**
- WooCommerce (`woocommerce/woocommerce.php`)
- Theme: Storefront

**Inactive:**
- Akismet
- Hello.php (WordPress default)

## Performance
- Load time: 0.24s
- Page size: 77,544 bytes
- HTTP 200 OK

## Related
- [[woocommerce-index]]
- [[woocom-wiki-log]]
- [[woocom-wiki-schema]]
- [[woocommerce-scaling]]
