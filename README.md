# Verifyistic — Advanced Age Verification

**Industry-agnostic age verification & age gate for WordPress.**
Built by [WordPressistic](https://wordpressistic.com) · Published on [WordPress.org](https://wordpress.org/plugins/verifyistic/) as `verifyistic` · License: GPL-2.0-or-later

Verifyistic restricts access to any age-gated website — alcohol, cannabis, tobacco, adult content, gaming, gambling, or any regulated industry — with no third-party service required. All verification runs locally on your server.

## Features

- **Three verification modes** — Date of Birth input, Yes/No confirmation, ID & Face document upload
- **Modern popup** — full-screen blur overlay, mobile responsive, WCAG AA contrast defaults, SEO-friendly (content stays crawlable), bot/crawler auto-bypass
- **Fully customizable** — logo, heading, message, button text, full color control, button shapes, custom CSS; color values may reference host-theme `var(--token)`s
- **Verification logging** — name, DOB, age, IP, user agent, page URL, timestamp; admin search/filter; CSV export with CSV-injection guards
- **Webhooks** — multiple connections (Zapier, Make, n8n, Google Sheets, custom endpoints), HMAC-SHA256 signatures, per-connection secrets & event subscriptions, delivery log with retry + exponential backoff
- **Security** — per-IP rate limiting (HTTP 429 + Retry-After), honeypot, HMAC-signed anti-bot timing tokens, cache-fail-open logic for page-cached sites, SSRF-guarded webhook URLs, trusted-proxy-aware IP resolution
- **Privacy** — uploaded IDs stored outside the public uploads tree behind deny-all rules, streamed only through a signed admin-capability endpoint; no remote font or third-party calls
- **Compliance** — minimum age floor hardcoded at 18 (raisable, never lowerable), strict mode to disable evidentially weak Yes/No checks, non-destructive uninstall by default
- **Translation-ready** — full i18n coverage with bundled `.pot`

## Requirements

- WordPress 5.8+
- PHP 7.4+

## Install

1. Download the [latest release zip](../../releases/latest) (or install from WordPress.org: **Plugins → Add New → search "Verifyistic"**)
2. Upload the zip through **Plugins → Add New → Upload Plugin**
3. Activate, then configure under **Verifyistic → Settings**

## Development

```
verifyistic/
├── verifyistic.php     # bootstrap (constants, activation, defaults)
├── uninstall.php       # data cleanup on delete
├── includes/           # 7 core classes (db, security, frontend, ajax, admin, webhook×2)
├── templates/          # admin screens + frontend popup
├── assets/             # CSS + JS
└── languages/          # translation template
```

No build step. Lint PHP with `php -l <file>`. Regenerate the POT with `php make_pot.php` from the local workspace.

## Releases

Versioned as `vMAJOR.MINOR.PATCH` (tags in this repo). Each release attaches the installable `verifyistic-<version>.zip` — same artifact that ships to the WordPress.org SVN directory.

| Tag | Notes |
|-----|-------|
| v1.5.0 | First public release — full Guns2Ammo decoupling, standalone Verifyistic brand styling, WCAG AA defaults, privacy-safe admin fonts, complete uninstall cleanup, translation-ready |

## Support

- Documentation: [verifyistic.com](https://verifyistic.com)
- Support: [wordpressistic.com/support](https://wordpressistic.com/support)

---

© WordPressistic. Licensed under the [GPL-2.0-or-later](LICENSE).
