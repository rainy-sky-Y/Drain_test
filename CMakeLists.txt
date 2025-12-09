cmake_minimum_required(VERSION 3.18)  # NDK 24 推荐 3.18+，兼容 Android toolchain

# 关键：必须先指定 Android 工具链（否则 CMake 编译 Windows 程序）
# 注：也可通过 CLion 配置传递该参数，这里写在文件里更稳妥
set(CMAKE_TOOLCHAIN_FILE "E:/IDE/Android/Sdk/ndk/24.0.8215888/build/cmake/android.toolchain.cmake")

# 配置 Android 编译参数（必选！）
set(ANDROID_ABI "arm64-v8a")        # 目标架构（可选：armeabi-v7a/x86/x86_64）
set(ANDROID_PLATFORM "android-24")  # 目标 Android 版本（需 ≥24，匹配 NDK 24）
set(ANDROID_STL "c++_shared")       # 指定 STL，避免链接 gcc_eh 依赖
set(CMAKE_SYSTEM_NAME "Android")    # 明确编译 Android 目标，不是 Windows

# 项目名称（自定义）
project(Drain_test C CXX)

# 定义 Android 可执行文件（NDK 编译 Android 程序，不是 Windows exe）
add_executable(Drain_test
        main.cpp
)

# 编译选项：适配 NDK 24（移除 gcc_eh 依赖）
target_compile_options(Drain_test PRIVATE
        -g
        -O0
        -fno-exceptions  # 可选：关闭异常，彻底避开 gcc_eh 依赖（或保留异常，用 c++_shared STL）
)

# 链接选项：指定使用 LLVM libunwind，替代 gcc_eh
target_link_options(Drain_test PRIVATE
        -lunwind  # NDK 23+ 用 libunwind 替代 gcc_eh
)