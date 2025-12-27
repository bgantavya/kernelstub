# kernelstub

The automatic manager for using the Linux Kernel EFI Stub to boot.

## Table of Contents

* [Description](#description)
* [How to Use](#how-to-use)
* [Technologies Used](#technologies-used)
* [Code Overview](#code-overview)
* [Known Issues / Improvements](#known-issues--improvements)
* [Additional Notes](#additional-notes)

## Description

`kernelstub` is a Python-based utility designed to simplify the process of managing the Linux Kernel EFI Stub for booting. It automates the setup and configuration of boot entries within the EFI System Partition (ESP), ensuring your Linux kernel can be booted directly by the UEFI firmware. The tool handles copying kernel and initrd images, configuring `loader.conf`, and updating NVRAM boot entries.

## How to Use

This script is intended to be run with root privileges to access and modify files within the ESP and system configurations.

### Installation

The installation process would typically involve placing the `installer.py` script in an appropriate location and ensuring its dependencies are met (standard Python libraries are used).

### Usage

The `installer.py` script is designed to be part of a larger system, likely invoked by a package manager or a higher-level tool. The core functionality is encapsulated within the `Installer` class, which requires instances of `NVRAM`, `OSInfo`, and `Drive` to be initialized.

```python
# Example of how the Installer class might be used (conceptual)

import logging
# Assume NVRAM, OSInfo, and Drive classes are defined elsewhere
# from nvram import NVRAM
# from os_info import OSInfo
# from drive import Drive

# Configure logging
logging.basicConfig(level=logging.DEBUG)

# Placeholder for necessary objects
class MockNVRAM:
    def __init__(self):
        self.os_entry_index = -1
        self.nvram = []
    def update(self): pass
    def delete_boot_entry(self, order_num, simulate): pass
    def add_entry(self, opsys, drive, kernel_opts, simulate): pass

class MockOSInfo:
    def __init__(self):
        self.name = "linux"
        self.kernel_name = "vmlinuz"
        self.initrd_name = "initrd.img"
        self.old_kernel_path = "/boot/vmlinuz-old"
        self.old_initrd_path = "/boot/initrd.img-old"
        self.kernel_path = "/boot/vmlinuz"
        self.name_pretty = "My Linux"

class MockDrive:
    def __init__(self):
        self.esp_path = "/boot/efi" # Example ESP path
        self.root_uuid = "some-uuid"

nvram_obj = MockNVRAM()
opsys_obj = MockOSInfo()
drive_obj = MockDrive()

installer = Installer(nvram_obj, opsys_obj, drive_obj)

# To copy kernel and initrd and setup systemd-boot loader
# installer.setup_kernel(kernel_opts="root=/dev/sda1 rw", setup_loader=True, overwrite=True)

# To set up EFISTUB loader and update NVRAM
# installer.setup_stub(kernel_opts="root=/dev/sda1 rw")

# To backup the old kernel and initrd
# installer.backup_old(kernel_opts="root=/dev/sda1 rw", setup_loader=True)
```

### Key Methods:

*   `backup_old(kernel_opts, setup_loader=False, simulate=False)`: Backs up the previous kernel and initrd images and optionally creates a loader entry for them.
*   `setup_kernel(kernel_opts, setup_loader=False, overwrite=False, simulate=False)`: Copies the current kernel and initrd to the ESP and optionally configures `systemd-boot` entries.
*   `setup_stub(kernel_opts, simulate=False)`: Configures the EFISTUB loader by copying `/proc/cmdline` and updating NVRAM boot entries.
*   `copy_cmdline(simulate)`: Copies the current kernel command line to the OS directory on the ESP.
*   `make_loader_entry(...)`: Creates a `.conf` file for `systemd-boot` entries.
*   `ensure_dir(directory, simulate=False)`: Ensures a directory exists, creating it if necessary.
*   `is_gzip(path)`: Checks if a file is gzipped.
*   `gunzip_files(src, dest, simulate)`: Decompresses a gzipped file.
*   `copy_files(src, dest, simulate)`: Copies a file from source to destination.

## Technologies Used

*   **Python 3**: The primary programming language.
*   **`os` module**: For interacting with the operating system, file paths, and directory operations.
*   **`shutil` module**: For high-level file operations like copying.
*   **`logging` module**: For outputting informational messages and debugging.
*   **`gzip` module**: For handling gzipped files.
*   **`pathlib` module**: For object-oriented filesystem path manipulations.

## Code Overview

The `Installer` class is the core component, responsible for all file operations and configuration management on the ESP.

*   **Initialization (`__init__`)**: Sets up paths based on the provided `Drive` object (ESP path), `OSInfo` (kernel/initrd names), and `NVRAM` object. It ensures the necessary `loader` and `entries` directories exist.
*   **File Operations**: Methods like `copy_files`, `gunzip_files`, and `ensure_dir` handle the low-level tasks of moving and creating files/directories. They include `simulate` functionality for dry runs.
*   **Backup (`backup_old`)**: Manages the process of saving the previous kernel and initrd, naming them with a `-previous` suffix.
*   **Kernel Setup (`setup_kernel`)**: Copies the active kernel and initrd to a dedicated directory within the ESP (e.g., `/boot/efi/EFI/linux-uuid`). It can also configure `systemd-boot` entries if `setup_loader` is `True`.
*   **EFISTUB Setup (`setup_stub`)**: This method focuses on setting up the kernel as a direct EFI boot entry. It copies `/proc/cmdline` to the ESP, removes any old boot entries, and adds a new entry via the `NVRAM` object.
*   **Loader Entry (`make_loader_entry`)**: A helper to create configuration files for `systemd-boot` (found in `/boot/efi/loader/entries/`).

## Known Issues / Improvements

*   **Error Handling**: While basic error handling is present, more granular error reporting and recovery mechanisms could be implemented.
*   **Dependency Management**: The script relies on standard Python libraries, but external dependencies for NVRAM manipulation or specific bootloader interactions might be necessary in a full implementation.
*   **Platform Specificity**: The paths and methods used (e.g., `/proc/cmdline`, EFI paths) are specific to Linux systems with UEFI.
*   **Simulation Mode**: The `simulate` flag is a good start, but a more comprehensive dry-run mode that details all intended changes without executing them would be beneficial.
*   **`systemd-boot` vs. GRUB/other**: The current implementation seems geared towards `systemd-boot` for loader entries. Integration with other bootloaders would require separate logic.

## Additional Notes

*   **Copyright**: The code is Copyright 2017-2018 Ian Santopietro. See the included `LICENSE.txt` file for terms.
*   **Purpose**: This script is a component of a system designed to automate the setup and management of Linux kernel booting via UEFI.
*   **Permissions**: Requires root privileges to perform operations on the ESP and system configuration files.