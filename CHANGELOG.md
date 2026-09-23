# Changelog — maispqosd

## [1.1.0] — 2026-09-23

### Fixed
- **Tester submissions weren't reaching the backend (critical, since SpamGuard shipped).**
  Every insert (ratings, status reports, speed tests, switch/signup reports, translation
  suggestions, tester feedback) sends SpamGuard's anonymous `device_id`, but that column didn't
  exist in the shared Supabase project, so the database rejected every submission with HTTP 400
  and it stayed in the tester's own browser only. Fixed on the backend (migration
  `add_device_id_antispam_and_open_lang_codes` — see `zwispqosp/supabase-antispam-migration.sql`);
  no data had been reaching the server, so there is nothing to backfill.
- Translation suggestions for this country's own language chips were also blocked by a database
  check that only listed Zimbabwe's language codes — now a format check (same migration).
- The public site no longer reads the admin login that the `*ispqosp` apps store in the shared
  `edmundondo.github.io` origin: the Supabase client here is now stateless
  (`persistSession:false`, own `storageKey`), so an expired admin session can't turn public
  reads into 401s on the same browser.

### Added
- **Offline-tolerant outbox.** A submission that fails for a transient reason (no signal,
  backend down, rate limit) is queued on the device and re-sent automatically on the next visit
  and when the browser comes back online. Genuine validation rejections are not re-sent. The
  banner now says "saved on this device — it will send automatically" instead of a dead-end
  error. Referral clicks now also carry `device_id`.

### Changed
- All inserts go through one `syncInsert()` helper; the speed-test "retry with base columns"
  fallback was removed (the rich columns have existed since v1.1.0's migration).

## [1.0.0] — 2026-09-18

### Added
- First build, replicating the zwispqosd (Zimbabwe) demo/backend split pattern for
  Malawi from day one — no export options here (see `maispqosp` for those),
  brand footer/logo matching the other sites, versioning in place.
- Provider list (`DATA`): Airtel Malawi (confirmed, 8.8M customers, 2025 Annual
  Report), TNM/Telekom Networks Malawi (estimated, ~7M GSM subscribers via press
  paraphrase of FY2025 results), Malawi Telecommunications Limited/MTL
  (unconfirmed, no public subscriber count), and Starlink (unconfirmed, no public
  subscriber count) — with published-vs-derived figures clearly distinguished in
  each entry's `note`/`source`.
- 15 real Malawi towns for GPS/nearest-city matching (Lilongwe, Blantyre, Mzuzu,
  Zomba, Kasungu, Mangochi, Karonga, Salima, Balaka, Mzimba, Dedza, Mchinji,
  Nkhotakota, Ntcheu, Liwonde) with area/suburb lists sourced from Wikipedia, the
  Malawi Electoral Commission's "Proposed Constituencies and Wards with Projected
  Population" report, iPostalCode.com/postzipcode.com postal jurisdiction
  listings, and UN-Habitat's Zomba Urban Profile — each city's sourcing notes and
  confidence level documented inline in `index.html`. Small, clearly-illustrative
  seed QoS/status/speed data across a subset of these towns.
- Language chips: English (full translation) + Chichewa (real best-effort draft,
  reused from the Mozambique site's Cinyanja content — same language, ISO 639-3
  nya) + eight more present but intentionally blank (Chitumbuka, Chiyao,
  Chilomwe, Chisena, Chitonga, Chingoni, Chilambya, Chinyakyusa) — see README.md
  for the full status breakdown (constitutional/national/no-legal-status) behind
  each language's inclusion.
- Malawi-correct phone number handling (`+265`, 9-digit national number, trunk 0
  dropped internationally, network codes 88/98/99) in
  `normalizePhone`/`isValidPhone` — `PHONE_ISP_PREFIXES` itself starts empty, no
  verified MACRA numbering-plan-to-carrier mapping was available for this build.

### Notes — deliberate v1 scope cuts (see README.md for the full list)
- `ISP_ASN` starts empty — no verified ASN-to-operator mapping compiled for this
  build; already degrades gracefully when empty.
- Cloudflare Radar national-benchmark feature not invoked (Zimbabwe-only edge
  function; a second, unverified proxy wasn't built sight-unseen for this release).
