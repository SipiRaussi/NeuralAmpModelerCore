cmake_minimum_required(VERSION 3.30)

# Make sure this matches ./NAM/version.h!
project(NAM VERSION 0.3.0)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED OFF)
set(CMAKE_CXX_EXTENSIONS OFF)

if (CMAKE_CXX_COMPILER STREQUAL "clang++")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++")
endif()

if (CMAKE_SYSTEM_NAME STREQUAL "Darwin")
  include_directories(SYSTEM /usr/local/include)
elseif (CMAKE_SYSTEM_NAME STREQUAL "Linux")
  link_libraries(stdc++fs)
elseif (CMAKE_SYSTEM_NAME STREQUAL "Windows")
  add_compile_definitions(NOMINMAX WIN32_LEAN_AND_MEAN)
else()
  message(FATAL_ERROR "Unrecognized Platform!")
endif()

add_library(core SHARED)

find_package(Eigen3 REQUIRED)
find_package(nlohmann_json REQUIRED)

target_link_libraries(core PRIVATE
  Eigen3::Eigen
  nlohmann_json::nlohmann_json
)

add_subdirectory(include)
add_subdirectory(src)

install(TARGETS core
  EXPORT coreTargets
  LIBRARY DESTINATION lib
  INCLUDES DESTINATION include/nam
  FILE_SET core_headers DESTINATION include/nam
  FILE_SET core_configured_headers DESTINATION include/nam
)

install(EXPORT coreTargets
  FILE coreTargets.cmake
  DESTINATION lib/cmake/nam
)

include(CMakePackageConfigHelpers)

configure_package_config_file(
  ${CMAKE_CURRENT_SOURCE_DIR}/cmake/coreConfig.cmake.in
  "${CMAKE_CURRENT_BINARY_DIR}/cmake/coreConfig.cmake"
  INSTALL_DESTINATION "lib/cmake/nam"
  NO_SET_AND_CHECK_MACRO
  NO_CHECK_REQUIRED_COMPONENTS_MACRO
)

write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/cmake/coreVersion.cmake"
  VERSION "${CMAKE_PROJECT_VERSION}"
  COMPATIBILITY AnyNewerVersion
)

install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/cmake/coreConfig.cmake
  ${CMAKE_CURRENT_BINARY_DIR}/cmake/coreVersion.cmake
  DESTINATION lib/cmake/nam
)

export(EXPORT coreTargets
  FILE "${CMAKE_CURRENT_BINARY_DIR}/cmake/coreTargets.cmake"
)
