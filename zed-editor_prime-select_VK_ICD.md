# Test Plan: GPU Rendering Behavior with `prime-select` and `VK_ICD_FILENAMES`

## Objective
To verify the behavior of **vkcube** and **Zed** under different `prime-select` modes and `VK_ICD_FILENAMES` settings, ensuring proper GPU selection and rendering.

---

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

---

## Test Scenarios

### Scenario 1: `prime-select intel`
#### Test Case 1.1: `VK_ICD_FILENAMES=intel_icd.x86_64.json`
- **Steps**:
  1. Set `prime-select` to `intel`.
  2. Run `vkcube` and `Zed` with `VK_ICD_FILENAMES=intel_icd.x86_64.json`.
- **Expected Results**:
  - `vkcube`: Uses Intel GPU.
  - `Zed`: Uses Intel GPU.
- **Actual Results**:
  - `vkcube`: ✔️ Works. Logs: `Selected GPU 0: Intel(R) UHD Graphics 630 (CFL GT2), type: 1`.
  - `Zed`: ✔️ Works. Logs: `Adapter: "Intel(R) UHD Graphics 630 (CFL GT2)"`.

#### Test Case 1.2: `VK_ICD_FILENAMES=nvidia_icd.json`
- **Steps**:
  1. Set `prime-select` to `intel`.
  2. Run `vkcube` and `Zed` with `VK_ICD_FILENAMES=nvidia_icd.json`.
- **Expected Results**:
  - `vkcube`: Uses NVIDIA GPU.
  - `Zed`: Uses NVIDIA GPU.
- **Actual Results**:
  - `vkcube`: ✔️ Works. Logs: `Selected GPU 2: NVIDIA GeForce GTX 1050 Ti, type: 2`.
  - `Zed`: ✔️ Works. Logs: `Adapter: "NVIDIA GeForce GTX 1050 Ti"`.

---

### Scenario 2: `prime-select nvidia` (Performance Mode)
#### Test Case 2.1: `VK_ICD_FILENAMES=intel_icd.x86_64.json`
- **Steps**:
  1. Set `prime-select` to `nvidia`.
  2. Run `vkcube` and `Zed` with `VK_ICD_FILENAMES=intel_icd.x86_64.json`.
- **Expected Results**:
  - `vkcube`: Black screen (Intel GPU disabled).
  - `Zed`: Fails to render (Intel GPU disabled).
- **Actual Results**:
  - `vkcube`: ⚫ Black screen.
  - `Zed`: ❌ Fails. Logs: No rendering (Intel GPU disabled).

#### Test Case 2.2: `VK_ICD_FILENAMES=nvidia_icd.json`
- **Steps**:
  1. Set `prime-select` to `nvidia`.
  2. Run `vkcube` and `Zed` with `VK_ICD_FILENAMES=nvidia_icd.json`.
- **Expected Results**:
  - `vkcube`: Uses NVIDIA GPU.
  - `Zed`: Uses NVIDIA GPU.
- **Actual Results**:
  - `vkcube`: ✔️ Works. Logs: `Selected GPU 2: NVIDIA GeForce GTX 1050 Ti, type: 2`.
  - `Zed`: ✔️ Works. Logs: `Adapter: "NVIDIA GeForce GTX 1050 Ti"`. Crashes on suspend/resume.

---

### Scenario 3: `prime-select on-demand`
#### Test Case 3.1: `VK_ICD_FILENAMES=intel_icd.x86_64.json`
- **Steps**:
  1. Set `prime-select` to `on-demand`.
  2. Run `vkcube` and `Zed` with `VK_ICD_FILENAMES=intel_icd.x86_64.json`.
- **Expected Results**:
  - `vkcube`: Uses Intel GPU.
  - `Zed`: Uses Intel GPU.
- **Actual Results**:
  - `vkcube`: ✔️ Works. Logs: `Selected GPU 0: Intel(R) UHD Graphics 630 (CFL GT2), type: 1`.
  - `Zed`: **TBD** (Likely ✔️ Works if Intel GPU is accessible).

#### Test Case 3.2: `VK_ICD_FILENAMES=nvidia_icd.json`
- **Steps**:
  1. Set `prime-select` to `on-demand`.
  2. Run `vkcube` and `Zed` with `VK_ICD_FILENAMES=nvidia_icd.json`.
- **Expected Results**:
  - `vkcube`: Uses NVIDIA GPU.
  - `Zed`: Uses NVIDIA GPU.
- **Actual Results**:
  - `vkcube`: ✔️ Works. Logs: `Selected GPU 2: NVIDIA GeForce GTX 1050 Ti, type: 2`.
  - `Zed`: **TBD** (Likely ✔️ Works if NVIDIA GPU is accessible).

---

## Test Execution
- **Tester**: [Your Name]
- **Date**: [Test Execution Date]
- **Notes**:
  - Ensure `prime-select` and `VK_ICD_FILENAMES` are correctly set before each test.
  - Verify logs for GPU selection and rendering behavior.

---

## Conclusion
- Summarize the findings and any deviations from expected results.
- Provide recommendations for fixes or improvements.

---
