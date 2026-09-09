# Triage playbook (Bram owns the needs-triage label)

SLAs (T6): first response within 72 hours on new PRs; within 5 business days on new
issues. Primary rotation: Bram Mon-Wed, Rehan Thu-Fri, Priya + Yusuke weekends. A real
response, never a hollow label.

## Labels
- bug, enhancement, docs, question, needs-repro, wontfix, duplicate, needs-triage,
  security (do NOT use in public; route to advisory).

## Flow (morning triage block, 30-45 min)
1. Sweep new issues + PRs.
2. Reproducible bug -> `bug`, confirm repro (Claude Max on PUBLIC inputs only), route to
   area owner.
3. No reproducer -> `needs-repro` + template response.
4. Duplicate -> close with a pointer to the canonical issue.
5. Enhancement / model request -> `enhancement`; if published weights + architecture,
   accept as an adapter candidate; else decline (T4).
6. Security-adjacent (a crafted input, a resource-exhaustion, anything exploit-shaped) ->
   do NOT answer in the public issue. Open a private GitHub Security Advisory and loop in
   Yusuke (T2). See security_disclosure_process.md.

## Pressure valves
- PR review queue > 15 awaiting past 72h -> drop that week's consulting hours (T6),
  route clients to next week.
- Untriaged issues > 40 -> call a Saturday triage-jam (all four maintainers).

## Freeze
- No new feature-scope RFC in the last two weeks before a release (T4).
