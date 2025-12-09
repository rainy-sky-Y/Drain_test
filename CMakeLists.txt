cmake_minimum_required(VERSION 3.18)  # NDK 24 推荐 3.18+，兼容 Android toolchain

set(CMAKE_TOOLCHAIN_FILE "E:/IDE/Android/Sdk/ndk/24.0.8215888/build/cmake/android.toolchain.cmake")

# 配置 Android 编译参数
set(ANDROID_ABI "arm64-v8a")        # 目标架构（可选：armeabi-v7a/x86/x86_64）
set(ANDROID_PLATFORM "android-24")  # 目标 Android 版本（需 ≥24，匹配 NDK 24）
set(CMAKE_SYSTEM_NAME "Android")    # 明确编译 Android 目标，不是 Windows

# 项目名称
project(Drain_test C CXX)

# 定义 Android 可执行文件
add_executable(Drain_test
        main.cpp
)

# 编译选项
target_compile_options(Drain_test PRIVATE
        -g
        -O0
        -fno-exceptions  # 可选：关闭异常，彻底避开 gcc_eh 依赖（或保留异常，用 c++_shared STL）
)
