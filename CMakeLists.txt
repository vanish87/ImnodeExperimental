# cmake minium version required
cmake_minimum_required(VERSION 3.13)

# Linux Build Command
# cmake -S . -B build_linux -DCMAKE_BUILD_TYPE=Debug
# cmake --build ./build_linux/

# cmake setting
set(CMAKE_VERBOSE_MAKEFILE ON)

# project setting
project(imnode)

# c++ setting
set(CMAKE_CXX_STANDARD 17)
if (MSVC)
    # warning level 4 and all warnings as errors
    add_compile_options(/W4 /WX)
elseif(APPLE)
    # no warning as error for macos/metal build for now
else()
    # lots of warnings and all warnings as errors
    add_compile_options(-Wall -Wextra -pedantic -Werror)
endif()

# output setting
set (CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set (CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# debug setting
set(CMAKE_DEBUG_POSTFIX d)
add_compile_options("$<$<CONFIG:DEBUG>:-DDEBUG>")



aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/source src)
add_executable(${PROJECT_NAME} ${src})

target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_link_libraries(${PROJECT_NAME}
    d3d12.lib
    dxgi.lib
    d3dcompiler.lib
)
