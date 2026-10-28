# kyty-ps5-
Vulkan optimization and hardware acceleration module for emulation kyty ps5
# KytyVulkan: Single-File Vulkan Engine Core for Console Emulation

A high-performance, production-grade, single-file Vulkan backend utility designed specifically for low-level console translation layers (such as the Kyty PS5 emulator environment). 

This header-only library focuses on solving critical graphics bottlenecks: memory sub-allocation, multi-threaded synchronization, host-non-coherent cache flushing, and aggressive shader stutter reduction.

## ⚡ Core Features

* **Advanced Memory Sub-Allocation:** Carves up large `VkDeviceMemory` blocks using a Best-Fit algorithm to prevent reaching `maxMemoryAllocationCount` limits under heavy console workloads.
* **Smart GPU Selection (`PickPhysicalDevice`):** Automatically evaluates and scores system GPUs based on device type (Discrete vs Integrated), VRAM size, ReBAR availability, and dedicated async queue support.
* **Zero-Allocation Submissions:** Batched timeline semaphore command submissions utilizing stack memory allocations to entirely eliminate runtime heap stutter.
* **Robust Pipeline Caching:** On-disk pipeline cache system utilizing safe `.tmp` writing and atomic renames to guarantee shader data persistence without file corruption.
* **Console-Grade Synchronization:** Built entirely around Vulkan 1.2+ Timeline Semaphores to cleanly match modern console hardware sync behaviors.
* **Modern C++20 RAII Architecture:** Automatic lifetime management via move-only `DeviceHandle` wrappers preventing memory and resource leaks.

## 🛠 Integration

Simply drop the `KytyVulkan.h` file into your graphics source directory. No extra dependencies or complex build configurations required.

```cpp
#include "KytyVulkan.h"

// 1. Pick the best GPU matching hardware requirements
KytyVulkan::GpuRequirements reqs;
reqs.requireTimelineSemaphore = true;
reqs.requireMemoryBudget = true;

auto pickedGpu = KytyVulkan::PickPhysicalDevice(instance, reqs);
if (!pickedGpu) {
    return; // No suitable GPU found
}

// 2. Initialize the sub-allocator
KytyVulkan::Allocator allocator(pickedGpu->gpu, device, pickedGpu->support.memoryBudget);
```

## 👤 Author & Support

* **Architected by:** A 14-year-old tech and graphics enthusiast from Poland 🇵🇱.
* **Core Engineering Support:** Powered by architectural code synthesis using Claude AI (Anthropic).

## 📜 License

This project is licensed under the MIT License. Feel free to use this layout in your own emulator or rendering setups!
