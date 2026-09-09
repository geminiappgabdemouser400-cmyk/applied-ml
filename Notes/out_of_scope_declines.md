# Out-of-scope request log (declined per policy, T4/T1)

A running log of requests declined because they fall outside project scope. Each decline
references the same scope policy and, where useful, points the asker somewhere better.

| Date | Request | Decision | Pointer |
|---|---|---|---|
| 2026-01-19 | Support a closed-weights internal model | Declined: published weights + published architecture only | Offered to review a PR for a public model |
| 2026-02-10 | RLHF training-loop support | Declined: out of scope (analysis-first, not a training framework) | Pointed to TRL |
| 2026-05-19 | Private fork with a client-only modification | Declined: any adapter lands upstream; no private forks (T1/T4) | Pointed to internal-only teams |
| 2026-05-25 | New feature-scope RFC, 3 days before the 05-28 release | Deferred past the release | No new feature-scope RFC in the last two weeks before a release |

## Standing scope reminders
- No closed-weights or private adapters; no private client forks.
- No training-loop / RLHF features (point to TRL).
- No new feature-scope RFC inside the two-week pre-release freeze.
- Recruiter cold emails: declined.
