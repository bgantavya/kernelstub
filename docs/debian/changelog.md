# kernelstub

This project provides a command-line utility for managing kernel boot entries and related configurations.

## Folder Structure

This project appears to be a single changelog file and does not have a traditional folder structure for source code.

## Description

The `kernelstub` utility is designed to simplify the process of configuring bootloaders and kernel parameters. It allows users to add, remove, or modify kernel options, manage boot entries, and handle kernel image locations. The changelog indicates a focus on reliability, ease of use, and integration with system services like `systemd-journald`.

## How to Use

Since this is a changelog, specific usage instructions for the `kernelstub` tool itself would be found in its main documentation or source code repository. However, based on the changelog entries, common operations might include:

*   **Adding kernel options:**
    ```bash
    sudo kernelstub -a <option>
    ```
*   **Removing kernel options:**
    ```bash
    sudo kernelstub -r <option>
    ```
*   **Viewing current configuration:**
    ```bash
    sudo kernelstub -p
    ```
*   **Handling live mode:**
    ```bash
    sudo kernelstub --live-mode
    ```

**Note:** Actual command syntax and options may vary. Refer to the `kernelstub` project's official documentation for precise usage.

## Technologies Used

*   **Language:** Python (inferred from common usage in system utilities and presence of `.py` files in similar projects)
*   **Packaging:** `stdeb` (mentioned in changelog entry 0.18)

## Architecture or Code Overview

The changelog indicates a significant rewrite in version 2.0.0, suggesting a modular and organized structure. Key components likely include:

*   **Configuration Parsing:** Reading and interpreting user-defined settings and system configurations.
*   **Kernel/Initrd Detection:** Locating necessary kernel and initrd images in the `/boot` directory or other specified paths.
*   **Bootloader Integration:** Modifying bootloader configurations (e.g., GRUB, systemd-boot) or NVRAM entries.
*   **Live Mode Handling:** Special logic to prevent unintended modifications on live boot media.
*   **Logging:** Integration with `systemd-journald` for system-level logging.

## Known Issues / Improvements

*   **Breaking API Changes:** Version 3.1.0 mentions a breaking API change in 2.3.0, with renamed/removed options (`--dry-run`) and changed short-form options. Users should be aware of these changes when upgrading.
*   **UUID Detection:** Earlier versions (pre-2.0.8) had less reliable UUID detection, which has since been improved.
*   **Drive Detection:** Code was rewritten for more reliable drive detection in version 2.0.7.

## Additional Notes or References

*   **License:** The project transitioned from BSD to ISC license in version 1.0.0.
*   **Origin:** The project appears to be developed and maintained by System76 employees.