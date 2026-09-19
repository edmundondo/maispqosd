# Changelog — maispqosd

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
