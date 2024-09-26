file(GLOB HEADER_LIST CONFIGURE_DEPENDS "${PROJECT_SOURCE_DIR}/include/*.h")

add_library(nam SHARED
	PRIVATE
		activations.cpp
		convnet.cpp
		dsp.cpp
		get_dsp.cpp
		lstm.cpp
		util.cpp
		wavenet.cpp
	PUBLIC
		${HEADER_LIST})

target_include_directories(nam PUBLIC ../include)

target_link_libraries(nam PRIVATE Eigen3::Eigen nlohmann_json::nlohmann_json)

source_group(
	TREE "${PROJECT_SOURCE_DIR}/include"
	PREFIX "Header Files"
	FILES ${HEADER_LIST})
