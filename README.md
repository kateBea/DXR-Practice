# DXR-Practice

DXR Practice following tutorial at: [NVIDIA Developer DXR Tutorial](https://developer.nvidia.com/rtx/raytracing/dxr/dx12-raytracing-tutorial-part-2)

## Requirements

- Windows 10 or later
- A DirectX Raytracing (DXR)-capable GPU and current graphics drivers
- Visual Studio with the Desktop development with C++ workload
- Windows 10/11 SDK
- CMake 3.20 or later
- DirectX Shader Compiler (DXC), including `dxcompiler.lib` and `dxcompiler.dll`

## Build

Open PowerShell in the project directory and run:

```powershell
cmake -S . -B build -A x64
cmake --build build --config Debug --parallel
```

The executable and copied shader files are placed in `build/bin/Debug/`.
Use `Release` instead of `Debug` for an optimized build:

```powershell
cmake --build build --config Release --parallel
```
