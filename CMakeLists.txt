cmake_minimum_required(VERSION 3.0)
project(drishti_faces)

set(DATA_INSTALL_DIR "share/${PROJECT_NAME}")
set(config_install_dir "lib/cmake/${PROJECT_NAME}")
set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")
set(project_config "${generated_dir}/${PROJECT_NAME}Config.cmake")

set(INDEX_NAME "${PROJECT_NAME}.json")
set(generated_index "${generated_dir}/${INDEX_NAME}")

include(CMakePackageConfigHelpers)

# TODO: Once face data is added we will probably need a GLOB here

# Initial list of unit test eye + face images
set(test_eye_image "2318-eye.png")
set(test_eye_model "2318-eye.json")
set(test_eye_model_private "2318-eye_private.json")
set(test_face_image "lena512color.png")

# Use variables:
# * PROJECT_NAME
# * FACTORY_NAME
configure_package_config_file(
    "cmake/Config.cmake.in"
    "${project_config}"
    INSTALL_DESTINATION ${config_install_dir}
    PATH_VARS DATA_INSTALL_DIR
)

install(FILES "${project_config}" DESTINATION "${config_install_dir}")

# Build a top level test file index (similar to drishti-assets)
# We may want to export variables for these images as well.

# Use variables:
# * test_eye_image
# * test_eye_model
# * test_eye_model_private
# * test_face_image
configure_file("cmake/json.cmake.in" ${generated_index} @ONLY)

install(
  FILES
  ${test_eye_image}
  ${test_eye_model}
  ${test_eye_model_private}
  ${test_face_image}
  ${generated_index}
  DESTINATION
  ${DATA_INSTALL_DIR}
)
