add_library(common
  cond.cc
  double_overloads.cc
  drake_assert_and_throw.cc
  drake_marker.cc
  hash.cc
  nice_type_name.cc
  nice_type_name_override.cc
  pointer_cast.cc
  polynomial.cc
  random.cc
  sorted_pair.cc
  symbolic.cc
  symbolic_chebyshev_basis_element.cc
  symbolic_chebyshev_polynomial.cc
  symbolic_codegen.cc
  symbolic_decompose.cc
  symbolic_environment.cc
  symbolic_expression.cc
  symbolic_expression_cell.cc
  symbolic_formula.cc
  symbolic_formula_cell.cc
  symbolic_generic_polynomial.cc
  symbolic_ldlt.cc
  symbolic_monomial_basis_element.cc
  symbolic_monomial.cc
  symbolic_monomial_util.cc
  symbolic_polynomial_basis_element.cc
  symbolic_polynomial.cc
  symbolic_rational_function.cc
  symbolic_simplification.cc
  symbolic_variable.cc
  symbolic_variables.cc
  text_logging.cc
  value.cc
)

add_library(maliput_drake::common ALIAS common)

set_target_properties(
  common
  PROPERTIES
    OUTPUT_NAME ${PROJECT_NAME}_common
)

target_include_directories(
  common
  PUBLIC
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

target_link_libraries(
  common
    dl
    Eigen3::Eigen
    fmt::fmt
)

install(
  TARGETS common
  EXPORT ${PROJECT_NAME}-targets
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
)

add_subdirectory(trajectories)