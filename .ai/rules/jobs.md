---
paths:
  - 'app/Jobs/**'
---

# Jobs

## Run heavy analysis as a resilient queued pipeline
Do not perform heavy analysis in one user HTTP request. Support a queued FetchListing → ParseListing → NormalizeProperty → ResolveLocation → FetchExternalEvidence → CalculateEstimates → CalculateScores → FindAlternatives → GenerateReport pipeline. Jobs must be retry-safe, idempotent where practical, observable, and tolerant of partial failure.
