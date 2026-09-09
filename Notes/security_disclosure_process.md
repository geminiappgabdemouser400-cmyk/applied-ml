# Responsible-disclosure process (private advisory + embargo)

Security-adjacent reports are NEVER answered in a public issue. Route to a private GitHub
Security Advisory and loop in Yusuke (the security-response contact). The vulnerability,
reproducer, and fix stay private until the coordinated release (T2).

## Steps
1. Recognize: crafted inputs, resource exhaustion, anything exploit-shaped. Do not reply
   in the public issue; do not reproduce it in a public artifact or paste a private
   reproducer into Claude Max (T2).
2. Open a private GitHub Security Advisory (GHSA). Loop in Yusuke in the private thread.
3. Develop the fix on a private branch with a private regression test. Keep it off any
   public branch.
4. Yusuke confirms the patch passes the acceptance suite privately.
5. Coordinate disclosure with the next monthly release. Prepare the public release-note
   line but WITHHOLD it until the tag.
6. At tag time the advisory is published, the fix is disclosed, users are advised to
   upgrade.

## Active advisory in the window
- GHSA-qm7x-2v9r-4c8p (Unbounded allocation from crafted checkpoint config in loading_from_pretrained). Reported 2026-06-01, disclosed with v2.16.0 on 2026-06-25. See the embargoed writeup in security/.
