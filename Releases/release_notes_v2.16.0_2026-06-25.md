# TransformerLens v2.16.0

Release date: 2026-06-25 (last-Thursday cadence). Package: transformer-lens on PyPI.
Tag: v2.16.0 on main. Cut after a green Wednesday demo-notebook sweep and green CI (Yusuke gate).

## Highlights

- adapter: Gemma-3 family (#1112)
- security: bound-check checkpoint config fields before allocation (GHSA-qm7x-2v9r-4c8p)
- ActivationCache signature change landed in a minor, not a patch (+1 rehan-ob)

## Merged this cycle
- 24 pull requests merged to main this cycle.
- Full merged-PR log: see Releases/pr_merge_log.csv.

## Credits

- security advisory GHSA-qm7x-2v9r-4c8p disclosed at release

## Install / upgrade
```
pip install --upgrade transformer-lens==2.16.0
```

Monitored PyPI and the community Discord for 48 hours after publish.
