# Description
CMake wrapper for `stb` lib

# Getting sources
Download from github:
```bash
$ git clone https://github.com/LazyMechanic/stb-cmake --recursive
```

# CMake configuration
Create a temporary *build* folder and change your working directory to it:
```bash
$ cd cpp/
$ mkdir build
$ cd build/
```

Build cmake:
```bash
$ cmake ..
```

## Installing
To install `stb` to the specified install folder:
```bash
$ cmake --install . --prefix /your/path/to/install/stb
```

# How to use
## Find package
Set `stb_DIR` to installed `stb-config.cmake` file:
```cmakes
set(stb_DIR /path/to/directory/with/config) # For example: "D:/dev/stb/lib/cmake/stb"
```
or in cmake command arguments:
```bash
-Dstb_DIR=/path/to/directory/with/config
```

Find the package:
```cmake
find_package(stb CONFIG)

if (NOT ${stb_FOUND})
    message(FATAL_ERROR "stb couldn't be located")
endif()
```

## Link library
Link library to target:
```cmake
add_executable(SomeTarget) # Your target

target_link_libraries(
    SomeTarget 
    stb::stb)
```
Include directory will automatically be added to *SomeTarget*