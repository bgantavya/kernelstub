# Kernel Stub Configuration Helper

## Folder Structure

```
.
└── zz-kernelstub
```

## Description

This project provides a simple shell script, `zz-kernelstub`, which acts as a convenient wrapper for the `kernelstub` utility. It simplifies the process of updating kernel entries in bootloaders (such as `systemd-boot`) by automating the provision of common arguments and handling the initial RAM disk (initrd) and kernel path inputs. The script is designed to streamline kernel updates, ensuring verbose output and preserving live mode settings.

## How to Use

To use this script, you need to have the `kernelstub` utility installed on your system. `kernelstub` is typically part of `systemd-boot` or related bootloader management packages.

### Prerequisites

*   `bash`
*   `kernelstub` utility (e.g., from `systemd-boot` or `pop-os-tools`)

### Usage

The script requires two positional arguments: the path to the initial RAM disk image and the path to the kernel image.

```bash
./zz-kernelstub <INITRD_PATH> <KERNEL_PATH>
```

**Example:**

```bash
./zz-kernelstub /boot/initrd.img-6.5.0-28-generic /boot/vmlinuz-6.5.0-28-generic
```

This command will execute `kernelstub` with the provided paths, along with `--verbose` and `--preserve-live-mode` flags.

## Technologies Used

*   **Shell Scripting:** `bash`
*   **Utility:** `kernelstub` (an external tool for bootloader entry management)

## Architecture or Code Overview

The `zz-kernelstub` script is a straightforward, sequential shell script with the following structure:

1.  **Variable Assignment:**
    *   It assigns the first command-line argument (`$1`) to the `INITRD` variable.
    *   It assigns the second command-line argument (`$2`) to the `KERNEL` variable.

2.  **Kernelstub Execution:**
    *   It invokes the `kernelstub` command.
    *   It passes the `--verbose` flag to enable detailed output.
    *   It passes the `--preserve-live-mode` flag to maintain existing live mode configurations.
    *   It uses the `INITRD` and `KERNEL` variables to specify the paths for the initial RAM disk and the kernel, respectively.

The script's purpose is to abstract away the direct invocation of `kernelstub` with these common parameters, making kernel updates more consistent and less prone to manual errors.

## Known Issues / Improvements

### Known Issues

*   The script lacks argument validation. If fewer than two arguments are provided, `kernelstub` might be called with empty or incorrect paths, potentially leading to errors.
*   It assumes `kernelstub` is in the system's PATH.

### Improvements

*   Add robust argument parsing and validation (e.g., check if `$1` and `$2` are provided, check if paths exist).
*   Include a usage message if arguments are missing or incorrect.
*   Allow additional `kernelstub` flags to be passed through the script, making it more flexible.
*   Add error handling for the `kernelstub` command itself (e.g., check its exit status).
*   Verify the existence of the `kernelstub` executable before attempting to run it.

## Additional Notes or References

*   For more information on the `kernelstub` utility and its capabilities, refer to its man page (`man kernelstub`) or the documentation for your system's bootloader (e.g., `systemd-boot`).
*   This script is particularly useful in environments where `kernelstub` is used for managing boot entries, such as Pop!_OS.