cmake_minimum_required(VERSION 3.1)

project(nanovg_example_glfw_integration)

set(CMAKE_CXX_STANDARD 20)

set (BUILD_SHARED_LIBS OFF)
set( GLFW_BUILD_DOCS OFF CACHE BOOL ""  )
set( GLFW_BUILD_TESTS OFF CACHE BOOL ""  )
set( GLFW_BUILD_EXAMPLES OFF CACHE BOOL ""  )
set( GLFW_BUILD_INSTALL OFF CACHE BOOL ""  )

add_subdirectory(../glfw glfw)

option(BUILD_EXAMPLE_VULKAN "Build Example using Vulkan GLFW" ON)
option(BUILD_EXAMPLE_VULKAN_MIN "Build minimal Example using Vulkan without GLFW" ON)
option(BUILD_EXAMPLE_VULKAN_GLFW_INTEGRATION "Build render pass integration Example using GLFW" ON)

option(DEMO_ANTIALIAS "Use NVG_ANTIALIAS" ON)
option(DEMO_MSAA "Use MSAA instead of NVG_ANTIALIAS" OFF)
option(DEMO_STENCIL_STROKES "NVG_STENCIL_STROKES" ON)

add_compile_definitions(DEMO_MSAA=$<BOOL:${DEMO_MSAA}>)
add_compile_definitions(DEMO_ANTIALIAS=$<BOOL:${DEMO_ANTIALIAS}>)
add_compile_definitions(DEMO_STENCIL_STROKES=$<BOOL:${DEMO_STENCIL_STROKES}>)

set(COMMON_INCLUDE ../src)

set(COMMON_SRC
    ../src/fontstash.h
    ../src/nanovg.c
    ../src/nanovg.h
    ../src/stb_image.h
    ../src/stb_truetype.h
    ../src/nanovg.c
    demo.c
    perf.c
)


if (BUILD_EXAMPLE_VULKAN_GLFW_INTEGRATION)

    find_package(Vulkan)

    if (Vulkan_FOUND)
        message(STATUS "Vulkan found ${Vulkan_LIBRARIES}")
    else()
        message(FATAL "Vulkan not found")
    endif()
    add_executable(
        example-vk_glfw_integration
        ${COMMON_SRC}
        ../src/nanovg_vk.h
        example_vulkan_glfw_integration.cpp
        vulkan_util.h
    )
    
    set(VK_TUT_INCLUDE 
      files_example_vulkan_glfw_integration/glm
    )
    
    target_include_directories(example-vk_glfw_integration PRIVATE ${COMMON_INCLUDE} ${VK_TUT_INCLUDE} ${Vulkan_INCLUDE_DIR})
    target_compile_definitions(example-vk_glfw_integration PRIVATE NANOVG_VULKAN_IMPLEMENTATION)
    target_link_libraries(example-vk_glfw_integration PRIVATE glfw ${Vulkan_LIBRARY})
    # let Visual Studio debugger find the shader files
    set_target_properties(example-vk_glfw_integration PROPERTIES VS_DEBUGGER_WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
    
endif()

if (MSVC)
    add_compile_definitions(_CRT_SECURE_NO_WARNINGS )
    set_source_files_properties(
        example_vulkan_glfw_integration.cpp
        demo.c
        PROPERTIES COMPILE_FLAGS
        /wd4244
    )
    set(BUILD_EXE_POSTFIX "-$<LOWER_CASE:${CMAKE_CXX_COMPILER_ID}-$<CONFIG>>" CACHE STRING "Append to executable name")

    set(ALL_TARGETS example-vk_glfw_integration)
    foreach(TARGET_NAME ${ALL_TARGETS})
      if (TARGET ${TARGET_NAME})
        set_property(TARGET ${TARGET_NAME} PROPERTY OUTPUT_NAME ${TARGET_NAME}${BUILD_EXE_POSTFIX})
        set_property(TARGET ${TARGET_NAME} PROPERTY RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}$<0:...>)
      endif()
    endforeach()
endif()

set(CMAKE_VERBOSE_MAKEFILE ON)
