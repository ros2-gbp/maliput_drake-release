add_library(math
  autodiff.cc
  autodiff_gradient.cc
  barycentric.cc
  bspline_basis.cc
  continuous_algebraic_riccati_equation.cc
  continuous_lyapunov_equation.cc
  cross_product.cc
  discrete_algebraic_riccati_equation.cc
  discrete_lyapunov_equation.cc
  eigen_sparse_triplet.cc
  evenly_distributed_pts_on_sphere.cc
  fast_pose_composition_functions.cc
  gradient.cc
  gray_code.cc
  hopf_coordinate.cc
  jacobian.cc
  matrix_util.cc
  normalize_vector.cc
  orthonormal_basis.cc
  quadratic_form.cc
  quaternion.cc
  random_rotation.cc
  rigid_transform.cc
  roll_pitch_yaw.cc
  rotation_conversion_gradient.cc
  rotation_matrix.cc
)

add_library(maliput_drake::math ALIAS math)

set_target_properties(
  math
  PROPERTIES
    OUTPUT_NAME ${PROJECT_NAME}_math
)

target_include_directories(
  math
  PUBLIC
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

target_link_libraries(
  math
    Eigen3::Eigen
    fmt::fmt
    maliput_drake::common
)

install(
  TARGETS math
  EXPORT ${PROJECT_NAME}-targets
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
)
