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

find_package(Eigen3 REQUIRED)
find_package(nlohmann_json REQUIRED)

add_subdirectory(include)
add_subdirectory(src)

install(TARGETS nam
  EXPORT namTargets
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES DESTINATION include/nam
)

install(
  FILES ${HEADER_FILES}
  DESTINATION include/nam
)

install(EXPORT namTargets
  FILE namTargets.cmake
  DESTINATION lib/cmake/nam
)

include(CMakePackageConfigHelpers)

configure_package_config_file(
  ${CMAKE_CURRENT_SOURCE_DIR}/cmake/namConfig.cmake.in
  "${CMAKE_CURRENT_BINARY_DIR}/cmake/namConfig.cmake"
  INSTALL_DESTINATION "lib/cmake/nam"
  NO_SET_AND_CHECK_MACRO
  NO_CHECK_REQUIRED_COMPONENTS_MACRO
)

write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/cmake/namVersion.cmake"
  VERSION "${CMAKE_PROJECT_VERSION}"
  COMPATIBILITY AnyNewerVersion
)

install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/cmake/namConfig.cmake
  ${CMAKE_CURRENT_BINARY_DIR}/cmake/namVersion.cmake
  DESTINATION lib/cmake/nam
)

export(EXPORT namTargets
  FILE "${CMAKE_CURRENT_BINARY_DIR}/cmake/namTargets.cmake"
)
