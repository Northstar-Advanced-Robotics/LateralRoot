# Northstar: `northstar-2026` (frozen)

This branch is the Taproot that Northstar's robots run in the 2026 season:
upstream Taproot `4cacf127` plus the Northstar edits (commits `5eeafba` and `42d0851`).

- **Frozen. Bug fixes only**, through a pull request.
- New work and upstream Taproot updates go to `northstar-dev`. See `NORTHSTAR.md` on that branch for the branch model and update procedure.
- Release tag: `northstar-2026-v1`.
- Checked on 2026-10-01: regenerating the controls project's Type C tree from this branch reproduces the committed
  `northstar-robomaster` tree at `f23bccd`, apart from cosmetic differences (line endings, banner position, trailing newline) and one stray
  `#include ""` in the unit-test branch of `drivers.hpp` that the old tree had and this branch no longer generates.
