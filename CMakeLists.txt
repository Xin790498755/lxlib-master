cmake_minimum_required(VERSION 3.10)
project(EmbeddedSystem C CPP ASM)  # 支持C和汇编

# 引入工具链配置（交叉编译）
include(cmake/gcc-arm-none-eabi.cmake)

# 通用编译选项
add_compile_options(
    -mcpu=cortex-m4  # 根据实际芯片修改
    -mthumb
    -mfloat-abi=hard
    -mfpu=fpv4-sp-d16
    -Wall
    -Wextra
    -Os
    -ffunction-sections
    -fdata-sections
)

# 预定义宏
add_definitions(
    -DGD32F30X_HD  # 芯片型号
#    -DUSE_HAL_DRIVER  # 使用HAL库
#    -DRTOS_ENABLE  # 启用RTOS（如果使用）
)

# 头文件路径（所有层的公共头文件）
include_directories(
    app/config
    app/modules

    services/task_mgr/inc
    services/event/inc
    services/utils/inc

    drivers/bsp/inc
    drivers/hal/CMSIS
    drivers/hal/CMSIS/GD/GD32F30x/Include
    drivers/hal/GD32F30x_standard_peripheral/Include
    drivers/periph/inc
)

# 收集源文件（按层递归查找）
file(GLOB_RECURSE APP_SOURCES "app/*.c")
file(GLOB_RECURSE SERVICES_SOURCES "services/*.c")
file(GLOB_RECURSE DRIVERS_SOURCES "drivers/*.c")
file(GLOB_RECURSE ASM_SOURCES "drivers/bsp/*.s")  # 启动文件

# 汇编文件处理（支持C预处理器）
set_source_files_properties(${ASM_SOURCES} PROPERTIES COMPILE_FLAGS "-x assembler-with-cpp")

# 生成可执行文件
add_executable(${PROJECT_NAME}.elf
    ${APP_SOURCES}
    ${SERVICES_SOURCES}
    ${DRIVERS_SOURCES}
    ${ASM_SOURCES}
)

# 链接脚本
target_link_options(${PROJECT_NAME}.elf PRIVATE
    -T${CMAKE_SOURCE_DIR}/gd32f30x_flash.ld  # 示例芯片
)

# 生成二进制文件（.bin/.hex）
include(cmake/create_bin.cmake)