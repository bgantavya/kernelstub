# kernelstub

A simple, yet powerful, tool to manage kernel efistubs on UEFI systems.

## Table of Contents

* [Description](#description)
* [How to Use](#how-to-use)
    * [Installation](#installation)
    * [Command-Line Usage](#command-line-usage)
* [Technologies Used](#technologies-used)
* [Architecture or Code Overview](#architecture-or-code-overview)
* [Known Issues / Improvements](#known-issues--improvements)
* [Additional Notes or References](#additional-notes-or-references)

## Description

`kernelstub` is a Python-based utility designed to simplify the management of EFI boot entries, specifically for Linux kernels. It allows users to easily add, remove, and manage kernel efistubs, which are the bootloader entries stored directly on the EFI System Partition (ESP). This is particularly useful for systems that boot directly from the EFI firmware without the need for a traditional bootloader like GRUB.

## How to Use

### Installation

The recommended way to install `kernelstub` is using `pip`:

```bash
pip install kernelstub
```

Alternatively, if you have the Debian package available:

```bash
sudo dpkg -i kernelstub_*.deb
```

### Command-Line Usage

`kernelstub` provides a command-line interface for managing efistubs.

**Add a new kernel efistub:**

```bash
sudo kernelstub -a
```

This command will prompt you for necessary information, such as the kernel path, initrd path, and kernel parameters.

**List existing efistubs:**

```bash
sudo kernelstub -l
```

This will display all currently configured EFI boot entries.

**Remove an efistub:**

```bash
sudo kernelstub -r <entry_name>
```

Replace `<entry_name>` with the name of the efistub you wish to remove.

**Set a default efistub:**

```bash
sudo kernelstub -d <entry_name>
```

This command sets the specified efistub as the default boot entry.

**Update an efistub:**

```bash
sudo kernelstub -u <entry_name>
```

This command allows you to update an existing efistub.

For a full list of options, run:

```bash
kernelstub -h
```

## Technologies Used

*   **Language:** Python 3
*   **Core Libraries:**
    *   `efibootmgr`: A tool to interface with EFI firmware's boot manager.
    *   `python3-debian`: Utilities for Debian package management.
    *   `util-linux`: General system utilities.
    *   `systemd` (Recommended): For deeper integration with systemd services.

## Architecture or Code Overview

The `kernelstub` script is the main entry point. It parses command-line arguments and delegates operations to different functions. The core logic involves interacting with the `efibootmgr` command-line utility through Python's `subprocess` module to read, create, update, and delete EFI boot variables. It also handles file system operations to locate kernel and initrd images and constructs the necessary command lines for the efistub entries.

Key components include:

*   **Argument Parsing:** Uses `argparse` to handle user commands and options.
*   **EFI Interaction:** Functions that call `efibootmgr` to manage boot entries.
*   **Kernel/Initrd Discovery:** Logic to find appropriate kernel and initrd files.
*   **Configuration Management:** Reading and writing configuration for efistubs.

## Known Issues / Improvements

*   **Error Handling:** More robust error handling for edge cases during `efibootmgr` execution.
*   **User Interface:** Potentially a more interactive or guided user experience for adding efistubs.
*   **Systemd Integration:** Enhanced integration with systemd for automatic updates on kernel installations.
*   **Cross-distribution Support:** While primarily designed for Debian-based systems, improving compatibility with other distributions could be beneficial.

## Additional Notes or References

*   **Maintainer:** Ian Santopietro `<isantop@gmail.com>`
*   **License:** (Details not provided in source, assuming standard open-source license)
*   **Related Tools:** `efibootmgr`, GRUB, systemd-boot.