add_library(nam SHARED
    activations.cpp
    convnet.cpp
    dsp.cpp
    get_dsp.cpp
    lstm.cpp
    util.cpp
    wavenet.cpp
)

target_link_libraries(nam
  PRIVATE
    Eigen3::Eigen
    nlohmann_json::nlohmann_json
  )

target_include_directories(nam PUBLIC
  $<BUILD_INTERFACE:${INCLUDE_DIR}>
  $<INSTALL_INTERFACE:include/nam>
)

