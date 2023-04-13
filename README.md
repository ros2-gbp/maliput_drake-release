[![gcc](https://github.com/maliput/maliput_drake/actions/workflows/build.yml/badge.svg)](https://github.com/maliput/maliput_drake/actions/workflows/build.yml)

# maliput_drake

## Description

A clone of [`drake`](https://github.com/RobotLocomotion/drake)'s trajectory integration support for `maliput` backends.

**Note**: For full information about Maliput please visit [Maliput Documentation](https://maliput.readthedocs.io/en/latest/index.html).

## Installation

### Supported platforms

Ubuntu Focal Fossa 20.04 LTS.

### Binary Installation on Ubuntu

See [Installation Docs](https://maliput.readthedocs.io/en/latest/installation.html#binary-installation-on-ubuntu).

### Source Installation on Ubuntu

#### Prerequisites

```
sudo apt install python3-rosdep python3-colcon-common-extensions
```

#### Build

1. Create colcon workspace if you don't have one yet.
    ```sh
    mkdir colcon_ws/src -p
    ```

2. Clone this repository in the `src` folder
    ```sh
    cd colcon_ws/src
    git clone https://github.com/maliput/maliput_drake.git
    ```

3. Install package dependencies via `rosdep`
    ```
    export ROS_DISTRO=foxy
    ```
    ```sh
    rosdep update
    rosdep install -i -y --rosdistro $ROS_DISTRO --from-paths src
    ```

4. Build the package
    ```sh
    colcon build --packages-up-to maliput_drake
    ```

For further info refer to [Source Installation on Ubuntu](https://maliput.readthedocs.io/en/latest/installation.html#source-installation-on-ubuntu)

## How to copy `drake` into `maliput_drake`?

### Copying files

Follow this tentative guide (it may change release to release) to copy files
from a [`drake`](https://github.com/RobotLocomotion/drake) release to this
package:

1. Clone [`drake`](https://github.com/RobotLocomotion/drake) and point it to a
   specific release tag, e.g. [`v0.33.0`](https://github.com/RobotLocomotion/drake/releases/tag/v0.33.0):

```sh
git clone --depth 1 --branch v0.33.0 git@github.com:RobotLocomotion/drake.git $DRAKE_WS/drake
```

Please note the use of `DRAKE_WS` which is the path to [`drake`](https://github.com/RobotLocomotion/drake)'s
workspace. You might want to create a docker container to run and configure
[`drake`](https://github.com/RobotLocomotion/drake) on it but not necessarily
place it inside maliput workspace to avoid any dependency collision.

2. Follow the instructions for the release to prepare the environment and
   [install `drake` dependencies](https://drake.mit.edu/from_source.html).

3. Derive which dependencies are necessary up to `antiderivative_function` and
   `scalar_initial_value_problem`:

```sh
cd $DRAKE_WS/drake
bazel query 'deps(//systems/analysis:antiderivative_function)' | sed -e 's#:.*##g;' | sort | uniq
bazel query 'deps(//systems/analysis:scalar_initial_value_problem)' | sed -e 's#:.*##g;' | sort | uniq
```


4. Create a new branch in this repo (`maliput_drake`) to update the files.

```sh
cd src/maliput_drake
git checkout -b username/bump_to_drake_v0.33.0
```

5. Remove the previous contents of the folders in `maliput_drake/src` and
   `maliput_drake/include`:

```sh
find maliput_drake/src/ -name "*.cc" -type f -delete
find maliput_drake/include/ -name "*.h" -type f -delete
```

6. Copy the source files to the respective `maliput_drake/src` folder and the
   header files to the respective `maliput_drake/include/maliput` folder.

7. Make sure you remove unnecessary files:

```sh
maliput_drake/include/maliput/drake/common/drake_path.h
maliput_drake/include/maliput/drake/common/filesystem.h
maliput_drake/include/maliput/drake/common/find_loaded_library.h
maliput_drake/include/maliput/drake/common/find_resource.h
maliput_drake/include/maliput/drake/common/find_runfiles.h
maliput_drake/include/maliput/drake/common/temp_directory.h
maliput_drake/include/maliput/drake/systems/analysis/bogacki_shampine3_integrator.h
maliput_drake/include/maliput/drake/systems/analysis/implicit_euler_integrator.h
maliput_drake/include/maliput/drake/systems/analysis/implicit_integrator.h
maliput_drake/include/maliput/drake/systems/analysis/lyapunov.h
maliput_drake/include/maliput/drake/systems/analysis/monte_carlo.h
maliput_drake/include/maliput/drake/systems/analysis/radau_integrator.h
maliput_drake/include/maliput/drake/systems/analysis/region_of_attraction.h
maliput_drake/include/maliput/drake/systems/analysis/runge_kutta2_integrator.h
maliput_drake/include/maliput/drake/systems/analysis/runge_kutta5_integrator.h
maliput_drake/include/maliput/drake/systems/analysis/semi_explicit_euler_integrator.h
maliput_drake/include/maliput/drake/systems/analysis/simulator.h
maliput_drake/include/maliput/drake/systems/analysis/simulator_config.h
maliput_drake/include/maliput/drake/systems/analysis/simulator_config_functions.h
maliput_drake/include/maliput/drake/systems/analysis/simulator_gflags.h
maliput_drake/include/maliput/drake/systems/analysis/simulator_print_stats.h
maliput_drake/include/maliput/drake/systems/analysis/simulator_status.h
maliput_drake/include/maliput/drake/systems/analysis/velocity_implicit_euler_integrator.h
maliput_drake/src/common/add_text_logging_gflags.cc
maliput_drake/src/common/drake_path.cc
maliput_drake/src/common/filesystem.cc
maliput_drake/src/common/find_loaded_library.cc
maliput_drake/src/common/find_resource.cc
maliput_drake/src/common/find_runfiles.cc
maliput_drake/src/common/find_runfiles_stub.cc
maliput_drake/src/common/resource_tool.cc
maliput_drake/src/common/temp_directory.cc
maliput_drake/src/systems/analysis/bogacki_shampine3_integrator.cc
maliput_drake/src/systems/analysis/explicit_euler_integrator.cc
maliput_drake/src/systems/analysis/implicit_euler_integrator.cc
maliput_drake/src/systems/analysis/implicit_integrator.cc
maliput_drake/src/systems/analysis/lyapunov.cc
maliput_drake/src/systems/analysis/monte_carlo.cc
maliput_drake/src/systems/analysis/radau_integrator.cc
maliput_drake/src/systems/analysis/runge_kutta2_integrator.cc
maliput_drake/src/systems/analysis/runge_kutta5_integrator.cc
maliput_drake/src/systems/analysis/semi_explicit_euler_integrator.cc
maliput_drake/src/systems/analysis/simulator.cc
maliput_drake/src/systems/analysis/simulator_config.cc
maliput_drake/src/systems/analysis/simulator_config_functions.cc
maliput_drake/src/systems/analysis/simulator_gflags.cc
maliput_drake/src/systems/analysis/simulator_status.cc
maliput_drake/src/systems/analysis/velocity_implicit_euler_integrator.cc
maliput_drake/src/systems/analysis/region_of_attraction.cc
maliput_drake/src/systems/analysis/simulator_print_stats.cc
```

8. Removes the following method from `maliput_drake/src/common/drake_assert_and_throw.cc`.
   ```cpp
   extern "C" void drake_set_assertion_failure_to_throw_excepts()
   ```
   To avoid having duplicated references in systems where both `maliput_drake` and `drake` are installed.

9. Update the CMakeLists.txt under `maliput_drake/src/` for the library targets.
   Make sure all source files are properly listed.

10. To avoid having duplicated definitions in systems where both `maliput_drake` and `drake` are installed,
   rename the following definitions from `maliput_drake/src/common/drake_throw.h` and `maliput_drake/src/common/drake_assert.h`.
    ```
    DRAKE_THROW_UNLESS to MALIPUT_DRAKE_THROW_UNLESS
    DRAKE_DEMAND to MALIPUT_DRAKE_DEMAND
    DRAKE_ASSERT_VOID to MALIPUT_DRAKE_ASSERT_VOID
    DRAKE_ASSERT to MALIPUT_DRAKE_ASSERT
    DRAKE_UNREACHABLE to MALIPUT_DRAKE_UNREACHABLE
    DRAKE_DISABLE_ASSERTS to MALIPUT_DRAKE_DISABLE_ASSERTS
    DRAKE_ENABLE_ASSERTS to MALIPUT_DRAKE_ENABLE_ASSERTS
    DRAKE_ASSERT_IS_DISARMED to MALIPUT_DRAKE_ASSERT_IS_DISARMED
    DRAKE_ASSERT_IS_ARMED to MALIPUT_DRAKE_ASSERT_IS_ARMED
    ```
    And replace all the occurrences.

11. Commit all the changes.

12. Run the steps listed in the `Namespacing` section. Make a new commit per
    change.

13. Test the workspace before making a PR.

### Namespacing

The following transformations are necessary to successfully namespace copied
files:

1. Replace occurrences of `namespace drake` by `namespace maliput::drake`.
2. Replace occurrences of `::drake` by `::maliput::drake`.
3. Replace occurrences of `#include "drake/` by `#include "maliput/drake`.
4. Replace occurrences of `<drake::` by `<maliput::drake`.
5. Replace occurrences of `(drake::` by `(maliput::drake::`.
6. Replace occurrences of ` drake::` by ` maliput::drake`.
7. Replace occurrences of `<::drake` by `<::maliput::drake`.

## Build

1. Setup a maliput_drake (or a wider maliput) development workspace as described [here](https://github.com/ToyotaResearchInstitute/maliput_documentation/blob/main/docs/installation_quickstart.rst).

2. Bring up your development workspace:

```sh
cd path/to/my/workspace
source /opt/ros/$ROS_DISTRO/setup.sh
```

where `$ROS_DISTRO` could be either `foxy` or `dashing`.

3. Build `maliput_drake` packages and their dependencies:

   ```sh
   colcon build --packages-up-to maliput_drake
   ```

## Use `maliput_drake` as a dependency

### Installed targets

The following targets are installed and can be used when adding `maliput_drake`
as an `ament` dependency.

- `maliput_drake::common`
- `maliput_drake::trajectories`
- `maliput_drake::math`
- `maliput_drake::analysis`
- `maliput_drake::framework`

All of them are shared libraries.

### Adding `maliput_drake` to you `package.xml`

When creating the `ament` package, make sure to add `maliput_drake` to your `package.xml`:

```xml
<depend>maliput_drake</depend>
```

### Finding the package

In your project, add the following to your `CMakeLists.txt` file to import the package:

```cmake
find_package(maliput_drake REQUIRED)
```

### Export the `ament` dependency

Make sure to export the dependency as follows:

```cmake
ament_export_dependencies(maliput_drake)
```

### Linking against the libraries

To link against one or man of the aforementioned targets, simply do the following:

```cmake
target_link_libraries(
  my_fancy_target
    PUBLIC # PRIVATE
    maliput_drake::analysis
    maliput_drake::common
    maliput_drake::framework
    maliput_drake::math
    maliput_drake::trajectories
)
```

Prefer `PRIVATE` linkage when possible to avoid any unneeded exposure of `drake`-specific
types.

### For development

It is recommended to follow the guidelines for setting up a development workspace as described [here](https://maliput.readthedocs.io/en/latest/developer_setup.html).

## Contributing

Please see [CONTRIBUTING](https://maliput.readthedocs.io/en/latest/contributing.html) page.

## License

[![License](https://img.shields.io/badge/License-BSD_3--Clause-blue.svg)](https://github.com/maliput/maliput_drake/blob/main/LICENSE)
