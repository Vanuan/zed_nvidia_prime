# Test Plan: GPU Rendering Behavior with `prime-select` and `VK_ICD_FILENAMES`

## Objective
To verify the behavior of **vkcube** and **Zed** under different `prime-select` modes and `VK_ICD_FILENAMES` settings, ensuring proper GPU selection and rendering.

## Test Environment
- **OS**: Ubuntu 22.04 LTS
- **GPU**: 
  - Integrated: Intel(R) UHD Graphics 630 (CFL GT2)
  - Discrete: NVIDIA GeForce GTX 1050 Ti
- **Tools**:
  - `vkcube`: Vulkan test application
  - `Zed`: Text editor with GPU rendering
- **Settings**:
  - `prime-select`: `intel`, `nvidia`, `on-demand`
  - `VK_ICD_FILENAMES`: `intel_icd.x86_64.json`, `nvidia_icd.json`

## Directory Structure
Store logs and screenshots in the following directory structure for easy reference:

```
test_results/
├── prime_select_intel/
│   ├── vkcube_intel_icd/
│   │   ├── logs.txt
│   │   ├── screenshot.png
│   ├── vkcube_nvidia_icd/
│   │   ├── logs.txt
│   │   ├── screenshot.png
│   ├── zed_intel_icd/
│   │   ├── logs.txt
│   │   ├── screenshot.png
│   ├── zed_nvidia_icd/
│   │   ├── logs.txt
│   │   ├── screenshot.png
├── prime_select_nvidia/
│   ├── vkcube_intel_icd/
│   │   ├── logs.txt
│   │   ├── screenshot.png
│   ├── vkcube_nvidia_icd/
│   │   ├── logs.txt
│   │   ├── screenshot.png
│   ├── zed_intel_icd/
│   │   ├── logs.txt
│   │   ├── screenshot.png
│   ├── zed_nvidia_icd/
│   │   ├── logs.txt
│   │   ├── screenshot.png
├── prime_select_on_demand/
│   ├── vkcube_intel_icd/
│   │   ├── logs.txt
│   │   ├── screenshot.png
│   ├── vkcube_nvidia_icd/
│   │   ├── logs.txt
│   │   ├── screenshot.png
│   ├── zed_intel_icd/
│   │   ├── logs.txt
│   │   ├── screenshot.png
│   ├── zed_nvidia_icd/
│   │   ├── logs.txt
│   │   ├── screenshot.png
```

## Test Scenarios

### Scenario 1: `prime-select intel`
#### Test Case 1.1: `VK_ICD_FILENAMES=intel_icd.x86_64.json`
- **Steps**:
  1. Set `prime-select` to `intel`.
  2. Restart the system.
  3. Run `vkcube` and `Zed` with `VK_ICD_FILENAMES=intel_icd.x86_64.json`.
- **Expected Results**:
  - `vkcube`: Uses Intel GPU.
  - `Zed`: Uses Intel GPU.
- **Actual Results**:
  - `vkcube`: ✔️ Works. Logs: `Selected GPU 0: Intel(R) UHD Graphics 630 (CFL GT2), type: 1`.
  - `Zed`: ✔️ Works. Logs: `Adapter: "Intel(R) UHD Graphics 630 (CFL GT2)"`.
- **Artifacts**:
  - Logs: `test_results/prime_select_intel/vkcube_intel_icd/logs.txt`
  - Screenshot: `test_results/prime_select_intel/vkcube_intel_icd/screenshot.png`
  - Logs: `test_results/prime_select_intel/zed_intel_icd/logs.txt`
  - Screenshot: `test_results/prime_select_intel/zed_intel_icd/screenshot.png`

- **Suspension**:
  - suspends/resumes without issues

#### Test Case 1.2: `VK_ICD_FILENAMES=nvidia_icd.json`
- **Steps**:
  1. Set `prime-select` to `intel`.
  2. Restart the system.
  3. Run `vkcube` and `Zed` with `VK_ICD_FILENAMES=nvidia_icd.json`.
- **Expected Results**:
  - `vkcube`: Uses NVIDIA GPU.
  - `Zed`: Uses NVIDIA GPU.
- **Actual Results**:
  - `vkcube`: ✔️ Works. Logs: `Selected GPU 2: NVIDIA GeForce GTX 1050 Ti, type: 2`.
  - `Zed`: ✔️ Works. Logs: `Adapter: "NVIDIA GeForce GTX 1050 Ti"`.
- **Artifacts**:
  - Logs: `test_results/prime_select_intel/vkcube_nvidia_icd/logs.txt`
  - Screenshot: `test_results/prime_select_intel/vkcube_nvidia_icd/screenshot.png`
  - Logs: `test_results/prime_select_intel/zed_nvidia_icd/logs.txt`
  - Screenshot: `test_results/prime_select_intel/zed_nvidia_icd/screenshot.png`
  - Screenshot on suspend/resume: `test_results/prime_select_intel/zed_nvidia_icd/screenshot2.png`

- **Suspension**:
  - Suspend/resume result in xdg-desktop-portal-gtk crash and rendering artifacts (see screenshot)


### Scenario 2: `prime-select nvidia` (Performance Mode)
#### Test Case 2.1: `VK_ICD_FILENAMES=intel_icd.x86_64.json`
- **Steps**:
  1. Set `prime-select` to `nvidia`.
  2. Restart the system.
  3. Run `vkcube` and `Zed` with `VK_ICD_FILENAMES=intel_icd.x86_64.json`.
- **Expected Results**:
  - `vkcube`: Black screen (Intel GPU disabled).
  - `Zed`: Fails to render (Intel GPU disabled).
- **Actual Results**:
  - `vkcube`: ⚫ Black screen.
  - `Zed`: ❌ Fails. Logs: No rendering (Intel GPU disabled).
- **Artifacts**:
  - Logs: `test_results/prime_select_nvidia/vkcube_intel_icd/logs.txt`
  - Screenshot: `test_results/prime_select_nvidia/vkcube_intel_icd/screenshot.png`
  - Logs: `test_results/prime_select_nvidia/zed_intel_icd/logs.txt`
  - Screenshot: `test_results/prime_select_nvidia/zed_intel_icd/screenshot.png`

#### Test Case 2.2: `VK_ICD_FILENAMES=nvidia_icd.json`
- **Steps**:
  1. Set `prime-select` to `nvidia`.
  2. Restart the system.
  3. Run `vkcube` and `Zed` with `VK_ICD_FILENAMES=nvidia_icd.json`.
- **Expected Results**:
  - `vkcube`: Uses NVIDIA GPU.
  - `Zed`: Uses NVIDIA GPU.
- **Actual Results**:
  - `vkcube`: ✔️ Works. Logs: `Selected GPU 2: NVIDIA GeForce GTX 1050 Ti, type: 2`.
  - `Zed`: ✔️ Works. Logs: `Adapter: "NVIDIA GeForce GTX 1050 Ti"`. Crashes on suspend/resume.
- **Artifacts**:
  - Logs: `test_results/prime_select_nvidia/vkcube_nvidia_icd/logs.txt`
  - Screenshot: `test_results/prime_select_nvidia/vkcube_nvidia_icd/screenshot.png`
  - Logs: `test_results/prime_select_nvidia/zed_nvidia_icd/logs.txt`
  - Screenshot: `test_results/prime_select_nvidia/zed_nvidia_icd/screenshot.png`

### Scenario 3: `prime-select on-demand`
#### Test Case 3.1: `VK_ICD_FILENAMES=intel_icd.x86_64.json`
- **Steps**:
  1. Set `prime-select` to `on-demand`.
  2. Restart the system.
  3. Run `vkcube` and `Zed` with `VK_ICD_FILENAMES=intel_icd.x86_64.json`.
- **Expected Results**:
  - `vkcube`: Uses Intel GPU.
  - `Zed`: Uses Intel GPU.
- **Actual Results**:
  - `vkcube`: ✔️ Works. Logs: `Selected GPU 0: Intel(R) UHD Graphics 630 (CFL GT2), type: 1`.
  - `Zed`: **TBD** (Likely ✔️ Works if Intel GPU is accessible).
- **Artifacts**:
  - Logs: `test_results/prime_select_on_demand/vkcube_intel_icd/logs.txt`
  - Screenshot: `test_results/prime_select_on_demand/vkcube_intel_icd/screenshot.png`
  - Logs: `test_results/prime_select_on_demand/zed_intel_icd/logs.txt`
  - Screenshot: `test_results/prime_select_on_demand/zed_intel_icd/screenshot.png`

- **Suspend/resume**:
  - Both Zed and vkcube work without issues

#### Test Case 3.2: `VK_ICD_FILENAMES=nvidia_icd.json`
- **Steps**:
  1. Set `prime-select` to `on-demand`.
  2. Restart the system.
  3. Run `vkcube` and `Zed` with `VK_ICD_FILENAMES=nvidia_icd.json`.
- **Expected Results**:
  - `vkcube`: Uses NVIDIA GPU.
  - `Zed`: Uses NVIDIA GPU.
- **Actual Results**:
  - `vkcube`: ✔️ Works. Logs: `Selected GPU 2: NVIDIA GeForce GTX 1050 Ti, type: 2`.
  - `Zed`: **TBD** (Likely ✔️ Works if NVIDIA GPU is accessible).
- **Artifacts**:
  - Logs: `test_results/prime_select_on_demand/vkcube_nvidia_icd/logs.txt`
  - Screenshot: `test_results/prime_select_on_demand/vkcube_nvidia_icd/screenshot.png`
  - Logs: `test_results/prime_select_on_demand/zed_nvidia_icd/logs.txt`
  - Screenshot: `test_results/prime_select_on_demand/zed_nvidia_icd/screenshot.png`

- **Suspend/resume**:
  - Zed crashes (see logs)
  - vkcube crashes

## Test Execution
- **Tester**: @vanuan
- **Date**: 30 Jan 2025
- **Zed version**: 0.172.2 - 0.172.5
- **Notes**:
  - Ensure `prime-select` and `VK_ICD_FILENAMES` are correctly set before each test.
  - Verify logs for GPU selection and rendering behavior.

## Conclusion
- Summarize the findings and any deviations from expected results.
- Provide recommendations for fixes or improvements.

