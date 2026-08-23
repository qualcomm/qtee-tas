# Qualcomm TEE Trusted Applications

This repository is the central hub for hosting pre-built Qualcomm **Trusted Execution Environment (TEE) Trusted Applications (TAs)**. These TAs run inside Qualcomm's secure world (QTEE) and are distributed as test signed `.mbn` binaries, organised per chipset target. The repository currently ships TAs for the following platforms:

| Chipset Directory | Platform |
|---|---|
| `ta/glymur/` | Glymur |
| `ta/kaanapali/` | Kaanapali |
| `ta/mahua/` | Mahua |
| `ta/qcm2290/` | QCM2290 |
| `ta/qcm6490/` | QCM6490 |
| `ta/qcs615/` | QCS615 |
| `ta/qcs8300/` | QCS8300 |
| `ta/qcs9100/` | QCS9100 |
| `ta/shikra/` | Shikra |
| `ta/sm8750/` | SM8750 |
| `ta/x1e80100/` | X1E80100 |
| `ta/x1p42100/` | X1P42100 |

Each chipset directory contains one or more signed TA binaries (`.mbn` files) ready for deployment.

## Branches

| Branch | Purpose |
|---|---|
| **`main`** | Primary development branch. All contributions should be based on and submitted against this branch via pull request.

## Requirements

The TAs in this repository are pre-built binaries and do not require a build toolchain to deploy. To use them you will need:

- A Qualcomm platform running a Linux-based OS with QTEE support.
- Root / privileged access to copy files into the TA installation directory.
- `adb` (Android Debug Bridge) or equivalent file-transfer tool if deploying to an embedded target over USB.

No additional build dependencies are required for deployment.

## Installation Instructions

TAs are installed into `/lib/qtee-tas/<chipset>/` on the target device. Replace `<chipset>` with the directory name that matches your platform (e.g. `sm8750`).

**Steps:**

1. Identify your chipset and locate the corresponding directory under `ta/`:

   ```
   ta/<chipset>/
   ```

2. Copy the `.mbn` binary (or binaries) to the target device:

   ```bash
   # Example using adb
   adb push ta/<chipset>/* /lib/qtee-tas/<chipset>/
   ```

3. Set the correct permissions on the target:

   ```bash
   adb shell chmod 644 /lib/qtee-tas/<chipset>/*.mbn
   ```

4. Reboot or restart the relevant TEE service so the new TAs are picked up.

> **Note:** The [`NO.LOGIN.BINARY.LICENSE.QTI.pdf`](NO.LOGIN.BINARY.LICENSE.QTI.pdf) document in the repository root governs the use of these pre-built binaries. Please read it before deploying.

## Usage

Once the TAs are installed on the target device, they can be loaded and invoked by a corresponding **Client Application (CA)** running in the normal world via the QTEE client API (e.g. `libteec` / GP TEE Client API).

**Typical workflow:**

1. Open a session to the TA using its UUID (e.g. `FD02C9DA-306C-48C7-A49C-BBD827AE86EE`).
2. Send commands to the TA using `TEEC_InvokeCommand`.
3. Close the session when done.

The `tzecotestapp.mbn` TA is a sample/test application that can be used to verify that the TEE stack is functioning correctly on your platform. Refer to the Qualcomm TEE SDK documentation for the full client API reference and sample CA code.

> **Security note:** These TAs are signed by Qualcomm. Do not attempt to modify the `.mbn` binaries — doing so will invalidate the signature and the TEE will refuse to load them.

## Development

- Read [CONTRIBUTING.md](CONTRIBUTING.md) for the full contribution checklist and coding style expectations.
- Keep each commit focused on a single logical change.
- All pull requests must pass review before merging.

### Reporting Security Issues

Please **do not** open a public GitHub issue for security vulnerabilities. See [SECURITY.md](SECURITY.md) for the responsible-disclosure process.

## License

**Qualcomm TEE Trusted Applications** is licensed under the [BSD 3-Clause License](https://spdx.org/licenses/BSD-3-Clause.html). See [LICENSE.txt](LICENSE.txt) for the full license text.

The pre-built TA binaries (`.mbn` files) distributed under the `ta/` directory are additionally governed by the **Qualcomm No-Login Binary License** — see [NO.LOGIN.BINARY.LICENSE.QTI.pdf](NO.LOGIN.BINARY.LICENSE.QTI.pdf) for details.
