---
title: cmake
tags:
  - Linux
categories:
  - linux
date: 2018-01-03 13:42:39
---

## what's cmake

1. CMakeLists.txt
2. `cmake PATH` or `ccmake PATH` generate `Makefile`
3. make

## example for one file

In the fold `Demo1`

makeup `CMakeLists.txt`

```
# Version
cmake_minimum_required (VERSION 2.8)

# Project information
project (Demo1)

# generate object
add_executable(Demo main.cc)
```

complie project

cmake .


## example for multi-files

```
./Demo2
    |
    +--- main.cc
    |
    +--- MathFunctions.cc
    |
    +--- MathFunctions.h
```

```
# VERSION
cmake_minimum_required (VERSION 2.8)

# Project information
project (Demo2)

# find source file save to DIR_SRCS
aux_source_directory(. DIR_SRCS)

# generate object
add_executable(Demo main.cc MathFunctions.cc) # add MathFunctions.cc
```

```
aux_source_directory(<dir> <variable>)
```


```
./Demo3
    |
    +--- main.cc
    |
    +--- math/
          |
          +--- MathFunctions.cc
          |
          +--- MathFunctions.h
```


```
cmake_minimum_required (VERSION 2.8)

project (Demo3)

aux_source_directory(. DIR_SRCS)

# add subdirectory
add_subdirectory(math)

add_executable(Demo main.cc)

# add link lib
target_link_libraries(Demo MathFunctions)
```

## complile options

```
cmake_minimum_required (VERSION 2.8)

project (Demo4)

configure_file (
	"${PROJECT_SOURCE_DIR}/config.h.in"
	"${PROJECT_BINARY_DIR}/config.h"
  )

# MathFunctions
option (USE_MYMATH
       "Use provided math implementation" ON)

#  MathFunctions
if (USE_MYMATH)
	include_directories ("${PROJECT_SOURCE_DIR}/math")
	add_subdirectory (math)
	set (EXTRA_LIBS ${EXTRA_LIBS} MathFunctions)
endif (USE_MYMATH)

aux_source_directory(. DIR_SRCS)

add_executable(Demo ${DIR_SRCS})
target_link_libraries (Demo  ${EXTRA_LIBS})
```


## install and test

add_test

## support gbd

```
set(CMAKE_BUILD_TYPE "Debug")
set(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb")
set(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")
```

## evironment check

CheckFunctionExists

## version

```
set (Demo_VERSION_MAJOR 1)
set (Demo_VERSION_MINOR 0)
```

## install package
```
include (InstallRequiredSystemLibraries)
set (CPACK_RESOURCE_FILE_LICENSE
	"${CMAKE_CURRENT_SOURCE_DIR}/License.txt")
set (CPACK_PACKAGE_VERSION_MAJOR "${Demo_VERSION_MAJOR}")
set (CPACK_PACKAGE_VERSION_MINOR "${Demo_VERSION_MINOR}")
include (CPack)
```
## move to cmake

- autotools
	- am2cmake
- qmake
	- qmake converter
- visual studio
