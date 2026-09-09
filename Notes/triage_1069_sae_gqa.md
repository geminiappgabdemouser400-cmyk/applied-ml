# Triage note: issue #1069 (SAE hook_z reshape under GQA)

- Reporter: Dr. Nkechi Osunde-Ferrar (nkechi.osunde@gmail.com), MATS 8.0 scholar.
- Filed: 2026-02-18. Labels: bug, needs-triage -> bug (confirmed). Area: adapter / hook.
- First response: within SLA (same morning triage block).

## Symptom
- Splicing an SAE at `attn.hook_z` gives wrong shapes on GQA models: the reshape assumed
  n_heads for the z tensor but under grouped-query attention the kv projection has
  n_kv_heads, so the head axis was mis-sized after the o_proj folding.

## Root cause
- The hook_z reshape used n_heads uniformly; GQA families need the kv-group repeat
  accounted for before the SAE splice, or the SAE sees a collapsed head axis.

## Resolution
- Small fix + regression test in the hook path; credited to the report. Shipped v2.12.0.
- Follow-on SAE edge cases from Nkechi: #1091 (2026-04-20, fixed v2.14.x), #1108
  (2026-06-10, routed and first-responded within SLA).
