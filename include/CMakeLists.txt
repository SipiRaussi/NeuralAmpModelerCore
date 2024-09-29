set(INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}")

set(HEADER_FILES
  ${INCLUDE_DIR}/activations.h
  ${INCLUDE_DIR}/convnet.h
  ${INCLUDE_DIR}/dsp.h
  ${INCLUDE_DIR}/lstm.h
  ${INCLUDE_DIR}/util.h
  ${INCLUDE_DIR}/wavenet.h
)

configure_file(
  "version.h.in"
  "version.h"
)

set(INCLUDE_DIR ${INCLUDE_DIR} PARENT_SCOPE)

set(HEADER_FILES
  ${HEADER_FILES}
  ${CMAKE_CURRENT_BINARY_DIR}/version.h
  PARENT_SCOPE
)
