cmake_minimum_required(VERSION 3.30)

# Make sure this matches ./NAM/version.h!
project(NAM VERSION 0.3.0)

configure_file(include/version.h.in include/version.h)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED OFF)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++")

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

add_subdirectory(src)

install(TARGETS nam LIBRARY DESTINATION lib)
install(FILES "${PROJECT_BINARY_DIR}/include/version.h" DESTINATION include)

file(GLOB HEADER_FILES "include/*.h")

install(FILES ${HEADER_FILES} DESTINATION include)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
	${PROJECT_BINARY_DIR}/cmake/namVersion.cmake
	VERSION ${PROJECT_VERSION}
	COMPATIBILITY AnyNewerVersion)

export(EXPORT namTargets FILE ${PROJECT_BINARY_DIR}/cmake/namTargets.cmake)

configure_file(${PROJECT_SOURCE_DIR}/cmake/namTargets.cmake.in
	${PROJECT_BINARY_DIR}/cmake/namTargets.cmake
	COPYONLY)

configure_package_config_file(${PROJECT_SOURCE_DIR}/cmake/namConfig.cmake.in
	${PROJECT_BINARY_DIR}/cmake/namConfig.cmake
	INSTALL_DESTINATION cmake)

install(EXPORT namTargets
	FILE ${PROJECT_BINARY_DIR}/cmake/namTargets.cmake
	DESTINATION cmake)

install(FILES
	${PROJECT_BINARY_DIR}/cmake/namConfig.cmake
	${PROJECT_BINARY_DIR}/cmake/namVersion.cmake
	DESTINATION cmake)
