cmake_minimum_required(VERSION 3.10)
project(qrencode)
find_path(QRENCODE_INCLUDE_DIR NAMES qrencode.h)
find_library(QRENCODE_LIBRARY_RELEASE qrencode)
find_library(QRENCODE_LIBRARY_DEBUG qrencoded)
set(QRENCODE_LIBRARIES optimized ${QRENCODE_LIBRARY_RELEASE} debug ${QRENCODE_LIBRARY_DEBUG})

find_package(jsoncpp CONFIG REQUIRED)
find_package(libpng CONFIG REQUIRED)

add_library(qrencode SHARED 4DPlugin-qrencode.cpp)
add_subdirectory("4D Plugin API")
add_subdirectory("support")
add_dependencies(qrencode F4D)
target_include_directories(qrencode PRIVATE 
    ${QRENCODE_INCLUDE_DIR}
)
target_link_libraries(qrencode PRIVATE 
    ${QRENCODE_LIBRARIES}
    png_static
    jsoncpp_lib
    F4D
    F4Dsupport
)

if (WIN32)
    target_compile_definitions(qrencode PUBLIC VERSIONWIN)
else()
    target_compile_definitions(qrencode PUBLIC VERSIONMAC)
endif()