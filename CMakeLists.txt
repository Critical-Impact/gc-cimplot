cmake_minimum_required(VERSION 3.10)
project(cimplot CXX)

set(CMAKE_CXX_STANDARD 17)

# Set default build type if not defined
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif()

add_compile_definitions(
        NDEBUG
        CIMPLOT_EXPORTS
        _WINDOWS
        _USRDLL
)

# Source files
set(SOURCES
        ../../lib/cimgui/imgui/imgui.cpp
        ../../lib/cimgui/imgui/imgui_demo.cpp
        ../../lib/cimgui/imgui/imgui_draw.cpp
        ../../lib/cimgui/imgui/imgui_tables.cpp
        ../../lib/cimgui/imgui/imgui_widgets.cpp

        ../../lib/cimplot/cimplot.cpp
        ../../lib/cimplot/implot/implot.cpp
        ../../lib/cimplot/implot/implot_demo.cpp
        ../../lib/cimplot/implot/implot_items.cpp
)

# Header files
set(HEADERS
        ../../lib/cimplot/cimplot.h
)

# Include directories
include_directories(
        ../../lib/cimgui
        ../../lib/cimgui/imgui
        ../../lib/cimplot/implot
)

# Create shared library
add_library(cimplot SHARED ${SOURCES} ${HEADERS})

# Platform-specific definitions
if(WIN32)
    target_compile_definitions(cimplot PRIVATE
            CIMPLOT_EXPORTS
            _WINDOWS
            _USRDLL
    )
else()
    target_compile_definitions(cimplot PRIVATE
            CIMPLOT_EXPORTS
    )
    set_target_properties(cimplot PROPERTIES PREFIX "lib")
endif()

# Output directories
set_target_properties(cimplot PROPERTIES
        RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin"
        LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin"
        ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin"
)