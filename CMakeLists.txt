cmake_minimum_required(VERSION 3.20)
project(c_learning C)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

# Windows 下让 GCC 按 UTF-8 读源码，避免中文注释乱码
if (WIN32 AND CMAKE_C_COMPILER_ID MATCHES "GNU|Clang")
    add_compile_options(-finput-charset=UTF-8 -fexec-charset=UTF-8)
endif()

# 根目录和 dayXX 目录下的每个 .c 都单独编译成一个程序
file(GLOB ROOT_SOURCES CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/*.c")
file(GLOB DAY_SOURCES CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/day*/*.c")

foreach (src ${ROOT_SOURCES} ${DAY_SOURCES})
    file(RELATIVE_PATH rel_path "${CMAKE_CURRENT_SOURCE_DIR}" "${src}")
    string(REPLACE "/" "_" target_name "${rel_path}")
    string(REPLACE ".c" "" target_name "${target_name}")
    add_executable(${target_name} "${src}")
endforeach()