# cloud-itonami-lei-549300lsgbxpyhxgdt93

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by WPP plc.**

This repository archives the publicly published Terms of Use / Terms and Conditions of
**WPP plc**, with source-url and retrieval-date provenance, per
[ADR-2607110300](https://github.com/com-junkawasaki/root/blob/main/90-docs/adr/2607110300-cloud-itonami-lei-corporate-tos-catalog.md)
(`cloud-itonami-lei-corporate-tos-catalog`, `com-junkawasaki/root`). It is a read-only
reference/archive repository — it does not act, propose, or execute anything on the
company's behalf, and is not a governed Advisor/Governor actor.

## Company identity

- **Legal name**: WPP plc — the live GLEIF record spells it `WPP PLC` (language `en`,
  last updated 2026-08-11) and also lists `WPP 2012 PLC` as a `PREVIOUS_LEGAL_NAME`.
  The previous name is read from the registry's `otherNames` field, which the vendored
  checker does not record, so it appears in this README only and is not in `facts.edn`.
- **LEI (ISO 17442)**: [549300LSGBXPYHXGDT93](https://search.gleif.org/#/record/549300LSGBXPYHXGDT93) (GLEIF-verified)
- **Jurisdiction**: JE — the live GLEIF record places the entity in Jersey, registered
  with the Jersey Financial Services Commission Companies Registry (`RA000414`, file
  number `111714`, ISO 20275 legal form `JOX1` Public Limited Company), created
  2012-10-25; legal address 22 Grenville Street, St. Helier, JE4 8PX. This README and
  `blueprint.edn` previously said `GB`, which is where the registry's **headquarters**
  address is (Sea Containers House, 18 Upper Ground, London SE1 9GL, `GB-LND`) — a
  different field from the jurisdiction. `facts.edn` below carries the registry's answer
  with provenance, and the checker flags `GB` as drift today.
- **Website**: https://www.wpp.com
- **Ticker**: WPP (LSE) — a listing named here from discovery context, not read from
  GLEIF. GLEIF maps **1 ISIN** to this LEI (`JE00B8KF9B49`, mirrored in `facts.edn`);
  which market it trades on is not something GLEIF answers, so nothing beyond the
  identifier is asserted.

## Contents

- `80-data/public/tos.journal.edn` — EDN quad-log of archived Terms of Use documents,
  each entry carrying `:tos/full-text`, `:tos/source-url`, `:tos/retrieved-at`,
  `:tos/sha256`, `:tos/doc-type`, and a `:tos/supersedes` chain for future revisions.
- `NOTICE` — copyright/attribution statement for the archived third-party text.
- `blueprint.edn` — machine-readable company identity record.
- `facts.edn` — 34 verified registry facts with per-fact provenance (the entity, its
  securities count and the 1 identifier behind it, issuer and issuer accreditation,
  registration authority, legal form, both parent-reporting exceptions, the
  direct-children count and the 24 children behind it). **Generated** — see below.
- `scripts/verify-facts.cljs` — re-fetches every source `facts.edn` cites and fails if
  the live record disagrees. Vendored from `com-junkawasaki/root`
  (`scripts/lei-verify-facts.cljs`); fix issues in the canonical and re-vendor.

## Verifying the record

The LEI claims above used to be assertions with nothing in the repository behind them —
and one of them (the jurisdiction) was wrong. `facts.edn` now carries them as data, and
every value in it was read out of a public registry response whose URL and retrieval
time sit next to the value:

```
nbb scripts/verify-facts.cljs           # check the recorded facts against the live sources
nbb scripts/verify-facts.cljs --write   # re-fetch and rewrite facts.edn
```

Twelve GLEIF/ISO requests back the file (`CHECKED 12` when it was written,
2026-08-23T09:54Z, golden copy 2026-08-23T00:00Z) — the LEI record (legal name
`WPP PLC`, jurisdiction `JE`, entity category `GENERAL`, entity **ACTIVE**,
registration **ISSUED** since 2014-10-18 with the next renewal due 2027-09-28, last
updated 2026-08-11, `FULLY_CORROBORATED`, conformity flag `CONFORMING`, no BIC, S&P
Global id `312546`, no OpenCorporates id; entity status and registration status are
different fields and are recorded separately), its **1 ISIN** as a count read from
`meta.pagination.total` of the cited page plus one `:security` entity for the identifier,
its managing LOU and LEI-issuer accreditation (London Stock Exchange LEI Limited, LEI
`213800WAVVOPS85N2205`, accredited 2017-11-06), registration authority `RA000414`
(Jersey Financial Services Commission Companies Registry), ISO 20275 legal form `JOX1`
(`Public Limited Company`, `JE`), reporting exceptions at both consolidation levels
(`NON_CONSOLIDATING` — GLEIF's reason code for an entity that does not consolidate
under any parent at that level, so the registry names no parent at either level; this
file records that answer and nothing about who owns this entity is asserted here), and
a measured **24 direct children**, read from `meta.pagination.total` of the cited page
(two pages of 15, both cited per child), each mirrored as a `:direct-child`
entity: four Thai companies (`TH`, names recorded in Thai script as the registry holds
them), eight Indian companies (`IN`: Pennywise Solutions, Hindustan Thompson
Advertising, Grey Worldwide (India), Bates India, Brand David Communications, WPP
Marketing Communications India, Hungama Digital Services, WPP Media India), Russell
Square Holding B.V. (`NL`), Wunderman Y&R and Burson Cohn & Wolfe (`BE`), WPP
Singapore Pte Ltd (`SG`), WPP Finance Deutschland GmbH (`DE`), WPP Finance SA (`FR`),
and six `GB` companies (WPP Jubilee Limited, WPP CP Finance plc, WPP Finance 2013, WPP
Finance 2017, WPP 2005 Limited, WPP Finance Co. Limited), all `ACTIVE`, all
`IS_DIRECTLY_CONSOLIDATED_BY` this entity. That is the registry's list of entities that
report this LEI as their direct accounting-consolidation parent; it is not a group
chart, and a subsidiary that holds no LEI or reports an exception does not appear in
it. The `direct-parent` and `ultimate-parent` endpoints answered `404` because GLEIF
publishes the exception side of that pair for this entity, which the checker treats as
a fact rather than a failure.

The checker's exit codes are three, not two: `0` every recorded fact matches the live
sources, `1` a citation broke or a fact drifted, `3` the check could not be performed at
all — an absent `facts.edn`, or every request failing at the transport level. A check
that could not run must not be indistinguishable from a check that ran and found
nothing, so it refuses to report a pass rather than exiting 0. All outcomes were
exercised before this landed: unmodified `0` (`OK all 34 recorded fact(s) still match`);
`:company/jurisdiction` rewritten back to `GB` → `1` naming
`DRIFT gleif-lei-record :company/jurisdiction`; `:securities/isin-count` edited `1` →
`2` → `1` naming `DRIFT gleif-isins :securities/isin-count`; the `:security` entity's
ISIN rewritten → `1` naming `DRIFT gleif-isin-je00b8kf9b49 :securities/isin`; the
measured `:relationship/direct-child-count` rewritten `24` → `23` → `1` naming
`DRIFT gleif-direct-children-count`; the WPP Singapore `:direct-child` entity deleted →
`1` naming it `ADDED` (the live registry still lists it); both levels'
`:relationship/exception-reason` rewritten to `NO_KNOWN_PERSON` → `1` naming the drift
in both `gleif-direct-parent-reporting-exception` and
`gleif-ultimate-parent-reporting-exception`; file number `111714` rewritten `111715` →
`1` naming the drift in `gleif-lei-record` and `gleif-registration-authority`;
`:elf/local-name` rewritten → `1` naming `DRIFT iso-20275-entity-legal-form
:elf/local-name`; `blueprint.edn`'s `:company/lei` edited → `1` (`facts.edn records a
different :company/lei than blueprint.edn`); the GLEIF host in the checker rewritten to
an unresolvable name → `3` (`INCONCLUSIVE could not reach GLEIF at all … refusing to
report a pass`); and with no `facts.edn` at all → `3` (`INCONCLUSIVE facts.edn is
missing or holds no facts`). Each mutation was reverted and the restored file compared
byte-for-byte against the generated one.

`facts.edn` is not yet on the shared query plane: `manifest/edn-query.cljs` in
`com-junkawasaki/root` has loaders for `blueprint.edn` and the ToS journal and none for
this file, so its datoms load here but are not joinable from `edn-query`. The
superproject's `manifest/repo-taxonomy.edn` row for this LEI is derived from
`blueprint.edn` and still says `GB` until it is regenerated.

## Design rationale

See ADR-2607110300 in `com-junkawasaki/root` (`90-docs/adr/`) for why this repo exists,
why it is keyed by LEI rather than GTIN or ticker, and why full-text archival (with
provenance) was chosen over excerpt-only storage.
