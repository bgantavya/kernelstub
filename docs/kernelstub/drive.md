# kernelstub

The automatic manager for using the Linux Kernel EFI Stub to boot.

## Folder Structure

* [drive.py](./drive.py)

## Description

The `kernelstub` project is a Python-based tool designed to automate the management of the Linux Kernel EFI Stub. It helps in configuring the EFI bootloader settings, specifically focusing on the relationship between the root filesystem and the EFI System Partition (ESP). The tool aims to simplify the process of setting up or updating boot configurations for systems using EFI.

## How to Use

This script is intended to be run as part of a larger system or boot process. It defines a `Drive` class that identifies and retrieves information about the root filesystem and the EFI System Partition.

### Initialization

The `Drive` class is initialized by providing the paths to the root filesystem and the ESP.

```python
import kernelstub.drive

# Initialize the Drive object
# Assumes root is at '/' and ESP is at '/boot/efi'
drive_manager = kernelstub.drive.Drive(root_path="/", esp_path="/boot/efi")

# Accessing drive information
print(f"Drive Name: {drive_manager.drive_name}")
print(f"Root FS: {drive_manager.root_fs}")
print(f"Root UUID: {drive_manager.root_uuid}")
print(f"ESP FS: {drive_manager.esp_fs}")
print(f"ESP Partition Number: {drive_manager.esp_num}")
```

### Core Functionality

The `Drive` class includes methods to:

*   `get_drives()`: Reads `/proc/mounts` to get a list of mounted filesystems.
*   `get_part_dev(path)`: Determines the block device file (e.g., `/dev/sda1`) for a given mount path.
*   `get_drive_dev(esp)`: Identifies the main disk device (e.g., `sda`) on which the ESP resides.
*   `get_uuid(path)`: Retrieves the UUID of the filesystem mounted at a given path using `findmnt`.

## Technologies Used

*   **Python 3**: The primary programming language.
*   **`os` module**: For interacting with the operating system, like path manipulation and symbolic link resolution.
*   **`logging` module**: For internal logging of the script's operations.
*   **`subprocess` module**: To execute external commands like `findmnt`.
*   **/proc/mounts**: A virtual file system in Linux that provides information about the currently mounted filesystems.
*   **`findmnt`**: A command-line utility to find mounted filesystems.

## Architecture or Code Overview

The `Drive` class is the central component. Its `__init__` method is responsible for gathering crucial information about the system's boot setup upon instantiation.

*   **`__init__(self, root_path="/", esp_path="/boot/efi")`**:
    *   Initializes logging.
    *   Sets the paths for the root filesystem and ESP.
    *   Calls `get_drives()` to populate `self.mtab`.
    *   Uses `get_part_dev()` to find the block devices for `root_path` and `esp_path`.
    *   Uses `get_drive_dev()` to determine the main disk for the ESP.
    *   Retrieves the UUID of the root filesystem using `get_uuid()`.
    *   Handles `NoBlockDevError` and `UUIDNotFoundError` by logging exceptions and exiting.

*   **Error Handling**: Custom exceptions `NoBlockDevError` and `UUIDNotFoundError` are defined to signify specific failure conditions related to finding block devices or UUIDs.

## Known Issues / Improvements

*   **Error Handling**: The script currently exits on critical errors. A more robust implementation might raise exceptions that can be handled by a calling script.
*   **External Dependencies**: Relies on the `findmnt` utility being available.
*   **Portability**: While designed for Linux, specific paths like `/proc/mounts` and the structure of `/sys/class/block/` are Linux-specific.
*   **Configuration Flexibility**: Could benefit from more configuration options beyond just `root_path` and `esp_path`.

## Additional Notes or References

*   **License**: The software is provided "as is" without warranty. See the included `LICENSE.txt` for full details.
*   **Copyright**: Copyright © 2017-2018 Ian Santopietro <isantop@gmail.com>.
*   **Inspiration**: Code and concepts appear to be inspired by firmware updater tools, as noted in the `get_drive_dev` method's comments.