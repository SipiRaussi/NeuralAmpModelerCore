configure_file(
  "version.h.in"
  "version.h"
)

target_sources(namcore PUBLIC
  FILE_SET core_configured_headers
  TYPE HEADERS
  BASE_DIRS ${CMAKE_CURRENT_BINARY_DIR}
  FILES ${CMAKE_CURRENT_BINARY_DIR}/version.h
)

target_sources(namcore PUBLIC
  FILE_SET core_headers
  TYPE HEADERS
  FILES
    activations.h
    convnet.h
    dsp.h
    lstm.h
    util.h
    wavenet.h
)

target_include_directories(namcore PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
  $<INSTALL_INTERFACE:include/nam>
)

