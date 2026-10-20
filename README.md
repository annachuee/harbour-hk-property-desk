# Harbour — Hong Kong Property Data

A Traditional Chinese, Hong Kong first proptech agency desk for property CRM workflows, market signals, WhatsApp sharing, and multi portal listing cleanup.

**Live data is not bundled.** The dashboard ships with clearly illustrative sample records. Import a CSV from an authorized source to try the workflow. Connectors for property portals should use their official APIs, licensed feeds, or exports that you are permitted to process. Check each portal's terms and access rules before collecting data.

## Product direction

Harbour is an early Hong Kong-first agency operating system concept: a property and client CRM connected to local market intelligence. It brings together the day-to-day sales workflow (enquiries, applicant matching, follow-ups and progression) with prospecting workflows (market signals, local campaigns and opportunities), while using address resolution to keep the underlying property record clean. This draws on broad workflow patterns seen in products such as [Street](https://street.co.uk/product/sales) and [Spectre](https://spectre.uk.com/product/sales); Harbour is an independent prototype and is not affiliated with either company.

The current prototype demonstrates the agency desk, pipeline stages, follow-up checklist, market-signal campaign actions, multi-source listing inventory, and fuzzy duplicate review. WhatsApp actions use WhatsApp click-to-chat with prefilled messages; the agent chooses a recipient and sends manually. CRM records, deal stages and signals are sample data. There is no live portal connection, backend, automated WhatsApp Business messaging, or client account system yet.

## Features

- Import and export CSV listings
- Normalize noisy English and Chinese address strings for comparison
- Fuzzy similarity scoring using normalized edit distance and token overlap
- Flag candidate duplicates for human review; merge or dismiss each candidate
- Flag incomplete records for data quality review
- Search and filter the listing table
- Responsive dashboard with source health, activity chart, and review queue
- Runs entirely in the browser; imported records are not sent to a server

## Run locally

No build step or package install is needed. Open `index.html` in a browser, or serve the folder:

```sh
python3 -m http.server 8000
```

Then visit <http://localhost:8000>.

## WhatsApp sharing

The one-click WhatsApp actions open an official `wa.me` click-to-chat link. A property-specific draft is prefilled, then the agent selects the recipient and reviews and sends it in WhatsApp. This requires no credentials in the browser. Automated outbound messaging requires an approved WhatsApp Business Platform setup and a secure backend.

## CSV columns

Accepted header aliases include:

| Field | Accepted headers |
| --- | --- |
| Address | `address`, `raw_address`, `full_address`, `location` |
| Listing name | `name`, `property`, `building`, `title` |
| Price | `price`, `asking_price` |
| Size | `size`, `area_sqft`, `sq_ft`, `saleable_area` |
| District | `district`, `area`, `region` |
| Source | `source`, `portal`, `agency` |
| Listing ID | `id`, `listing_id` |
| URL | `url`, `link` |

Price and size are parsed as numbers. Unknown columns are ignored. Export produces a consistent Harbour CSV format.

## Matching behavior

The demo normalizer case-folds, strips punctuation, collapses whitespace, and removes common address descriptors while retaining identifying numbers and place names. Similarity combines normalized Levenshtein distance and token overlap. Candidate scores receive a small boost when sources differ and prices are close. A score of 78 or higher enters the review queue. This is a triage heuristic, not proof two records represent one property: review the address, unit, price, and source before merging. The normalization and scoring helpers are in `app.js` and can be replaced with production-grade locale-aware rules.

## Production architecture

For a real multi-portal service, add a backend ingestion queue, per-source licensed/API adapters, an auditable raw-record store, address/building entity resolution, confidence thresholds and a human review trail. Keep credentials server-side. Do not bypass logins, CAPTCHAs, rate limits, or technical access controls. Respect source terms, privacy obligations, and applicable Hong Kong requirements.

## License

MIT. See [LICENSE](LICENSE).
