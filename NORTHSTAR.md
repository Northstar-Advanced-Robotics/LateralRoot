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

`git diff develop northstar-dev -- src` shows exactly these (11 files). Everything else is upstream's current code.

| Commit | Files |
|---|---|
| DT7 / VT13 / FlySky remote selection | `communication/serial/{module.lb, remote.cpp, remote.hpp, remote_serial_constants.hpp.in}` |
| Middle mouse button and `channelLessThan` trigger | `control/{remote_map_state.cpp, remote_map_state.hpp, trigger_helpers.hpp}` |
| `Drivers::DT` and generated drivers.hpp layout | `drivers/{drivers.hpp.in, drivers.py}` |
| DJI motor encoder debug value | `motor/{dji_motor.hpp.in, dji_motor_encoder.cpp}` |

## Differences from `northstar-2026`

`northstar-2026` held 17 files at older upstream versions and used 4 older files from the old LateralRoot `main`. This branch takes upstream's
current versions of all of them. The commits that added those holds and the commits that revert them are both in this branch's history.

| Area | What changes for team code |
|---|---|
| Ref serial | Upstream v1.3.1 (`b8bc808`). 2026 was held at `a0641bd` only because v1.3.1 was not out before competition. |
| Math / transforms / `cmsis_mat` / `smooth_pid` / ballistics | Adds the slerp fix (`bb03295`) and new helpers. The IMU mounting-transform math (the only transform team code uses) is unchanged. |
| IMU (`abstract_imu`) | Upstream `0086ebe` adds sample averaging that nothing fills yet, so readings are unchanged (+256 B RAM). |
| Wrapped encoder / trigger binding | `922d592` access change; `b703b88` fixes `whileFalse` bindings never being removed. Team code uses neither. |

All robots, the simulator and the unit tests build against this branch. A robot test is still pending before robots move to it.

## Updating from upstream Taproot

1. On GitHub, click **Sync fork** on `develop`.
2. Open a PR `develop` → `northstar-dev` and resolve conflicts (the team edit files above are the usual spots).
   **Keep our version of every Northstar change** unless the team decides otherwise in that PR.
3. In the controls repo, point the `taproot` submodule at the PR branch, regenerate (`cd northstar-robomaster-project && pipenv run lbuild build`), build all robots, run the
   tests, and test on a robot.
4. Merge. At the start of a season, cut `northstar-<year>` from `northstar-dev` and freeze it like `northstar-2026`.

**Never hand-edit the generated `taproot/` in the controls repo.** Change LateralRoot and regenerate.
