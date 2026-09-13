# FreeRTOS Demo 配置、编译与运行

本文档说明如何在 Windows PowerShell 中配置、编译和运行 FreeRTOS Kernel 的最小 CMake Demo。

## 1. Demo 位置

Demo 源码目录：

```text
FreeRTOS\FreeRTOS-Kernel\examples\cmake_example
```

主要文件：

- `CMakeLists.txt`：CMake 工程配置
- `main.c`：Demo 程序入口

## 2. 检查工具

需要安装并配置以下工具：

```powershell
cmake --version
gcc --version
```

如果这两个命令都能显示版本号，说明工具链可用。

## 3. 第一次配置和编译

打开 PowerShell，进入仓库根目录：

```powershell
cd D:\Code\FreeRTOS-LTS
```

执行 CMake 配置：

```powershell
cmake -S ".\FreeRTOS\FreeRTOS-Kernel\examples\cmake_example" -B ".\build" -G "MinGW Makefiles"
```

配置成功后，CMake 会在 `build` 目录中生成 Makefile 和其他构建文件。

然后执行编译：

```powershell
cmake --build ".\build"
```

也可以使用 4 个并行任务加快编译：

```powershell
cmake --build ".\build" --parallel 4
```

编译成功时会看到：

```text
[100%] Built target example
```

生成的程序为：

```text
D:\Code\FreeRTOS-LTS\build\example.exe
```

## 4. 运行 Demo

在仓库根目录运行：

```powershell
& ".\build\example.exe"
```

程序启动后会输出：

```text
Example FreeRTOS Project
```

这个 Demo 启动 FreeRTOS 调度器后会持续运行，不会自动退出。停止程序请按：

```text
Ctrl+C
```

## 5. 如果已经进入 build 目录

如果当前目录是：

```text
D:\Code\FreeRTOS-LTS\build
```

并且已经配置过 CMake，直接编译：

```powershell
cmake --build .
```

或者：

```powershell
cmake --build . --parallel 4
```

运行程序：

```powershell
.\example.exe
```

如果 PowerShell 无法直接运行，则使用：

```powershell
& ".\example.exe"
```

## 6. 如果 build 目录还没有配置

如果已经进入 `build` 目录，但是里面还没有 CMake 配置文件，执行：

```powershell
cmake -S "..\FreeRTOS\FreeRTOS-Kernel\examples\cmake_example" -B "." -G "MinGW Makefiles"
```

然后编译：

```powershell
cmake --build .
```

运行：

```powershell
.\example.exe
```

## 7. 修改代码后重新编译

如果已经成功配置过 CMake，之后只修改了 `main.c`，通常只需要重新编译：

在仓库根目录执行：

```powershell
cmake --build ".\build"
```

如果当前已经在 `build` 目录，则执行：

```powershell
cmake --build .
```

以下情况需要重新执行 CMake 配置命令：

- 第一次编译；
- 删除了 `build` 目录；
- 修改了 `CMakeLists.txt`；
- 更换了编译器；
- 更换了 CMake 生成器。

## 8. 遇到配置错误时重新开始

如果 `build` 目录中的配置损坏，可以从仓库根目录删除构建目录后重新配置：

```powershell
cd D:\Code\FreeRTOS-LTS
Remove-Item -Recurse -Force ".\build"
cmake -S ".\FreeRTOS\FreeRTOS-Kernel\examples\cmake_example" -B ".\build" -G "MinGW Makefiles"
cmake --build ".\build"
```

## 9. Clean 后重新编译

### 9.1 只清理编译产物

如果 CMake 配置仍然有效，只想删除目标文件并重新编译，可以执行：

在仓库根目录执行：

```powershell
cmake --build ".\build" --target clean
cmake --build ".\build"
```

如果当前已经进入 `build` 目录，则执行：

```powershell
cmake --build . --target clean
cmake --build .
```

使用 4 个并行任务重新编译：

```powershell
cmake --build ".\build" --target clean
cmake --build ".\build" --parallel 4
```

这种方式会保留 CMake 配置文件，不需要重新执行配置命令。

### 9.2 彻底清理并重新配置

如果想从头开始，删除整个 `build` 目录，然后重新配置和编译：

```powershell
cd D:\Code\FreeRTOS-LTS
Remove-Item -Recurse -Force ".\build"
cmake -S ".\FreeRTOS\FreeRTOS-Kernel\examples\cmake_example" -B ".\build" -G "MinGW Makefiles"
cmake --build ".\build" --parallel 4
```

运行重新生成的程序：

```powershell
& ".\build\example.exe"
```

两种 clean 方式的区别：

- `cmake --build ".\build" --target clean`：只清理编译产物，保留 CMake 配置；
- `Remove-Item -Recurse -Force ".\build"`：删除整个构建目录，之后必须重新配置 CMake。

## 10. 最简命令流程

从仓库根目录执行以下命令即可完成配置、编译和运行：

```powershell
cd D:\Code\FreeRTOS-LTS
cmake -S ".\FreeRTOS\FreeRTOS-Kernel\examples\cmake_example" -B ".\build" -G "MinGW Makefiles"
cmake --build ".\build"
& ".\build\example.exe"
```
