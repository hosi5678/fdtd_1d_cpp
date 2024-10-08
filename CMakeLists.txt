# CMakeのバージョン指定
cmake_minimum_required(VERSION 3.5)
cmake_policy(SET CMP0148 NEW)

# compilerのpath設定
set(CMAKE_CXX_COMPILER "/usr/bin/g++-11")
set(CMAKE_SUPPRESS_DEVELOPER_WARNINGS 1)

find_package(Python REQUIRED COMPONENTS Interpreter Development)

# python include directory

include_directories(${PYTHON_INCLUDE_DIRS} /usr/include/python3.11)
include_directories(${Python3_NumPy_INCLUDE_DIRS})

# buildの設定
set(CMAKE_BUILD_TYPE Debug)

#プロジェクトの指定と言語の指定
project(my_cpp_prac)

# targetの名前の設定
add_executable(main main.cpp)

# sub directoryの設定
add_subdirectory(./sub_dir)

target_link_libraries(main ${PYTHON_LIBRARIES})

# sub directoryをtargetに結合
target_link_libraries(main cpp_lib)

target_compile_options(main PRIVATE -Wall -Wextra)
target_compile_options(main PRIVATE -Wno-deprecated-declarations)
target_compile_options(main PRIVATE -Wno-dev)
target_compile_options(main PRIVATE -O2)
target_compile_options(main PRIVATE -std=c++11)

# ビルドの高速化
target_compile_options(main PRIVATE -pipe)

# 自動的にそのアーキテクチャにそったオプションをつける
target_compile_options(main PRIVATE -march=native)

# ループの展開を行う。
target_compile_options(main PRIVATE -funroll-loops)

# 計算をメモリ上ではなくCPUのレジスタにコピーして行うようにする
target_compile_options(main PRIVATE -fforce-addr)

# CPUの拡張命令セットを使うようにする
target_compile_options(main PRIVATE -msse -msse2 -msse3)
target_compile_options(main PRIVATE -m3dnow)
target_compile_options(main PRIVATE -mfpmath=sse)

# open mp用
target_compile_options(main PRIVATE -fopenmp)

# math.hを使用するためのライブラリのリンク
target_link_libraries(main m)

# カスタムターゲットの定義,即座に実行する
add_custom_target(run_main
    COMMAND $<TARGET_FILE:main>
    DEPENDS main
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)

# エイリアスの定義,make runで実行できる
# add_custom_target(run ALL DEPENDS run_main)
# エイリアスの定義、make runでプログラムを実行可能にする
add_custom_target(run
    DEPENDS run_main
)
