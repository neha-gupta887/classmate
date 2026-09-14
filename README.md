# CampusMate

An independent student-service-provider website for the Chandigarh University community. The current scope is a public, WhatsApp-first website, agreed after discussing the full-stack limitations of this workspace. It is **not** the original authenticated student/admin platform.

## Current features

- Original responsive identity: deep green, warm neutral surfaces, muted yellow accents, Manrope and DM Sans typography, inline SVG icons.
- Landing page with hero, public service discovery, how it works, benefits, custom enquiry, FAQ, final contact CTA, and footer.
- WhatsApp enquiries addressed to **+91 89577 46102**, using `https://wa.me/918957746102`.
- Custom enquiry fields: name, category, title, detailed requirements, optional deadline and budget.
- Client-side validation for required fields, whitespace-only entries, text lengths, past dates, and budget limits.
- Review dialog before opening WhatsApp. Nothing is claimed sent: visitors must press **Send in WhatsApp**.
- Services fetched from the public Git-managed catalog file, never invented or hardcoded in rendering logic.
- Search across title, short/full description, subject, and category; category, price and delivery filters; price/newest sorting; six-card pagination.
- Active-only display, detailed service dialog, service-specific WhatsApp message, and shareable service-detail URL.
- Loading state, empty catalog, no-match/reset state, network error/retry, and unavailable-service handling.
- Responsive mobile navigation, visible keyboard focus, skip links, semantic structure, labelled inputs, native modal-dialog focus handling, reduced-motion support.
- Privacy notice, service terms, academic-integrity guidance, and clear non-affiliation notice.
- No fake login, admin dashboard, transactions, chat, tracking, or notifications.

## Entry points

| URI | Function |
| --- | --- |
| `/` or `/index.html` | Landing page |
| `/index.html#services` | Searchable public catalog |
| `/index.html?service=<service-id>` | Opens an active catalog service in its detail dialog |
| `/index.html#how-it-works` | Enquiry process |
| `/index.html#why-campusmate` | Platform approach |
| `/index.html#custom-request` | Custom enquiry form |
| `/index.html#faq` | FAQ |
| `/privacy.html` | Privacy notice |
| `/terms.html` | Terms of service |
| `/data/services.json` | Public service-catalog document |
| `/tests/browser.html` | Explicitly labelled developer browser checks, noindex; no records saved or messages sent |

No production URL has been assigned or verified. Nothing has been deployed in this session. Publish through the project's **Publish tab** when ready. There is no custom backend API or public account-registration endpoint.

## Files

```
index.html
privacy.html
terms.html
css/style.css
js/main.js
data/services.json
tests/browser.html
tests/browser.js
README.md
.tables/schema.json   # platform-managed, reserved schema only
```

## Catalog data and publishing

`data/services.json` is the **current source of truth**. It is intentionally empty because the owner has not provided confirmed services, prices, delivery estimates, or descriptions. The visible categories are enquiry categories, not promises that services are available.

Only editors with project/repository publishing access update this catalog. There is **no visitor-facing write endpoint or client-side admin editor**. Catalog changes become live only after publishing again. Deactivation hides cards but does not make records in this public JSON confidential.

Document shape:

```json
{ "services": [] }
```

Each service supports these fields:

| Field | Type | Notes |
| --- | --- | --- |
| `id` | string | Required unique stable identifier; used in detail links |
| `title` | string | Required public title |
| `slug` | string | Optional reserved human-readable identifier |
| `shortDescription` | string | Card summary |
| `fullDescription` | string | Detail text; HTML deliberately not rendered |
| `category` | string | `Academic Support`, `Career & Skills`, `Design & Creative`, or `Everyday Essentials` |
| `subject` | string | Optional; not required for non-academic services |
| `price` | number | Nonnegative INR amount; omit for a quote |
| `pricingType` | string | `fixed`, `from`, or `quote` |
| `deliveryHours` | number | Positive estimate; omit if to be agreed |
| `requirements` | string | One item per newline |
| `instructions` | string | Additional public instructions |
| `imageUrl` | string | Optional HTTPS image URL; otherwise category icon |
| `active` | boolean | Only exact `true` records are displayed |
| `createdAt` | string | ISO timestamp for newest sorting |
```

A platform-managed `services` table schema was prepared during initial setup, but **the site does not read or write it** and it contains no seed rows. It is not the catalog's current source. Using an unrestricted table write API for a supposedly admin-controlled catalog would not be an appropriate security boundary. No credentials or private student data should be put in this unused table. A future secured backend may replace the public catalog-file approach.

The public catalog is fetched once and filtered/paginated in the browser, appropriate for a small provider catalog. This is not server-side database pagination; migrate to a secured, indexed catalog API when scale requires it.

## Enquiry data and privacy

- Form details exist only in the current page while composing an enquiry.
- No localStorage, cookies, database writes, website uploads, or account sessions are created by application code.
- Clicking the final WhatsApp link passes the prepared message in an encoded URL to WhatsApp. That URL can be stored in browser history.
- Only the visitor can send the message from WhatsApp. The website cannot verify delivery, recipient availability, replies, acceptance, price agreement, payment, or completion.
- Supporting documents are shared directly in WhatsApp, not uploaded to the website.
- The proprietor manages subsequent correspondence and records in WhatsApp under applicable privacy obligations.
- Catalog text and enquiry text are HTML-escaped before insertion into the DOM; service image URLs are limited to HTTPS.
- All catalog data is public. Never add private student information or secrets to this repository or catalog.

## External resources and environment

- Fonts: Google Fonts (`fonts.googleapis.com`, `fonts.gstatic.com`).
- Hero photograph: Unsplash image CDN (`images.unsplash.com/photo-1523240795612-9a054b0db644`). A general student-life image, not asserted to depict CU.
- Contact: WhatsApp click-to-chat endpoint, no API credentials or WhatsApp Business API integration.
- No build step, environment variables, package installation, external JavaScript library, or server runtime is required for this public site.
- Secrets in frontend `.env` files would not be safe; none are used or committed.

## Verification

- Real `index.html` visually captured at **1280px desktop** and **390px mobile**; tool reported no overflow, broken layout, visible code, or broken hero image. Full-page capture tools cap scroll height, so captures do not establish every below-fold state on long mobile pages.
- Main-page browser console capture: no logged errors.
- **44/44 browser checks passed** in the final automated run of `/tests/browser.html`. The harness checks real DOM events and pure catalog helpers, including menu controls, catalog searching/filtering/sorting, availability, input validation, message content/recipient, escaping, modal handling, and section anchors.
- Additional isolated catalog fixtures are supplied by temporarily intercepting fetch **only inside the developer test iframe**. They test cards, pagination, detail links, empty results and recovery from network failure, then restore the real empty catalog. They are not published services or persisted data.
- No real WhatsApp enquiry has been sent by automated tests. Device-specific WhatsApp handoff and recipient ownership need manual owner verification.
- Exact 1440px, 1024px and 768px rendering has not been verified by the available screenshot tool. CSS includes adaptive breakpoints; verify these widths in a browser before launch.

## Not implemented / outside revised scope

Secure student/admin accounts; password hashing and recovery; role-based or per-record authorization; student/admin dashboards; request database persistence; unique request numbers; service management UI; order tracking; private file storage; in-app messaging and notifications; payment processing; production analytics; transactional audit history. None are simulated or advertised as existing features.

## Recommended next steps

1. Supply actual service titles, descriptions, prices or quote rules, categories, delivery estimates, requirements, and images. Publish only genuine supported offerings.
2. Confirm ownership and WhatsApp availability of +91 89577 46102. Manually send a test enquiry from a phone and WhatsApp Web.
3. Finalize operator legal identity, contact address, service-specific payment/cancellation/refund terms, and data-retention practices; review legal notices before accepting paid work.
4. Check the complete website at 1440, 1024, 768, and 390px plus keyboard and screen-reader navigation.
5. Publish and repeat link/enquiry checks on the actual live domain. Add a canonical URL, sitemap and domain-specific Open Graph image after a domain is assigned.
6. If the original authenticated platform is still required, use a full-stack environment with server-enforced roles/ownership, transactional PostgreSQL records, secure sessions, private object storage, and integration/security tests. Do not bolt a fake credential gate onto this site.
