# Kernel Stub Manager

This project provides a set of Python scripts to manage kernel stub configurations, primarily for OpenCore bootloader. It allows for inspection, modification, and installation of kernel stub settings.

## Folder Structure

*   [application.py](./application.py)
*   [drive.py](./drive.py)
*   [installer.py](./installer.py)
*   [kernel_option.py](./kernel_option.py)
*   [nvram.py](./nvram.py)
*   [opsys.py](./opsys.py)

## Description

The Kernel Stub Manager aims to simplify the process of configuring kernel stubs within an OpenCore EFI. It interacts with various system components to read, write, and manage kernel patch data, operating system configurations, and NVRAM settings relevant to boot processes.

## How to Use

This project is designed to be run as a collection of Python scripts. Specific usage instructions will depend on the functionality of each script.

**Example:**

To inspect kernel options, you might run:

```bash
python kernel_option.py --inspect /path/to/config.plist
```

To install kernel stubs, you might run:

```bash
python installer.py --install /path/to/efi/partition
```

*(Note: Actual commands and arguments may vary. Refer to individual script documentation or `--help` flags for precise usage.)*

## Technologies Used

*   **Language:** Python 3

## Architecture or Code Overview

*   **`application.py`**: Likely handles the main application logic and user interface, orchestrating calls to other modules.
*   **`drive.py`**: Provides utilities for interacting with storage drives, potentially for mounting EFI partitions or accessing filesystems.
*   **`installer.py`**: Manages the installation or update of kernel stub configurations onto the EFI system.
*   **`kernel_option.py`**: Deals with the parsing, manipulation, and validation of kernel stub options within configuration files (e.g., `config.plist`).
*   **`nvram.py`**: Contains functions for reading and writing NVRAM variables, crucial for bootloader settings.
*   **`opsys.py`**: Manages operating system-specific configurations and settings relevant to kernel stubs and booting.

## Known Issues / Improvements

*   **Error Handling:** Robust error handling can be improved across all modules.
*   **User Interface:** A more user-friendly CLI or a GUI could enhance usability.
*   **Configuration Validation:** More comprehensive validation of `config.plist` structures and values.
*   **Cross-Platform Compatibility:** Ensure compatibility across different operating systems and environments.

## Additional Notes or References

*   **Licensing:** [Specify License Here]
*   **Credits:** [Acknowledge any contributors or inspirations]
*   **Related Tools:**
    *   OpenCore Bootloader: [Link to OpenCore Documentation]