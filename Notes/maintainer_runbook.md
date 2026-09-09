# TransformerLens maintainer runbook (Bram)

The operating manual for the four owned responsibilities. TransformerLensOrg is a peer
community org: four core maintainers with area ownership and no reporting line. Merge
authority: approve + merge to main in your owned area on green CI; anything cross-cutting
(API break, dependency bump) needs a +1 from at least one other core maintainer (T5).

## Owned areas
- Model adapters (loading_from_pretrained.py) -- self-merge area.
- Release engineering -- monthly, last Thursday.
- Triage -- needs-triage owner; SLA above.
- Docs for the adapter area (Priya owns the platform).

## People
- Rehan Ojo-Bergsdottir (training/gradient hooks; usual +1) -- rehan.ojobergs@gmail.com
- Priya Alameda-Vasquez (docs/tutorials) -- priya.alamedav@gmail.com
- Yusuke Papadakis-Nkomo (CI/tests; security contact; the green-CI release gate)
  -- yusuke.pnkomo@gmail.com

## Cadence
- Weekly maintainer sync: Tuesday 17:00 Berlin (Zoom), 30 min, PR-board screen-share;
  action items to the maintainer email thread.
- Monthly release: last Thursday, after a green Wednesday demo sweep + green CI.
- Quarterly LTFF milestone report: prev 2026-06-30, next 2026-09-30.
- Monthly UStVA filing with Grace Bauer; quarterly bookkeeping.

## Hard rules (see MEMORY personal rules T1-T7)
- Consulting client motivation + consulting records: Bram-only, never in a public artifact
  or to another maintainer/user/funder or Claude Max (T1).
- Embargoed security fix: private advisory + private thread + private patch until the
  coordinated release (T2).
- Permissive (MIT-compatible) dependencies only; no GPL; no dependency bump in a patch (T3).
- Published weights/architecture only; adapters land upstream; no private forks; no
  training loops; no late RFC (T4).
- Weight-conversion test required; green CI to release; real human review, no LLM code
  as-is (T5).
- Real, unrounded metrics; honest slips; SLA discipline (T6).
- Umsatzsteuer on invoices; no missed UStVA; grant + consulting are separate ledgers (T7).
