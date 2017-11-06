# SQLite3

This is only the amalgamation file with CMake.

https://www.sqlite.org/download.html

I use this for personal projects to link against.

## Building

Debug:

```shell
mkdir build
cd build
cmake ..
make
```

Release:

```shell
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_EXPORT_COMPILE_COMMANDS=1 ..
make
```
