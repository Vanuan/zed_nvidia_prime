# Zed GPU Behavior Testing Results

This repository documents testing results for Zed editor and vkcube behavior across different GPU configurations on Linux systems with hybrid graphics.

## Test results summary

| `prime-select` Mode | `VK_ICD_FILENAMES` Setting | vkcube Behavior | Zed Behavior | Suspend/Resume |
|---------------------|---------------------------|-----------------|--------------|----------------|
| **`intel`** | `intel_icd.x86_64.json` | ✔️ Works: Uses Intel GPU.<br><details><summary>Logs</summary>`Selected GPU 0: Intel(R) UHD Graphics 630 (CFL GT2), type: 1`</details> | ✔️ Works: Uses Intel GPU.<br><details><summary>Logs</summary>`Adapter: "Intel(R) UHD Graphics 630 (CFL GT2)"`</details> | ✔️ Works without issues |
| **`intel`** | `nvidia_icd.json` | ✔️ Works: Uses NVIDIA GPU.<br><details><summary>Logs</summary>`Selected GPU 0: NVIDIA GeForce GTX 1050 Ti, type: 2`</details> | ✔️ Works: Uses NVIDIA GPU.<br><details><summary>Logs</summary>`Adapter: "NVIDIA GeForce GTX 1050 Ti"`</details> | ❌ xdg-desktop-portal-gtk crash and rendering artifacts |
| **`nvidia`** | `intel_icd.x86_64.json` | ⚫ Black screen (Intel GPU disabled) | ❌ No rendering (Intel GPU disabled) | N/A |
| **`nvidia`** | `nvidia_icd.json` | ✔️ Works: Uses NVIDIA GPU.<br><details><summary>Logs</summary>`Selected GPU 2: NVIDIA GeForce GTX 1050 Ti, type: 2`</details> | ✔️ Works: Uses NVIDIA GPU.<br><details><summary>Logs</summary>`Adapter: "NVIDIA GeForce GTX 1050 Ti"`</details> | ❌ Crashes on suspend/resume <details><summary>Logs</summary>GPU hung</details> |
| **`on-demand`** | `intel_icd.x86_64.json` | ✔️ Works: Uses Intel GPU.<br><details><summary>Logs</summary>`Selected GPU 0: Intel(R) UHD Graphics 630 (CFL GT2), type: 1`</details> | ✔️ Works: Uses Intel GPU.<br><details><summary>Logs</summary>`Adapter: "Intel(R) UHD Graphics 630 (CFL GT2)"`</details> | ✔️ Works without issues |
| **`on-demand`** | `nvidia_icd.json` | ✔️ Works: Uses NVIDIA GPU.<br><details><summary>Logs</summary>`Selected GPU 0: NVIDIA GeForce GTX 1050 Ti with Max-Q Design, type: 2`</details> | ✔️ Works: Uses NVIDIA GPU.<br><details><summary>Logs</summary>`GPU has crashed, and no debug information is available.`</details> | ❌ Crashes on suspend/resume ❌ Crashes.<br><details><summary>Logs</summary>`Selected GPU 0: NVIDIA GeForce GTX 1050 Ti with Max-Q Design, type: 2`<br>`vkcube: ./cube/cube.c:1080: demo_draw: Assertion '!err' failed.`</details><details><summary>Logs</summary>`GPU has crashed, and no debug information is available.`</details> |

## Key findings

1. **Intel GPU Reliability**:
   - Works reliably across all modes where it's available
   - Best suspend/resume behavior
   - Recommended for stability

2. **NVIDIA GPU Issues**:
   - Suspend/resume problems in all modes
   - Crashes in on-demand mode
   - Desktop portal crashes when using NVIDIA in Intel mode

3. **Mode-specific Behavior**:
   - Intel mode: Most stable with Intel GPU
   - NVIDIA mode: Works but with suspend/resume issues
   - On-demand mode: Unstable with NVIDIA GPU

## Recommendations

1. For stability: Use Intel mode with Intel GPU
2. For performance with caveats: Use NVIDIA mode with NVIDIA GPU
3. Avoid on-demand mode with NVIDIA GPU until issues are resolved

## Related issues

### Suspend/Resume related
- [Zed is sometimes unresponsive when macOS awakes from sleep #7940](https://github.com/zed-industries/zed/issues/7940) - oldest issue
- [Crash when screen is locked and monitor goes into power-saving mode #14022](https://github.com/zed-industries/zed/issues/14022)
- [Linux: Zed causes brief system freezes when opening new windows #22320](https://github.com/zed-industries/zed/issues/22320)
- [Zed hung after Ubuntu 24.04 hibernate reawakens #18478](https://github.com/zed-industries/zed/issues/18478)

### General cashes and freezed
- [Don't panic on GPU hang #14974](https://github.com/zed-industries/zed/pull/14974)
- [Crash when moving window in Xorg + Awesome WM #12766](https://github.com/zed-industries/zed/issues/12766)
- [Zed freezing my enviroment #14519](https://github.com/zed-industries/zed/issues/14519)

### Solution discussions

- [docs: Update linux.md to include NVIDIA PRIME workaround #23438](https://github.com/zed-industries/zed/pull/23438) - old PR to document workarounds (closed)
- [docs: Update linux.md to include NVIDIA hybrid graphics limitations #24343](https://github.com/zed-industries/zed/pull/24343) - current PR to document workarounds
- [Implement graceful recovery for GPU device loss in Zed #23288](https://github.com/zed-industries/zed/issues/23288)
- [Zed doesn't work with NVIDIA Optimus on Linux out of the box #22900](https://github.com/zed-industries/zed/issues/22900)
- [Feature request: Introduce SyncStatus and InvalidSyncPoint for enhanced synchronization feedback #248](https://github.com/kvark/blade/issues/248) - Blade error handling improvement


## Test environment

- **OS**: Ubuntu 22.04 LTS
- **GPUs**:
  - Intel(R) UHD Graphics 630 (CFL GT2)
  - NVIDIA GeForce GTX 1050 Ti
- **Zed Version**: 0.172.2 - 0.172.5
- **Test Date**: January 30, 2025

## Further details

For complete test logs and screenshots, see the [test_results](./test_results) directory.

For the full test plan and methodology, see [zed-editor_prime-select_VK_ICD.md](./zed-editor_prime-select_VK_ICD.md).

For the proposed solution, see [SOLUTION.md](./SOLUTION.md).
