cmake_minimum_required(VERSION 3.20)

project(DXRPractice LANGUAGES CXX)

if(NOT WIN32)
	message(FATAL_ERROR "DXRPractice requires Windows and the DirectX 12 SDK")
endif()

set(DXR_SOURCES
	D3D12HelloTriangle.cpp
	DXSample.cpp
	Main.cpp
	Win32Application.cpp
	stdafx.cpp
	nv_helpers_dx12/BottomLevelASGenerator.cpp
	nv_helpers_dx12/RaytracingPipelineGenerator.cpp
	nv_helpers_dx12/RootSignatureGenerator.cpp
	nv_helpers_dx12/ShaderBindingTableGenerator.cpp
	nv_helpers_dx12/TopLevelASGenerator.cpp
)

set(DXR_HEADERS
	D3D12HelloTriangle.h
	DXRHelper.h
	DXSample.h
	DXSampleHelper.h
	Win32Application.h
	d3dx12.h
	stdafx.h
	nv_helpers_dx12/BottomLevelASGenerator.h
	nv_helpers_dx12/RaytracingPipelineGenerator.h
	nv_helpers_dx12/RootSignatureGenerator.h
	nv_helpers_dx12/ShaderBindingTableGenerator.h
	nv_helpers_dx12/TopLevelASGenerator.h
)

set(DXR_SHADERS
	Common.hlsl
	Hit.hlsl
	Miss.hlsl
	RayGen.hlsl
	shaders.hlsl
)

add_executable(DXRPractice WIN32 ${DXR_SOURCES} ${DXR_HEADERS})

target_compile_features(DXRPractice PRIVATE cxx_std_17)
target_precompile_headers(DXRPractice PRIVATE
	stdafx.h
	<algorithm>
	<stdexcept>
)
target_compile_definitions(DXRPractice PRIVATE
	UNICODE
	_UNICODE
	WIN32_LEAN_AND_MEAN
)
target_compile_options(DXRPractice PRIVATE
	"$<$<CXX_COMPILER_ID:MSVC>:/FIstdexcept>"
)
target_include_directories(DXRPractice PRIVATE
	${CMAKE_CURRENT_SOURCE_DIR}
	${CMAKE_CURRENT_SOURCE_DIR}/nv_helpers_dx12
)

target_link_libraries(DXRPractice PRIVATE
	d3d12
	dxgi
	d3dcompiler
	dxcompiler
)

set_target_properties(DXRPractice PROPERTIES
	RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/$<CONFIG>"
)

foreach(shader IN LISTS DXR_SHADERS)
	add_custom_command(TARGET DXRPractice POST_BUILD
		COMMAND ${CMAKE_COMMAND} -E copy_if_different
			"${CMAKE_CURRENT_SOURCE_DIR}/${shader}"
			"$<TARGET_FILE_DIR:DXRPractice>/${shader}"
	)
endforeach()
