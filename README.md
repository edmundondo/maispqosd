# maispqosd

Public demo for the Malawi ISP Tracker — crowdsourced quality-of-service ratings,
live status reports and speed tests for Malawi's internet providers, sourced from
company annual reports, MACRA licensing records and press reporting.

Sibling of [zwispqosd](https://github.com/edmundondo/zwispqosd) (Zimbabwe),
[bwispqosd](https://github.com/edmundondo/bwispqosd) (Botswana),
[saispqosd](https://github.com/edmundondo/saispqosd) (South Africa),
[zaispqosd](https://github.com/edmundondo/zaispqosd) (Zambia) and
[moispqosd](https://github.com/edmundondo/moispqosd) (Mozambique) — same codebase
pattern, same shared Supabase backend (multi-tenant via a `site` column), different
country data. See `zwispqosd`'s README/SETUP docs for the full technical background
on how the backend, live feeds (IODA) and speed test work — nothing about that
plumbing is Malawi-specific.

Formatted report exports (PDF/CSV/EPUB) live on the privileged
[maispqosp](https://github.com/edmundondo/maispqosp) backend, not here — see its
README for details.

## What's different about this site (v1 scope)

- **Language chips: English + Chichewa, with eight more present but blank.**
  Malawi's 1994 Constitution doesn't declare an official language outright, but
  sections 51/56/94 require English competency for Parliament and Ministers,
  structurally entrenching English as the only language with real constitutional
  status. Chichewa has been the national language since 1968 (a Malawi Congress
  Party policy decision, not a constitutional one) and is the most widely spoken
  language nationwide. Chitumbuka was itself official from 1947-1968 before losing
  that status and today has no legal status of its own. Chiyao, Chilomwe, Chisena,
  Chitonga, Chingoni, Chilambya (Chinkhonde) and Chinyakyusa have no official,
  national or legal status. Chichewa ships with a real best-effort-draft
  translation, because it's literally the same standard language (ISO 639-3 nya)
  already drafted for the Mozambique site's Cinyanja content — reused rather than
  re-fabricated, still unreviewed, still flagged with the same "🚧 need
  translation" badge wherever a string hasn't been checked. Chitumbuka, Chiyao,
  Chilomwe, Chisena, Chitonga, Chingoni, Chilambya and Chinyakyusa have no
  cross-border shortcut and no verified source yet, so their chips exist and fall
  back cleanly to English rather than being guessed. Community translation via the
  suggest/endorse flow works for all ten languages today.
- **No backbone (RIPEstat/ASN) badges.** `ISP_ASN` is intentionally empty — no
  verified ASN-to-operator mapping has been compiled for Malawi yet. The badge
  simply doesn't render for any ISP without an entry, the same graceful fallback the
  Zimbabwe site already relies on for its own untracked ISPs.
- **No Cloudflare Radar national benchmark.** That feature calls a Supabase Edge
  Function that's hardcoded server-side to Zimbabwe's numbers only (and is a
  separately-tracked, not-fully-verified feature even there) — rather than build a
  second country-specific proxy sight-unseen, it's simply never invoked on this site.
- **No phone-prefix ISP detection.** `PHONE_ISP_PREFIXES` starts empty — no verified
  MACRA numbering-plan-to-carrier mapping was available for this build. Malawi's
  mobile format (9-digit national number, trunk 0 dropped internationally, TNM=88,
  Airtel Malawi=99/98) is documented in the code even though the prefix table itself
  is empty.
- Provider list is intentionally small (4 tracked providers — Airtel Malawi, TNM,
  MTL, Starlink) and seed ratings/status/speed data is illustrative, not a real
  crowdsourced history.

See `CHANGELOG.md` for version history.
