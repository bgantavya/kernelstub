# zz-kernelstub

## Folder Structure

*   [zz-kernelstub](zz-kernelstub/)

## Description

This script is a simple wrapper around the `kernelstub` command-line utility, designed to manage kernel boot entries on certain systems (often associated with macOS bootloaders like Clover or OpenCore). It specifically preserves the live mode setting when updating the kernel stub.

## How to Use

### Prerequisites

*   `kernelstub` utility must be installed on your system.

### Usage

To use this script, execute it with the kernel version and the path to the initrd image as arguments.

```bash
./zz-kernelstub <kernel_version> <initrd_path>
```

**Example:**

If your kernel version is `5.15.0-56-generic` and your initrd is located at `/boot/initrd.img-5.15.0-56-generic`, you would run:

```bash
./zz-kernelstub 5.15.0-56-generic /boot/initrd.img-5.15.0-56-generic
```

This command will configure `kernelstub` to use the specified kernel and initrd, while ensuring that the live mode setting is not altered.

## Technologies Used

*   Bash Scripting

## Architecture or Code Overview

The script is a straightforward Bash script that accepts two arguments:

1.  `$1`: The kernel version string, used to construct the path to the kernel image (e.g., `/boot/vmlinuz-$1`).
2.  `$2`: The path to the initrd image.

It then calls the `kernelstub` command with the following options:

*   `--verbose`: Enables verbose output from `kernelstub`, providing more details about the operation.
*   `--preserve-live-mode`: Instructs `kernelstub` to keep the existing live mode setting for the boot entry.

## Known Issues / Improvements

*   **Error Handling**: The script lacks robust error handling. It doesn't check if the provided arguments are valid or if the `kernelstub` command executed successfully.
*   **Input Validation**: No validation is performed on the input arguments to ensure they are in the expected format or that the specified files exist.
*   **Platform Specificity**: `kernelstub` is typically used on specific platforms. This script assumes the user is on such a platform and has `kernelstub` installed.

## Additional Notes or References

*   **kernelstub Documentation**: For more details on the `kernelstub` utility, refer to its official documentation or man pages.
*   **License**: No license information provided.
*   **Authors**: Anonymous