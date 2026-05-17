# custos-perception — CLAUDE.md

Vision pipelines, sensor fusion, and the **public mock node** that satisfies the same `custos-interfaces` contract as the private NOVATEK wrapper.

Workspace-wide rules and state caveats live at `../CLAUDE.md` (= `/space/drone/CLAUDE.md`). If you are in a standalone clone, the one rule below is the one most likely to be violated when working in this repo.

## The one rule

**This repo never depends on `custos-novatek-sdk-wrapper`.** Not as `<depend>`, not as `#include`, not as a `find_package` call. The wrapper's outputs reach you only via the ROS2 graph, expressed as `custos-interfaces` messages. If you find yourself wanting to "just grab a frame directly from the wrapper," that is the failure mode ADR 0010 was written to prevent — extend `custos_perception_msgs` instead.

## State of this repo

- Single package today: `custos_perception_mock` (`package.xml`, `CMakeLists.txt`, empty `src/`). No real implementation yet.
- The mock will eventually be joined by real vision-pipeline packages here (sensor fusion, detection, tracking).
- **Brace-expansion litter:** there is a literal directory named `{.github` alongside the real `.github/`. Do not stage it.

## Cross-repo edges

- **Depends on:** `rclcpp`, `rclcpp_lifecycle`, `lifecycle_msgs`, `diagnostic_msgs`, `sensor_msgs`, `custos_common_msgs`, `custos_perception_msgs`. Identical to `custos_novatek_wrapper` by design.
- **Contract-paired with:** `custos-novatek-sdk-wrapper/custos_novatek_wrapper`. The mock and the wrapper present the same external surface; if one changes, the other follows in the same merge cycle.
- **Depended on by (runtime, by topic):** `custos-navigation/custos_navigation_planner` consumes `custos_perception_msgs`. `custos-simulation` runs the mock to drive sim scenarios.

## Repo-specific hard rules

- **Mock parity is the public CI's source of truth.** Every external behavior of `custos_novatek_wrapper` must be expressible by this mock, because public CI cannot run the wrapper. When the wrapper's surface changes, the mock changes in the same PR cycle. A mock that lags reality lets public CI green-light a broken contract.
- **The mock is not a stub.** It must publish on the same topics, with the same QoS, the same lifecycle transitions, and the same diagnostics output as the real wrapper. Test code that pretends the mock is fully featured is fine; test code that assumes the mock is a no-op is a trap.
- **Future vision-pipeline packages do not import the mock.** The mock is a node, not a library. Use `custos_perception_msgs` types in your downstream.

## Build / test cheat sheet

```bash
colcon build --packages-select custos_perception_mock
colcon test --packages-select custos_perception_mock

# Run the mock against the same topic set the wrapper would expose.
ros2 launch custos_perception_mock mock.launch.py   # TODO: launch file unwritten
```

## Pointers specific to this repo

- Isolation contract: ADR 0010
- Wrapper counterpart: `custos-novatek-sdk-wrapper/custos_novatek_wrapper/`
- Wire types: `custos-interfaces/custos_perception_msgs/`

> TODO(post-first-commit): delete the `{.github` litter directory before the initial commit.
> TODO(post-active): write the mock implementation and the launch file.
> TODO(post-active): list real vision-pipeline packages here once they exist; expect detection, tracking, sensor-fusion sub-packages.
