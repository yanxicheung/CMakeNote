# 概述：

​		CMake是一个比make更高级的编译配置工具，它可以根据不同平台、不同的编译器，生成相应的Makefile或者vcproj项目。通过编写CMakeLists.txt，可以控制生成的Makefile，从而控制编译过程。

​		我们经常使用CMake自动生成的Makefile来构建项目生成目标文件，安装文件。本文主要介绍几种常见的工程结构及对应的CMakeList文件的写法。



# case1：

入门例子，所有文件都在同一个文件夹下。

源代码来自于：

## 目录结构：

```cmake
.
├── CMakeLists.txt
├── main.cpp
├── MathFunctions.cpp
└── MathFunctions.h
```

## CMakeLists:

```cmake
# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

# 项目信息
project (Demo2)

# 查找目录下的所有源文件
# 并将名称保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)

# 指定生成目标
add_executable(Demo ${DIR_SRCS})
```



# case2：

多个目录，多个文件的例子。

源代码来自于：

## 目录结构：

```bash
.
├── CMakeLists.txt
├── include
│   ├── Amount.h
│   └── Length.h
├── source
│   └── Length.cpp
└── test
    ├── LengthTest.cpp
    └── main.cpp
```

## CMakeLists:

```cmake
# 设置工程名称
project(quantity) 
cmake_minimum_required(VERSION 2.8)

# 用于设置环境变量
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")

# 设置头文件包含路径
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

# 遍历文件
file(GLOB_RECURSE all_files
source/*.cpp
source/*.cc
source/*.c
test/*.cpp
test/*.cc
test/*.c)

# 生成目标可执行文件
add_executable(quantity-test ${all_files})

# 由于使用了gtest框架,所以需要链接这个库
target_link_libraries(quantity-test gtest)
```



# case3：

case3是一个多级目录使用CMakeLists.txt进行编译的例子。

源代码来自于：https://github.com/yanxicheung/CMakeLists

## 目录结构：

```bash
.
├── CMakeLists.txt               //  顶层CMakeList
├── hello
│   ├── CMakeLists.txt
│   ├── include
│   │   └── hello.h
│   └── source
│       └── hello.cpp
├── main.cpp
└── world
    ├── CMakeLists.txt
    ├── include
    │   └── world.h
    └── source
        └── world.cpp
```



## CMakeLists:

### 顶层：

```cmake
cmake_minimum_required(VERSION 2.8)

set(curr_dir ${CMAKE_CURRENT_SOURCE_DIR})
set(hello_dir ${curr_dir}/hello)
set(world_dir ${curr_dir}/world)

project(helloworld)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

# Add header file include directories
include_directories(
    ${hello_dir}/include
    ${world_dir}/include
)

# Add block directories
add_subdirectory(hello)
add_subdirectory(world)
# Target
add_executable(helloworld main.cpp)
target_link_libraries(helloworld hello world)
```



https://github.com/yanxicheung/cub

https://github.com/yanxicheung/transaction-dsl





# 参考文献：

1. https://www.hahack.com/codes/cmake/
2. https://github.com/wzpan/cmake-demo





