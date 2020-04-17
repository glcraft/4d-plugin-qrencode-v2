cmake_minimum_required (VERSION 3.10)
project (F4Dsupport)
file(GLOB F4Dsupport_SRC
    "4DPlugin-JSON.cpp"
    "4DPlugin-JSON.h"
)
add_library(F4Dsupport STATIC ${F4Dsupport_SRC})
target_include_directories(F4Dsupport INTERFACE ${PROJECT_SOURCE_DIR})
target_link_libraries(F4Dsupport PRIVATE 
    F4D
)