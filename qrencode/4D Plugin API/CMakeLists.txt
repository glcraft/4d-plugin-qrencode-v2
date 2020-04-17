cmake_minimum_required (VERSION 3.10)
project (F4D)
file(GLOB F4Dlib_SRC
    "*.h"
    "*.c"
)
file(GLOB F4Dclasses_SRC
    "Classes/*.h"
    "Classes/*.cpp"
)
add_library(F4D INTERFACE )
add_library(F4Dlib STATIC ${F4Dlib_SRC})
add_library(F4Dclasses STATIC ${F4Dclasses_SRC})
target_include_directories(F4Dclasses PRIVATE 
"${PROJECT_SOURCE_DIR}"
)
target_include_directories(F4D INTERFACE 
"${PROJECT_SOURCE_DIR}"
"${PROJECT_SOURCE_DIR}/Classes"
)
target_link_libraries(F4D INTERFACE F4Dlib F4Dclasses)