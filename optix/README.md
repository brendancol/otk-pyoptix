# pyoptix-contrib

Community-maintained Python bindings for [NVIDIA OptiX](https://developer.nvidia.com/optix), forked from [NVIDIA/otk-pyoptix](https://github.com/NVIDIA/otk-pyoptix).

This fork adds support for OptiX 9.1 features including cluster acceleration structures and cooperative vectors.

## Requirements

- [OptiX SDK](https://developer.nvidia.com/designworks/optix/download) 7.6 or newer (9.1+ for cluster accel / coop vec features)
- [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads) 12.6 or newer
- [CMake](https://cmake.org/)
- A C++17 compiler

## Installation

```bash
export OptiX_INSTALL_DIR=/path/to/OptiX-SDK
pip install pyoptix-contrib
```

On Windows (PowerShell):

```powershell
$env:OptiX_INSTALL_DIR = 'C:\ProgramData\NVIDIA Corporation\OptiX SDK 9.1.0'
pip install pyoptix-contrib
```

The package builds from source via CMake, so the OptiX SDK must be available at install time.

For additional CMake arguments, use the `PYOPTIX_CMAKE_ARGS` environment variable.

## Usage

```python
import optix

# Create a device context
ctx = optix.deviceContextCreate(cuda_context, optix.DeviceContextOptions())

# Query device properties
rtcore_version = optix.deviceContextGetProperty(
    ctx, optix.DeviceProperty.DEVICE_PROPERTY_RTCORE_VERSION
)
```

See the [examples](https://github.com/brendancol/otk-pyoptix/tree/master/examples) directory for complete samples including triangle rendering, curves, denoising, and motion blur.

## What's New in This Fork

- **OptiX 9.1 support**: Conditional compilation via `IF_OPTIX91` macro
- **Cluster acceleration structures**: Enums, structs, and host functions (`clusterAccelComputeMemoryUsage`, `clusterAccelBuild`)
- **Cooperative vectors**: Element types, matrix layouts, and description structs
- **New primitive types**: ROCAPS curve variants and associated flags
- **`allowClusteredGeometry`** pipeline compile option
- **New device properties**: `COOP_VEC`, `CLUSTER_ACCEL`, max cluster vertices/triangles/SBT index/clusters-per-GAS

## Windows: CUDA DLL Loading (Python 3.8+)

Python 3.8+ on Windows no longer uses `PATH` to find DLLs. PyOptiX will auto-detect CUDA from the `CUDA_PATH` environment variable. If auto-detection fails, set `CUDA_BIN_DIR`:

```powershell
$env:CUDA_BIN_DIR = 'C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.9\bin'
```

## License

BSD 3-Clause. See [LICENSE.txt](https://github.com/brendancol/otk-pyoptix/blob/master/LICENSE.txt).

## Acknowledgments

Original work by Keith Morley and NVIDIA Corporation ([NVIDIA/otk-pyoptix](https://github.com/NVIDIA/otk-pyoptix)).
