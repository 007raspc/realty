---
paths:
  - 'tests/**'
---

# Tests

## Store evidence with every significant property fact
A significant fact must carry value, evidence status, source, source URL/identifier, retrieved or verified time when applicable, and confidence for estimates. Use VERIFIED, CLAIMED, OBSERVED, ESTIMATED, UNKNOWN, and NOT_APPLICABLE with their literal meanings; show UNKNOWN instead of inventing a value. Repeated copies of the same listing claim never promote CLAIMED to VERIFIED.

## Keep source layers separate and treat AI as non-authoritative
Keep listing, official government, municipal, map/geospatial, AI observation, user-provided, and inspector evidence separate. Regional infrastructure never proves connection at a specific property. LLMs may extract, classify, suggest normalization, summarize, explain, generate questions, flag inconsistencies, and emit schema-validated structured output, but must not silently overwrite verified data; verified sources win and important AI conclusions must remain auditable.

## Require official evidence for cadastre and legal claims
Never claim legal cleanliness, cadastre verification, or absence of encumbrances without current official data; listing field `Uknjiženo` is CLAIMED. Keep cadastral municipality, parcel and property/building identifiers, registered versus advertised property and land areas, ownership/shares, encumbrances, mortgages, annotations, legalization/construction status, permits, and official verification date separately when available. Surface advertised/registered area mismatches as an explicit risk, not buried prose.

## Track utilities independently with property-level evidence
Track electricity, public water, private well, public sewer, septic, gas, heating, fiber, broadband, mobile coverage, road/asphalt access, drainage, and waste collection independently, each with an evidence status. Do not infer a specific property's connection from neighborhood infrastructure.

## Use PostGIS location analysis without collapsing distance types
Use PostgreSQL + PostGIS for spatial logic and support city-center, transit, school, healthcare, shop, major-road, rail, park, noise-source, industrial-area, and relevant-POI analysis. Keep straight-line, walking/driving, and listing-provided distances distinct; store coordinates canonically in WGS84.

## Keep region intelligence distinct from property facts
Support practical regional context: climate and seasons, precipitation, heat/winter conditions, terrain, water, flood/landslide context, air quality, seismic context, infrastructure, transport, population trends, tourism seasonality, construction characteristics, and maintenance issues where relevant and sourced. Never automatically promote region-level information to a property-level fact.

## Calculate deterministic versioned Property Scores
Never let an LLM generate the numeric Property Score. Calculate it programmatically from explicit factors and weights so it is reproducible, explainable, versioned, and recalculable; persist the scoring-model version. Candidate factors include price, legal/document confidence, condition, renovation exposure, location, infrastructure, utilities, environmental risks, liquidity, and total acquisition cost; do not finalize weights without explicit approval.

## Keep sponsorship out of evidence, scores, and risks
Sponsorship may affect only visibility or promotion. It must never raise a score, hide negative findings, change risk classification, or grant VERIFIED or INSPECTED status.

## Model money and total acquisition cost explicitly
Never use binary floating point for money; store currency with every value and distinguish source from display currency. Asking price is not total acquisition cost: model purchase price, applicable transfer tax/VAT, agency, notary, legal/document, cadastral/geodetic, inspection, immediate renovation, utility connection, furnishing, financing, and contingency items as relevant. Every item carries amount/range, calculation method, status, and assumptions; renovation is a range, never false precision.

## Store canonical measurement units
Store normalized canonical units suitable for calculation: property and land area in square meters, distances in meters, and geographic coordinates in WGS84. Presentation may convert units, but storage must remain unambiguous.

## Distinguish visual renovation signals from verified defects
Do not assert a structural defect from AI photo analysis. Distinguish VISUAL_INDICATION, INSPECTOR_FINDING, and VERIFIED_DEFECT. Preliminary renovation estimates must consider property type, area, visible condition, construction status, known age, local labor/material assumptions, missing utilities, and finishing level; persist assumptions separately from the result.

## Gate media display through explicit rights
Every media item must have a rights mode: SOURCE_ONLY, DISPLAY_LICENSE, OWN_MEDIA, or REVIEW_EVIDENCE only under an approved legal policy. Store rights holder, provider, license scope, attribution, derivative permission, validity period, consent record, and original source. Never copy or publicly render third-party listing images unless the media-rights layer authorizes it.

## Preserve source listings and historical values
Always preserve original listing URL, source platform, source listing identifier when available, first-seen date, and last-checked date. Retain recoverable history for asking price, status, description, advertised area, creation/removal, and important attribute changes; normalization must not erase provenance or overwrite history irreversibly.

## Isolate listing parsers behind source adapters
Give each listing source its own ListingSourceAdapter implementation; keep selectors and source-specific parsing out of controllers and shared domain logic. Normalized property data must not depend on portal HTML. On source markup changes, fail clearly, record diagnostics, preserve prior/verified data instead of replacing it with null, and retain a lawful raw snapshot or sufficient extraction context.

## Run heavy analysis as a resilient queued pipeline
Do not perform heavy analysis in one user HTTP request. Support a queued FetchListing → ParseListing → NormalizeProperty → ResolveLocation → FetchExternalEvidence → CalculateEstimates → CalculateScores → FindAlternatives → GenerateReport pipeline. Jobs must be retry-safe, idempotent where practical, observable, and tolerant of partial failure.

## Version reports and expose their provenance
Each report must retain report version, generation time, data freshness, scoring version, sources used, and unresolved checks. Material source-data changes must not silently rewrite an old report.

## Show at most two genuinely better alternatives
A report may show at most two alternatives, and only when an alternative has a higher Property Score or a clear documented reason it is better; never pad the block. For each, explain score and price differences, main advantages/disadvantages, and why it may be the better choice.

## Make uncertainty and provenance visible in the UX
Users must be able to distinguish what is known, who claims it, what was calculated, what is assumed, and what still needs verification. Do not visually present CLAIMED as equivalent to VERIFIED, and never fill an unsupported card with a plausible-looking value.

## Minimize personal data in the MVP
Collect only data necessary for public-listing analysis: listing URL, optional goal, optional budget, and account/contact data when required. Do not request passports, bank documents, contracts, or sensitive transaction documents without a specific approved future need.

## Test critical domain rules below the controller layer
Automate tests for scoring, monetary calculations, evidence-status transitions, alternative selection, listing normalization, property/listing matching, total acquisition cost, and rights-based media rendering. Controller tests alone are insufficient for critical domain logic.

## Do not silently invent material business rules
Do not silently choose scoring weights, liquidity meanings, better-alternative thresholds, renovation coefficients, confidence thresholds, or other decisions that change product meaning. Surface the missing decision and propose an option before fixing it in code. Record only newly settled durable architecture/domain decisions through Laravel Boost `record-rule`, never temporary TODOs.

## Cover the complete pre-purchase decision in every report
Design reports to answer price attractiveness, total extra costs, legal/cadastre status, claimed versus confirmed utilities, condition and renovation exposure, building/parcel/neighborhood characteristics, climate/seasonal/natural risks, liquidity, seller questions, and checks required before a deposit, plus no more than two qualifying alternatives. Keep this as independent decision support, not a claim of legal due diligence or engineering inspection.
