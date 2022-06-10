add_library(trajectories
  bspline_trajectory.cc
  discrete_time_trajectory.cc
  exponential_plus_piecewise_polynomial.cc
  piecewise_polynomial.cc
  piecewise_pose.cc
  piecewise_quaternion.cc
  piecewise_trajectory.cc
  trajectory.cc
)

add_library(maliput_drake::trajectories ALIAS trajectories)

set_target_properties(
  trajectories
  PROPERTIES
    OUTPUT_NAME ${PROJECT_NAME}_trajectories
)

target_include_directories(
  trajectories
  PUBLIC
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

target_link_libraries(
  trajectories
    Eigen3::Eigen
    fmt::fmt
    maliput_drake::common
    maliput_drake::math
)

install(
  TARGETS trajectories
  EXPORT ${PROJECT_NAME}-targets
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
)
