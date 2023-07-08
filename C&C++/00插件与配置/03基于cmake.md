# 3、基于cmake

## 1）CMakeList.txt

```c
project（"MYSWAP"）
add_executable（my_cmake_swap main.cpp swap.cpp）
```

## 2）多文件编译

```c
ctrl + shift + p
CMake Configure
```

* 或

```c
mkdir build
cd build
// 如果电脑上已安装了VS,可能会调用微软MSVC编译器,使用（cmake -G "Mingw Makefiles"..）代替（cmake..）即可
// 仅第一次使用cmake时使用（ cmake -G "MinGw Makefiles"..）后面可使用（cmake..）
cmake ..
```

```c
mingw32-make.exe
``` 

## 3）配置

### （1）launch.json --for debug

```c
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "（gdb） 启动",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/out.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${fileDirname}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "miDebuggerPath": "gdb",
            "setupCommands": [
                {
                    "description": "为 gdb 启用整齐打印",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "build"
        }
    ]
}
```

### （2） tasks.json --for build before debug

```c
{
    "version": "2.0.0",
    "options": {
        "cwd": "${workspaceFolder}/build"
    },
    "tasks": [
        {
            "label": "cmake",
            "type": "shell",
            "command": "cmake",
            "args": [
                ".."
            ]
        },
        {
            "label": "make",
            "group":{
                "kind": "build",
                "isDefault": true
            },
            "command": "mingw32-make.exe",
        },
        {
            "label":"build",
            "dependsOn":[
                "cmake",
                "make"
            ]
        }
    ]
}
```

## 4）CMakeLists.txt  

```c
project("excute")
include_directories(include)
aux_source_directory(src SRC_LIST)
add_executable(${PROJECT_NAME} ${SRC_LIST})
```
