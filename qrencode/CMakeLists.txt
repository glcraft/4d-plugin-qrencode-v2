cmake_minimum_required(VERSION 3.10)
project(qrencode)
if (WIN32 AND MSVC)
    set(MSVC_STATIC ON CACHE BOOL "Build with static MSVC CRT")
endif()
find_path(QRENCODE_INCLUDE_DIR NAMES qrencode.h)
find_library(QRENCODE_LIBRARY_RELEASE qrencode)
find_library(QRENCODE_LIBRARY_DEBUG qrencoded)
set(QRENCODE_LIBRARIES optimized ${QRENCODE_LIBRARY_RELEASE} debug ${QRENCODE_LIBRARY_DEBUG})

find_package(jsoncpp CONFIG REQUIRED)
find_package(libpng CONFIG REQUIRED)

if (WIN32)
    add_library(qrencode SHARED 4DPlugin-qrencode.cpp "4D Plugin API/4DPluginAPI.def")
    set_target_properties(qrencode PROPERTIES SUFFIX ".4DX")
    if (MSVC AND MSVC_STATIC)
        set(CompilerFlags
            CMAKE_CXX_FLAGS
            CMAKE_CXX_FLAGS_DEBUG
            CMAKE_CXX_FLAGS_RELEASE
            CMAKE_C_FLAGS
            CMAKE_C_FLAGS_DEBUG
            CMAKE_C_FLAGS_RELEASE
        )
        foreach(CompilerFlag ${CompilerFlags})
            string(REPLACE "/MD" "/MT" ${CompilerFlag} "${${CompilerFlag}}")
        endforeach()
    endif()
else()
    add_library(qrencode MODULE 4DPlugin-qrencode.cpp "4D Plugin API/4DPluginAPI.def")
    if ("${CMAKE_SIZEOF_VOID_P}" STREQUAL "4")
        set(OSXARCH i386)
    else()
        set(OSXARCH x86_64)
    endif()
    set_target_properties(TLSi PROPERTIES 
        BUNDLE TRUE
        BUNDLE_EXTENSION "bundle"
        OSX_ARCHITECTURES ${OSXARCH}
    )
endif()
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