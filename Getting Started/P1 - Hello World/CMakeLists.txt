cmake_minimum_required(VERSION 3.23)
project(hello)

set(CMAKE_CXX_STANDARD 14)
set(LLVM_VERSION 15)

set(LLVM_INCLUDE /usr/include/llvm-${LLVM_VERSION})
set(LLVM_C_INCLUDE /usr/include/llvm-c-${LLVM_VERSION})
set(LLVM_CONFIG llvm-config-${LLVM_VERSION})

# 设置LLVM库
include_directories(${LLVM_INCLUDE})
include_directories(${LLVM_C_INCLUDE})

# c++ flags
execute_process(
        COMMAND ${LLVM_CONFIG} --cxxflags
        OUTPUT_VARIABLE llvm_cxxflags
        OUTPUT_STRIP_TRAILING_WHITESPACE)

# 连接器 flags
execute_process(
        COMMAND ${LLVM_CONFIG} --ldflags
        OUTPUT_VARIABLE llvm_ldflags
        OUTPUT_STRIP_TRAILING_WHITESPACE)
# 库 flags
execute_process(
        COMMAND ${LLVM_CONFIG} --system-libs --libs core mcjit
        OUTPUT_VARIABLE llvm_libs
        OUTPUT_STRIP_TRAILING_WHITESPACE)

set(LLVM_FLAGS "${llvm_cxxflags} ${llvm_ldflags} ${llvm_libs}")
set(CMAKE_CXX_FLAGS ${LLVM_FLAGS})

add_executable(hello main.cpp)
