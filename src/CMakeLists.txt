file(GLOB HEADER_LIST CONFIGURE_DEPENDS "${NeuralAmpModelerCore_SOURCE_DIR}/include/*.h")

add_library(neural_amp_modeler_lib
	activations.cpp
	convnet.cpp
	dsp.cpp
	get_dsp.cpp
	lstm.cpp
	util.cpp
	wavenet.cpp
	${HEADER_LIST})

target_include_directories(neural_amp_modeler_lib PUBLIC ../include)

target_link_libraries(neural_amp_modeler_lib PRIVATE Eigen3::Eigen)

source_group(
	TREE "${PROJECT_SOURCE_DIR}/include"
	PREFIX "Header Files"
	FILES ${HEADER_LIST})
