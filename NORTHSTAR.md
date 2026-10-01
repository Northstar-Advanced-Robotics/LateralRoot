# Northstar: LateralRoot

LateralRoot is Northstar Advanced Robotics' fork of [Taproot](https://github.com/uw-advanced-robotics/taproot).
The controls repo (`northstar-robomaster`) uses it as its `taproot` submodule and generates its `taproot/` tree from it.

## Branches

| Branch | What it is |
|---|---|
| `develop` | Untouched mirror of upstream Taproot `develop`. Updated only with GitHub's **Sync fork** button. |
| `northstar-2026` | **Frozen.** What the robots ran in 2026: upstream `4cacf127` + our edits. Bug fixes only, via PR. Tag `northstar-2026-v1`. |
| `northstar-dev` | **This branch.** Current upstream + our edits as separate commits. Upstream updates are merged in here. |
| `northstar-<year>` | Cut from `northstar-dev` when a season's code is final, then frozen like `northstar-2026`. |
| `legacy/*` | Old LateralRoot branches (Spark Max/REV, custom CAN RX, …), kept for reference. |

## Our changes on top of upstream

`git diff develop northstar-dev -- src` shows exactly these (15 files).

**Team edits**

| Commit | Files |
|---|---|
| DT7 / VT13 / FlySky remote selection | `communication/serial/{module.lb, remote.cpp, remote.hpp, remote_serial_constants.hpp.in}` |
| Middle mouse button and `channelLessThan` trigger | `control/{remote_map_state.cpp, remote_map_state.hpp, trigger_helpers.hpp}` |
| `Drivers::DT` and generated drivers.hpp layout | `drivers/{drivers.hpp.in, drivers.py}` |
| DJI motor encoder debug value | `motor/{dji_motor.hpp.in, dji_motor_encoder.cpp}` |

**Held at an older upstream version** (what the robots ran in 2026; not yet tested against upstream's current version)

| Group | Files | Matches upstream |
|---|---|---|
| IMU | `communication/sensors/imu/abstract_imu.{cpp,hpp}` | `ffda4c2` (`c839023`) |
| Wrapped encoder / trigger binding | `sensors/encoder/wrapped_encoder.hpp`, `control/trigger_binding.cpp` | `24e5988` / `34c4084` |

**Taken from upstream here, held on `northstar-2026`:**

- Math, transforms, `cmsis_mat`, `smooth_pid`, ballistics: all of `src/tap/algorithms` is upstream's current version (includes the slerp fix
  `bb03295`). `northstar-2026` held older versions plus files from the old LateralRoot `main`; the commits that added those holds and the
  commits that revert them are both in this branch's history, so the old state is easy to find. All robots, the simulator and the tests build
  against it. The IMU mounting-transform math (the only transform team code uses) gives the same result as before. Robot test still pending.
- Ref serial: `northstar-2026` keeps upstream `a0641bd` (Ref Serial 1.3) only because the v1.3.1 update was not out in time for
  the 2026 competition. This branch uses upstream's current ref serial.

Git treats every held-back file as a deliberate revert. When upstream changes one of them, a merge either keeps our old version silently
(if the lines don't overlap) or conflicts. Either way, check these files in every update PR.

## Updating from upstream Taproot

1. On GitHub, click **Sync fork** on `develop`.
2. Open a PR `develop` → `northstar-dev` and resolve conflicts (the held-back files and team edits are the usual spots).
   **Keep our version of every Northstar change** unless the team decides otherwise in that PR.
3. In the controls repo, point the `taproot` submodule at the PR branch, run `scripts/regenerate_taproot.sh`, build all robots, run the
   tests, and test on a robot.
4. Merge. At the start of a season, cut `northstar-<year>` from `northstar-dev` and freeze it like `northstar-2026`.

**Never hand-edit the generated `taproot/` in the controls repo.** Change LateralRoot and regenerate.
