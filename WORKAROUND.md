### NVIDIA Optimus Rendering Issues

If you're using a system with NVIDIA Optimus (hybrid graphics with Intel integrated and NVIDIA discrete GPUs), you'll encounter different behaviors depending on your system's graphics mode:

#### Behavior by Mode

1. **Intel Mode** (`prime-select intel`)
   - ✔️ Works reliably with Intel GPU
   - ✔️ Stable suspend/resume with Intel GPU
   - ❌ Using NVIDIA GPU causes xdg-desktop-portal-gtk crashes and rendering artifacts after suspend

2. **NVIDIA Mode** (`prime-select nvidia`)
   - ✔️ Works with NVIDIA GPU
   - ❌ Intel GPU is completely disabled
   - ❌ Crashes on suspend/resume with "GPU hung" errors

3. **On-Demand Mode** (`prime-select on-demand`)
   - ✔️ Works reliably with Intel GPU
   - ❌ Using NVIDIA GPU leads to crashes, especially on suspend/resume
   - ❌ Most unstable mode when forcing NVIDIA graphics

#### Recommended Configuration

1. **On-Demand Mode** (Best Balance)
   ```sh
   sudo prime-select on-demand
   # Restart your system
   ```
   - Uses Intel GPU by default (stable)
   - Allows per-application GPU selection
   - Avoid forcing NVIDIA graphics until issues are resolved

2. **Intel Mode** (Most Stable)
   ```sh
   sudo prime-select intel
   # Restart your system
   ```

3. **NVIDIA Mode** (Performance with Caveats)
   ```sh
   sudo prime-select nvidia
   # Restart your system
   ```
   - Requires additional configuration to work (see below)
   - Be prepared for suspend/resume issues

#### Required NVIDIA Configuration

When using NVIDIA mode (`prime-select nvidia`), you must force the NVIDIA GPU using one of these methods:

1. **PRIME Render Offload** (Recommended)
   ```sh
   __NV_PRIME_RENDER_OFFLOAD=1 zed
   ```
   
   Or with full debugging options:
   ```sh
   __NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only zed
   ```

2. **Vulkan ICD Loader**
   ```sh
   VK_ICD_FILENAMES=/usr/share/vulkan/icd.d/nvidia_icd.json zed
   ```

Without one of these configurations, Zed may fail to start or render properly in NVIDIA mode.

#### Alternative GPU Selection Methods

1. **Using GNOME's GUI** (Easiest)
   - Right-click Zed in Applications menu
   - Select "Launch using Discrete Graphics Card"

2. **Using Vulkan ICD Loaders in Other Modes**
   ```sh
   # Force Intel GPU
   VK_ICD_FILENAMES=/usr/share/vulkan/icd.d/intel_icd.x86_64.json zed
   
   # Force NVIDIA GPU
   VK_ICD_FILENAMES=/usr/share/vulkan/icd.d/nvidia_icd.json zed
   ```

   Note: Exact paths may vary by distribution. Check your `/usr/share/vulkan/icd.d/` directory.


#### Technical Background

The core issues stem from:
- Vulkan device loss during suspend/resume operations
- Lack of runtime re-initialization support in Zed's graphics stack
- Different behavior patterns across PRIME modes

We're tracking these issues in [#22900](https://github.com/zed-industries/zed/issues/22900) and working on implementing proper device loss recovery ([#23288](https://github.com/zed-industries/zed/issues/23288)).

#### Known Limitations

- Suspend/resume operations are likely to cause issues when using NVIDIA GPU
- Forcing NVIDIA graphics may lead to system instability
- No current workaround for automatic GPU switching
- Recovery from GPU device loss requires restarting Zed

