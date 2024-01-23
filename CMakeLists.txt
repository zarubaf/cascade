cmake_minimum_required(VERSION 3.7.0)
project(cascade)

set(CMAKE_CXX_STANDARD 11)

file(GLOB DESCORE_SRCS src/descore/*.cpp)
file(GLOB CASCADE_SRCS src/cascade/*.cpp)

add_library(descore
    ${DESCORE_SRCS}
)

add_library(cascade
    ${CASCADE_SRCS}
)

target_include_directories(descore
    PUBLIC
    include/descore
)

target_include_directories(cascade
    PUBLIC
    include/
    include/cascade
)

target_link_libraries(cascade descore)

# Create cascade_verilog library with _VERILOG define set
add_library(cascade_verilog
    ${CASCADE_SRCS}
)
target_include_directories(cascade_verilog
    PUBLIC
    include/
    include/cascade
    $ENV{VCS_HOME}/include
)

target_compile_definitions(cascade_verilog PRIVATE -D_VERILOG)
target_link_libraries(cascade_verilog descore)

file(GLOB LIFE_SRCS examples/life/*.cpp)
file(GLOB ADDER_SRCS examples/adder_verilog/*.cpp)

add_executable(life
    ${LIFE_SRCS}
)
target_link_libraries(life cascade -lz -lpthread)

add_library(adder
    ${ADDER_SRCS}
)
target_link_libraries(adder cascade -lz -ltermcap -lpthread)