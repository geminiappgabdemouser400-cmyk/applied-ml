# Dependency license policy (permissive-only) and the #1101 case

TransformerLens is MIT licensed. Every dependency must be MIT-compatible (permissive:
MIT / BSD / Apache-2.0). A GPL/copyleft dependency would virally re-license the project
and is not allowed. Every dependency-adding PR gets a license check (T3).

## The #1101 case (2026-05-11 .. 2026-05-15)
- External PR #1101 added a convenient GPL-licensed helper to solve a tokenizer edge case.
- 2026-05-12: flagged the GPL license in pyproject.toml against the MIT LICENSE and the
  permissive-only policy. Declined the GPL dependency; asked the contributor for a
  permissive (MIT/BSD/Apache) alternative or to vendor a small helper.
- 2026-05-14: PR revised to a permissive alternative; license check passed; merged green.

## The separate patch-line request (2026-05-15)
- A request to bump a dependency version inside the pending v2.15.x patch line was
  declined: no dependency bump in a patch release (T3). Deferred to the next minor.

## Check procedure
- New/changed dependency -> confirm SPDX license is in the permissive allowlist; the CI
  license check (added around v2.15.0) enforces this on every PR.
