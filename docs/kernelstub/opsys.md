# KernelStub

The automatic manager for using the Linux Kernel EFI Stub to boot.

## Folder Structure

- `opsys.py`

## Description

This project, `kernelstub`, is a Python script designed to manage the Linux Kernel EFI Stub. It automates the process of booting the Linux kernel using the EFI stub, a feature that allows the kernel to be booted directly by the EFI firmware without the need for a separate bootloader.

The `opsys.py` file specifically defines an `OS` class that abstracts operating system details relevant to kernel booting on Linux. It retrieves information such as the OS name, version, kernel command line arguments, and kernel/initrd filenames from the system.

## How to Use

This script is intended to be run within a Linux environment. The `OS` class is likely a component of a larger system that utilizes this information to configure EFI boot entries.

### Example Usage (Illustrative - depends on the larger system)

```python
from opsys import OS

# Instantiate the OS class to get system information
os_info = OS()

# Accessing OS details
print(f"Pretty OS Name: {os_info.name_pretty}")
print(f"Technical OS Name: {os_info.name}")
print(f"OS Version: {os_info.version}")
print(f"Kernel Command Line: {os_info.cmdline}")
print(f"Kernel Name: {os_info.kernel_name}")
print(f"Initrd Name: {os_info.initrd_name}")
```

## Technologies Used

*   **Python 3**: The primary programming language.
*   `/proc/cmdline`: Linux file used to read kernel command line arguments.
*   `/etc/os-release`: Linux file used to read OS identification data.
*   `platform` module: Python standard library for accessing underlying platform’s identifying data.

## Architecture or Code Overview

The core of this file is the `OS` class:

*   **`__init__(self)`**: The constructor initializes OS-specific attributes by calling helper methods to fetch data from the system.
*   **`clean_names(self, name)`**: This method sanitizes OS names by removing characters that are not suitable for technical identifiers (e.g., spaces, special symbols).
*   **`get_os_cmdline(self)`**: Reads `/proc/cmdline` and parses the kernel boot parameters, excluding boot image, root, and initrd paths.
*   **`get_os_name(self)`**: Reads `/etc/os-release` to extract the OS name.
*   **`get_os_version(self)`**: Reads `/etc/os-release` to extract the OS version.
*   **`strip_quotes(self, value)`**: A utility function to remove leading/trailing quotes from strings.
*   **`get_os_release(self)`**: Reads `/etc/os-release` and returns its lines. It includes a fallback mechanism if `/etc/os-release` is not found.

## Known Issues / Improvements

*   The `clean_names` method's list of `badchar` seems comprehensive but might need adjustments for very niche OS distributions or naming conventions.
*   The fallback for `/etc/os-release` assumes a generic Linux system; more specific fallbacks could be implemented if needed.
*   Error handling for file operations is basic (`try-except FileNotFoundError`). More robust error handling could be added.

## Additional Notes or References

*   **License**: This software is provided "as is" and the author disclaims all warranties with regard to this software including all implied warranties of MERCHANTABILITY AND FITNESS. See the `LICENSE.txt` file for full licensing terms.
*   **Copyright**: Copyright 2017-2018 Ian Santopietro `<isantop@gmail.com>`