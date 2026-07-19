# face-match

Face-match comparison contract for kotoba-lang eKYC/AML. **This repo has no
embedding model and no comparison algorithm.** It exists to define an
honest boundary: `face-match.core/match` always routes to human review
(`:review`/`:not-implemented`) unless a real
`face-match.ports/IFaceMatcher` is injected — and none exists anywhere in
this org today.

## Why ship a repo that does nothing?

Comparing a selfie against a document photo needs a real face-embedding
model with a real, measured false-accept/false-reject rate. Building or
porting one without real benchmark data would produce a component that
*looks* like it verifies identity while actually offering no real
assurance — worse than not having the check at all, because it creates
false confidence. See `MATURITY.md` for the full reasoning and the
conditions under which this would be revisited.

This repo is the "seam", matching `mrz.ports/IZoneOcr` and
`face-liveness.ports/ILandmarkTracker`'s own host-injection shape, so a
real implementation has somewhere to plug in later without changing any
caller's contract.

## Usage

```clojure
(require '[face-match.core :as core])

(core/match selfie-bytes document-photo-bytes)
;;=> {:face-match/status :review
;;    :face-match/confidence nil
;;    :face-match/reason :not-implemented
;;    :face-match/non-adjudicating true}
```

See `90-docs/adr/*-kotoba-lang-face-match.edn` (in the
`com-junkawasaki/root` superproject) for the design rationale.
