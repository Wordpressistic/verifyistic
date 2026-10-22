=== Verifyistic — Advanced Age Verification ===
Contributors: developershuvo
Tags: age verification, age gate, age check, popup, compliance, access restriction
Requires at least: 5.8
Tested up to: 7.1
Stable tag: 1.5.0
Requires PHP: 7.4
License: GPLv2 or later
License URI: https://www.gnu.org/licenses/gpl-2.0.html

Industry-agnostic age verification for WordPress. Modern popup with Date of Birth, Yes/No, and ID & Face verification modes, logging, and webhooks.

== Description ==

**Verifyistic** is a production-ready age verification and age-gate plugin for WordPress. Use it on any website that must restrict access by age — alcohol, cannabis, tobacco, adult content, gaming, gambling, or any regulated industry — with no third-party service required.

= Key Features =

* **Three Verification Modes:**
  * Date of Birth Input (with optional name fields)
  * Yes / No Confirmation
  * ID & Face Document Upload

* **Modern Popup Design:**
  * Full-screen blur overlay (0–100% opacity control)
  * Mobile responsive & WCAG-aware (AA contrast defaults)
  * SEO-friendly — content stays crawlable by search engines
  * Bot/crawler auto-bypass (Googlebot, Bingbot, etc.)
  * Smooth CSS animations, scroll lock

* **Fully Customizable:**
  * Upload your logo
  * Custom heading, message, and button text
  * Full color control (popup background, overlay, buttons, fonts)
  * Button shape selector (Rounded, Pill, Sharp)
  * Custom CSS injection
  * Color values may reference your theme's CSS custom properties (`var(--token)`)

* **Cookie & Session Management:**
  * Persistent verification cookie (configurable duration)
  * Session-only option (no remember me)
  * "Remember Me" checkbox toggle

* **Verification Logging:**
  * Logs: name, DOB, age, IP, user agent, page URL, timestamp
  * COPPA-safe: no data stored for failed DOB checks
  * Admin search, filter, and paginated log table
  * One-click CSV export (CSV-injection guarded)

* **Webhook / API Integration:**
  * Multiple webhook connections — fan out to Zapier, Make, n8n, Google Sheets, or custom endpoints
  * HMAC-SHA256 signature header for security
  * Per-connection secrets and event subscriptions (passed / failed / declined)
  * Delivery log with automatic retry and capped exponential backoff
  * Built-in webhook test button

* **Security & Abuse Protection:**
  * Per-IP rate limiting with HTTP 429 + Retry-After
  * Honeypot field and signed anti-bot timing tokens
  * Trusted-proxy-aware client IP resolution (Cloudflare ranges preconfigured)
  * SSRF guard on webhook URLs (private/loopback IPs rejected)
  * Uploaded IDs stored outside the public uploads tree with deny-all protection

* **Compliance:**
  * Minimum age floor hardcoded at 18 (admins can raise it, never lower it)
  * Strict mode: disable the evidentially weak Yes/No path
  * Non-destructive uninstall by default (audit log kept unless you opt in)

= Built by WordPressistic =

Verifyistic is designed, developed, and maintained by [WordPressistic](https://wordpressistic.com).

== Installation ==

1. Upload the `verifyistic` folder to `/wp-content/plugins/`
2. Activate through the **Plugins** screen in WordPress admin
3. Navigate to **Verifyistic → Settings** to configure
4. Set your minimum age, choose verification mode, upload your logo
5. Done! The popup activates on all frontend pages

== Frequently Asked Questions ==

= What is the minimum age I can set? =

The minimum age has a hardcoded floor of 18 and cannot be lowered. You can raise it to any value up to 99 — for example 21 where your industry or jurisdiction requires it.

= Is the site content crawlable by search engines? =

Yes. The popup is a CSS/JS overlay only. All page content remains in the HTML and is fully crawlable. Known bots are auto-bypassed.

= Can I exclude certain pages? =

Yes. In Settings → General, add comma-separated page IDs to exclude.

= Does it work with WooCommerce? =

Yes. The popup works on all WordPress pages including WooCommerce shop, product, and checkout pages.

= Where are ID uploads stored? =

ID and selfie uploads are stored in `/wp-content/uploads/private/verifyistic-ids/` with year/month subfolders, protected by a deny-all `.htaccess`. Files are served only through a signed, admin-capability-gated endpoint. On nginx hosts, add a server-block rule to deny direct access to that path.

= Does this require any third-party API? =

No. All verification is handled locally on your server. The webhook integration is optional and connects to endpoints you control.

= What happens when I delete the plugin? =

By default, your verification log and webhook delivery history are preserved. If you enable "Delete verification records on uninstall" in Settings → Advanced, uninstall will also remove all tables and uploaded ID documents.

== Screenshots ==

1. Dashboard — verification stats and recent logs
2. Settings — General configuration panel
3. Settings — Design & appearance customization
4. Settings — Advanced & webhook integration
5. Verification Logs — full audit table with export
6. Frontend popup — Date of Birth mode
7. Frontend popup — Yes/No mode
8. Frontend popup — ID & Face upload mode

== Changelog ==

= 1.5.0 =
* First public release on the WordPress.org plugin directory.
* Branding: the plugin is now fully industry-agnostic — popup defaults, admin UI, and copy no longer reference any specific industry. Color defaults reworked to the Verifyistic palette with WCAG AA contrast (dark ink on the teal accent button instead of low-contrast white).
* Privacy: removed the remote Google Fonts import from the admin stylesheet — the admin UI now uses a privacy-safe system font stack, so no admin IP is disclosed to any third party.
* Uninstall cleanup: all plugin options are now removed on uninstall (previously several — including the webhook connection list, cookie domain, strict mode, and trusted proxies — were left behind). When destructive uninstall is enabled, uploaded ID documents are now deleted too.
* Translation-ready: the frontend popup and admin screens are now fully wrapped in WordPress i18n functions and a translation template is bundled.
* Removed an unused minified stylesheet from the package.
* Version header, version constant, and stable tag are now kept in sync.

= 1.4.7 =
* Fix: age-gate popup and admin settings page color-contrast issues in dark/light mode, including a required-field-asterisk color and a dynamic-CSS color sanitizer that was permanently overriding the plugin's own theme integration with fixed hex values instead of the live theme tokens.

= 1.4.6 =
* Hotfix: client IP resolution now trusts Cloudflare/forwarded headers only when the origin request came from a trusted proxy, with IPv4 and IPv6 CIDR support.
* Hotfix: token minting, failed verification attempts, and successful verifications use separate limiter buckets.
* Hotfix: real rate limits return HTTP 429 with `Retry-After`; lock timeouts no longer create false shared-user 429 responses.
* Hotfix: frontend token refresh uses short session storage caching and forces a fresh token only after failed/stale submissions.

= 1.4.5 =
* Uninstall is now non-destructive by default: the age-verification log and webhook delivery history are kept when the plugin is deleted, unless an admin explicitly opts in via a new "Delete verification records on uninstall" toggle on the Settings page.

= 1.4.1 =
* FIX: Visitors on page-cached sites were walled off with "Network error. Please try again." and could not enter at all. The security nonce baked into cached HTML expires after the WordPress nonce lifetime; once stale, every verify request was rejected with a 403 — and the token-refresh endpoint required that same stale nonce, so there was no way to recover. The refresh endpoint no longer requires a nonce (it only mints anti-abuse material and is now per-IP rate limited) and returns a fresh nonce along with the fresh token. The popup JS refreshes both on show, and a submit that still hits a 403 silently fetches fresh credentials and retries once instead of dead-ending on the error message.

= 1.3.2 =
* FIX: Customers on page-cached sites (WP Rocket, LiteSpeed, Cloudflare, etc.) could be permanently blocked at the age gate with "Please take a moment to complete the form and try again", even with a valid date of birth. The anti-bot timing token baked into cached HTML was arriving stale, expired, or already burned by an earlier visitor and was treated as a hard failure. The token is now a soft anti-bot signal: a stale/expired/burned/missing token fails open (the nonce, honeypot, per-IP rate limit and DOB age check remain the hard gates), while a correctly-signed token submitted impossibly fast for a human is still hard-blocked as a bot.

= 1.2.0 =
* NEW: Mobile-friendly Date of Birth picker — replaced the native date input with fast Month / Day / Year dropdowns (no more endless year-scrolling on phones). Validates real calendar dates client- and server-side.
* Theme-skinnable popup (the host theme can restyle the .vfy-* elements).

= 1.1.0 =
* NEW: Multiple webhook connections — fan verification data out to unlimited platforms (Zapier, Make, n8n, CRM, Google Sheets, custom endpoints), each with its own URL, HMAC secret, enabled flag and per-event subscription.
* NEW: Webhook delivery log + automatic retry with capped exponential backoff.
* NEW: Hardened verification layer — per-IP rate limiting, honeypot field, HMAC-signed anti-bot/anti-replay timing token, and a DOB sanity ceiling.
* Legacy single webhook is auto-migrated into a "Primary" connection.
* Fix: removed a duplicated code block in the old single-webhook sender.

= 1.0.0 =
* Initial release
* Three verification modes: DOB, Yes/No, ID & Face
* Full admin dashboard with stats
* Settings panel with tabbed UI
* Verification log with CSV export
* Webhook integration with HMAC signature
* Cookie and session management
* Accessible, SEO-friendly popup

== Upgrade Notice ==

= 1.5.0 =
* First WordPress.org release. Industry-agnostic branding, privacy-safe admin fonts (no remote Google Fonts), complete uninstall cleanup, full translation readiness.
