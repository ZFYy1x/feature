cmake_minimum_required(VERSION 3.25.2)

project(feature C)

set(CMAKE_C_STANDARD 99)

# 内核补丁库路径
set(KP_DIR "${CMAKE_SOURCE_DIR}/../__KernelPatch_lib")
set(INCLUDE_DIRS
    .
    include
    patch/include
    linux/include
    kpm/include
    linux/arch/arm64/include
    linux/tools/arch/arm64/include
)

# 添加头文件路径
foreach(dir ${INCLUDE_DIRS})
    include_directories(${KP_DIR}/kernel/${dir})
endforeach()

# 输出库名
set(OUTPUT_NAME "yuuki")

# 源文件
set(SOURCES
    mod_main.c
    mod_main.h
)

# 添加共享库
add_library(${OUTPUT_NAME} SHARED ${SOURCES})

# 设置输出文件名
set_target_properties(${OUTPUT_NAME} PROPERTIES OUTPUT_NAME "${OUTPUT_NAME}")

# 编译完成后将文件重命名为 .kpm
add_custom_command(TARGET ${OUTPUT_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E rename $<TARGET_FILE:${OUTPUT_NAME}> ${CMAKE_CURRENT_BINARY_DIR}/${OUTPUT_NAME}.kpm
)
