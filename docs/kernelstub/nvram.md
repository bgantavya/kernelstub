# kernelstub

The automatic manager for using the Linux Kernel EFI Stub to boot.

## Table of Contents

*   [Description](#description)
*   [How to Use](#how-to-use)
    *   [Installation](#installation)
    *   [Usage](#usage)
*   [Technologies Used](#technologies-used)
*   [Architecture / Code Overview](#architecture--code-overview)
*   [Known Issues / Improvements](#known-issues--improvements)
*   [Additional Notes / References](#additional-notes--references)

## Description

`kernelstub` is a Python-based utility designed to automate the process of managing boot entries for Linux kernels using the EFI System Partition (ESP) and `efibootmgr`. It simplifies the creation, deletion, and management of bootable kernel entries directly within the system's UEFI NVRAM.

## How to Use

### Installation

`kernelstub` is a Python script. To use it, you'll need Python 3 and the `efibootmgr` utility installed on your system. Ensure `efibootmgr` is available in your PATH.

1.  **Clone the repository (if applicable) or save the script:**
    ```bash
    git clone <repository_url>
    cd kernelstub_directory
    ```
    Or simply save the `nvram.py` file to a desired location.

2.  **Make the script executable (optional, but recommended):**
    ```bash
    chmod +x nvram.py
    ```

### Usage

The `nvram.py` script itself is a class that is likely intended to be imported and used by another script or application. However, a direct usage pattern can be inferred from its methods:

**Core Functionality (as inferred from the class methods):**

The `NVRAM` class interacts with `efibootmgr` to manage UEFI boot entries.

*   **Initialization:**
    The `NVRAM` class is initialized with the OS name and version. This is used to create a unique label for the boot entry.
    ```python
    from kernelstub.nvram import NVRAM
    import logging

    logging.basicConfig(level=logging.DEBUG) # For demonstration

    nvram_manager = NVRAM("MyOS", "1.0")
    ```

*   **Updating NVRAM Info:**
    Fetches current NVRAM boot entries and identifies the entry for the current OS.
    ```python
    nvram_manager.update()
    ```

*   **Adding a New Boot Entry:**
    This method is crucial for adding a new kernel to boot from. It requires information about the OS, the drive, and kernel options.
    ```python
    # Assuming 'this_os', 'this_drive' are objects with necessary attributes
    # and 'kernel_opts' is a string of kernel parameters.
    # This is a conceptual example as the full script context is needed.
    # nvram_manager.add_entry(this_os, this_drive, kernel_opts, simulate=False)
    ```
    *   `this_os`: An object representing the operating system (e.g., having `name` and `version` attributes).
    *   `this_drive`: An object representing the boot drive (e.g., having `drive_name` and `esp_num` attributes).
    *   `kernel_opts`: A string containing kernel command-line parameters.
    *   `simulate`: A boolean flag to only print the command without executing it.

*   **Deleting a Boot Entry:**
    Removes a specific boot entry by its index.
    ```python
    # Assuming 'index' is the boot entry number to delete (e.g., '0001')
    # nvram_manager.delete_boot_entry(index, simulate=False)
    ```
    *   `index`: The boot entry number (e.g., "0001") to be deleted.
    *   `simulate`: A boolean flag to only print the command without executing it.

**Note:** The `nvram.py` script appears to be a module intended for use within a larger application. Direct execution of this script without the necessary supporting objects and a main execution block might not be directly possible.

## Technologies Used

*   **Python 3:** The primary programming language.
*   **`subprocess`:** For executing external commands like `efibootmgr`.
*   **`logging`:** For emitting debug and error messages.
*   **`efibootmgr`:** The underlying Linux utility for managing UEFI boot variables.

## Architecture / Code Overview

The `NVRAM` class encapsulates the logic for interacting with the system's NVRAM via `efibootmgr`.

*   **`__init__(self, name, version)`:**
    Initializes the logger and sets the OS label based on provided name and version. It then calls `update()` to fetch initial NVRAM data.
*   **`update(self)`:**
    Refreshes the `self.nvram` list by calling `get_nvram()` and then attempts to find the OS entry using `find_os_entry()`. If found, it extracts the `order_num`.
*   **`get_nvram(self)`:**
    Executes `efibootmgr` using `subprocess.check_output()` and returns the output as a list of strings, split by newline characters. Includes error handling for `efibootmgr` execution.
*   **`find_os_entry(self, nvram, os_label)`:**
    Iterates through the `nvram` list to find the index of the entry that contains the specified `os_label`. Updates `self.os_entry_index`.
*   **`add_entry(self, this_os, this_drive, kernel_opts, simulate=False)`:**
    Constructs and executes an `efibootmgr -c` command to add a new boot entry. It dynamically generates the EFI paths for the kernel and initrd based on OS and drive UUID. Includes error handling and a `simulate` option.
*   **`delete_boot_entry(self, index, simulate)`:**
    Constructs and executes an `efibootmgr -B` command to delete a specified boot entry by its index. Includes error handling and a `simulate` option.

## Known Issues / Improvements

*   **Error Handling:** While basic error handling is present, more granular error messages or specific exception catching could be beneficial.
*   **Dependency on `efibootmgr`:** The script is tightly coupled to `efibootmgr`. Compatibility with different `efibootmgr` versions or alternative EFI boot managers is not addressed.
*   **Chroot Environment:** The `get_nvram` method explicitly mentions potential issues when running in a chroot. The script might require specific permissions or configurations in such environments.
*   **Object Dependencies:** Methods like `add_entry` rely on external objects (`this_os`, `this_drive`) which are not defined within this script, suggesting it's part of a larger system.
*   **Concurrency:** No explicit handling for concurrent modifications to NVRAM is present.
*   **Cross-Platform Compatibility:** This script is specifically designed for Linux systems with UEFI support and `efibootmgr`.

## Additional Notes / References

*   **License:** The code is provided under a permissive license (MIT-like), allowing use, modification, and distribution with or without fee, provided the copyright notice and permission notice are included. Refer to the `LICENSE.txt` file for full terms.
*   **Copyright:** Copyright © 2017-2018 Ian Santopietro <isantop@gmail.com>
*   **Related Tools:**
    *   `efibootmgr`: The command-line utility for managing UEFI boot entries.
    *   **Arch Wiki - EFI Boot Loaders:** For general information on EFI booting on Arch Linux.