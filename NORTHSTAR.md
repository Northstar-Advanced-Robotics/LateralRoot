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

Each group is its own commit, so `git log develop..northstar-dev` lists them.

**Team edits**

| Commit | Files |
|---|---|
| DT7 / VT13 / FlySky remote selection | `communication/serial/{module.lb, remote.cpp, remote.hpp, remote_serial_constants.hpp.in}` |
| Middle mouse button and `channelLessThan` trigger | `control/{remote_map_state.cpp, remote_map_state.hpp, trigger_helpers.hpp}` |
| `Drivers::DT` and generated drivers.hpp layout | `drivers/{drivers.hpp.in, drivers.py}` |
| DJI motor encoder debug value | `motor/{dji_motor.hpp.in, dji_motor_encoder.cpp}` |
| Remove files the 2026 tree does not have | `algorithms/transforms/{axis.hpp, intrinsic_euler_extractor.hpp, vector.cpp}`, `control/finite_repeat_command.hpp` |
| Imported from old LateralRoot `main` | `algorithms/{cmsis_mat.hpp, smooth_pid.cpp}`, `algorithms/transforms/{angular_velocity.hpp, transform.cpp}` |

**Held at an older upstream version** (on purpose: team code depends on these versions)

| Group | Files | Matches upstream |
|---|---|---|
| Transforms API | `transforms/{dynamic_orientation.hpp, dynamic_position.hpp, orientation.hpp, position.cpp, position.hpp, transform.hpp, vector.hpp}` | `486a015` / `8fbd0ac`, `vector.hpp` `b9ce1d7` |
| IMU | `communication/sensors/imu/abstract_imu.{cpp,hpp}` | `ffda4c2` (`c839023`) |
| Math utils / ballistics | `algorithms/{math_user_utils.cpp, math_user_utils.hpp, ballistics.hpp}` | `f0e079c` / `791ff77` |
| Wrapped encoder / trigger binding | `sensors/encoder/wrapped_encoder.hpp`, `control/trigger_binding.cpp` | `24e5988` / `34c4084` |

Because of the transforms and math holds, upstream's slerp fix (`bb03295`, `Orientation::interpolate` / `Transform::interpolate`) is not in this branch.

Ref serial is **not** held here. `northstar-2026` keeps upstream `a0641bd` (Ref Serial 1.3) only because the v1.3.1 update was not out in time for
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
