# Kernelstub

## Folder Structure

```
.
├── [data](data/)
│   ├── [initramfs](data/initramfs/)
│   │   └── zz-kernelstub
│   └── [kernel](data/kernel/)
│       └── zz-kernelstub
├── [debian](debian/)
│   ├── [source](debian/source/)
│   │   ├── format
│   │   └── options
│   ├── changelog
│   ├── compat
│   ├── conffiles
│   ├── control
│   ├── copyright
│   ├── kernelstub.postinst
│   ├── license
│   ├── readme
│   └── rules
├── [kernelstub](kernelstub/)
│   ├── application.py
│   ├── drive.py
│   ├── installer.py
│   ├── kernel_option.py
│   ├── nvram.py
│   └── opsys.py
├── COPYRIGHT.txt
├── LICENSE.txt
└── setup.py
```

## Description

Kernelstub is an automatic manager for booting Linux on (U)EFI systems. It simplifies the process of managing your OS's EFI System Partition (ESP) by copying the current kernel and initramfs image onto the ESP, making them automatically bootable by most EFI bootloaders and the EFI firmware itself. Additionally, it can configure the system's NVRAM to add and maintain firmware boot menu entries for the kernel, ensuring they stay up-to-date with new kernel versions.

## How to Use

### Installation

Kernelstub can be installed via Debian packaging or Python packaging.

**Debian-based Systems**

If `kernelstub` is available in your distro's repositories, install it using your package manager (e.g., `sudo apt install kernelstub`).

To build a Debian package locally:

1.  **Install Build Dependencies:**
    ```bash
    sudo apt build-dep kernelstub
    # Alternatively, install specific packages:
    # sudo apt install python3-all pyflakes3 debhelper dh-python
    ```
2.  **Build and Install:**
    ```bash
    git clone https://github.com/pop-os/kernelstub
    cd kernelstub
    dpkg-buildpackage -b -us -uc
    sudo dpkg -i ../kernelstub*.deb
    ```

**Non-Debian Systems or Python Packaging**

1.  **Clone the Repository:**
    ```bash
    git clone https://github.com/pop-os/kernelstub
    cd kernelstub
    ```
2.  **Install using `setup.py`:**
    ```bash
    sudo python3 setup.py install --record=installed_files.txt
    ```
    This command will create an `installed_files.txt` list of all installed files for easy removal later.

### Usage

Kernelstub commands generally require root privileges (`su`/`sudo`).

**Basic Usage**

To specify special kernel parameters:

```bash
sudo kernelstub -o "options_go here wrapped in-quotes"
```
Using `-o` will save these options into the user configuration file for future automatic runs. By default, `quiet splash` options are assumed.

**Verbosity**

Control output verbosity using the `-v`/`--verbose` option:

*   **Default:** Shows `Warning`, `Error`, and `Critical` messages.
*   **`-v`:** Displays information about program progress.
*   **`-vv`:** Displays detailed debugging information.

**NVRAM and Loader Configuration**

*   By default, Kernelstub attempts to set up an NVRAM entry to boot the kernel directly.
*   To use Kernelstub with a separate boot program (e.g., `bootctl`/`systemd-boot`, rEFIt/rEFInd), use the `-m` flag. This copies the kernel and initrd to the ESP without setting NVRAM entries.
*   The `-l` option explicitly configures for `systemd-boot` or `gummiboot`.

**Command-Line Options**

| Option                                    | Action                                                      |
| :---------------------------------------- | :---------------------------------------------------------- |
| `-h`, `--help`                            | Display the help Text                                       |
| `-c`, `--dry-run`                         | Don't actually copy any files or set anything up.           |
| `-p`, `--print-config`                    | Print the current configuration and exit.                   |
| ***Path Options***                        |                                                             |
| `-r <path>`, `--root-path <path>`         | Manually specify the root filesystem path.                  |
| `-e <path>`, `--esp-path <path>`          | Manually specify the ESP path.\*                            |
| `-k <path>`, `--kernel-path <path>`       | Manually specify the path to the kernel image.              |
| `-i <path>`, `--initrd-path <path>`       | Manually specify the path to the initrd image.              |
| ***Kernel Parameters***                   |                                                             |
| `-o <options>`, `--options <options>`     | Set kernel boot options.\*                                  |
| `-a <options>`, `--add-options <options>` | Adds new options to the list of kernel boot options.\*⁺     |
| `-d <options>`, `--delete-options <options>` | Remove options from the list of kernel boot options.\*⁺    |
| ***Output/logging Options***              |                                                             |
| `-v`, `--verbose`                         | Display more information to the command line                |
| `-g <log>`, `--log-file <log>`            | Where to save the log file.                                 |
| ***Behavior Options***                    |                                                             |
| `-l`, `--loader`                          | Create a `systemd-boot`-compatible loader config.\*         |
| `-n`, `--no-loader`                       | Turns off creating the loader configuration.                |
| `-s`, `--stub`                            | Set up NVRAM entries for the copied kernel.                 |
| `-m`, `--manage-only`                     | Don't set up any NVRAM entries.\*                           |
| `-f`, `--force-update`                    | Forcefully update the main loader.conf.**                   |

\*These options save information to the config file.
\*\*This may overwrite another OS's information.
⁺Does not add options if they are already present in the configuration, or remove options if they are not present. Each option is checked individually.

## Technologies Used

*   **Python 3**: Primary programming language for the `kernelstub` utility.
*   **Debian Packaging Tools**: `debhelper`, `dh-python` for creating `.deb` packages.

## Architecture or Code Overview

The `kernelstub` directory contains the core Python modules:

*   `application.py`: The main entry point and orchestrator for Kernelstub's operations. It handles command-line argument parsing and coordinates tasks.
*   `drive.py`: Handles detection and interaction with disk drives and partitions, particularly the EFI System Partition (ESP).
*   `installer.py`: Manages the process of copying kernel and initramfs images to the ESP and updating boot configuration files.
*   `kernel_option.py`: Responsible for parsing, managing, and applying kernel boot options.
*   `nvram.py`: Provides functionalities to interact with the system's NVRAM for creating, updating, and removing EFI boot entries.
*   `opsys.py`: Contains operating system-specific utilities, such as detecting current kernel and initramfs paths.

## Additional Notes or References

### Configuration

Kernelstub uses a robust configuration system with multiple fallbacks:

*   **Main Configuration**: Stored in `/etc/kernelstub/configuration`. This file is created if it doesn't exist and records options specified via `-o`, `-l`, `-s`, or `-m`.
*   **Sample Configuration**: A non-functional sample demonstrating available options and JSON syntax is found in `/etc/kernelstub/SAMPLE`.
*   **Distributor Configuration**: Your distribution may provide a template in `/etc/default/kernelstub`. This file is for maintainers and should not be edited by users.
*   **Precedence**: Command-line options always take precedence over options in configuration files. The user config file `/etc/kernelstub/configuration` takes precedence over distributor files.

### Return Codes

For scripting environments, Kernelstub provides the following exit codes:

| Exit Code | Meaning                                                      |
| :-------- | :----------------------------------------------------------- |
| 0         | Success                                                      |
| 166       | The kernel path supplied/detected was invalid                |
| 167       | The initrd path supplied/detected was invalid                |
| 168       | No kernel options found/supplied                             |
| 169       | Malformed configuration found                                |
| 170       | Couldn't copy kernel image to ESP                            |
| 171       | Couldn't copy initrd image to ESP                            |
| 172       | Couldn't create a new NVRAM entry                            |
| 173       | Couldn't remove an old NVRAM entry                           |
| 174       | Couldn't detect the block device file for the root partition |
| 175       | Coundn't detect the block device file for the ESP            |
| 176       | Wasn't run as root                                           |
| 177       | Couldn't get a required UUID                                 |

### License

Kernelstub is available under a COLPL + ISC-based license. The full license text is provided in the [LICENSE.txt](LICENSE.txt) file.

**Summary:**

Permission to use, copy, modify, and/or distribute this software for any purpose with or without fee is hereby granted, provided that the above copyright notice and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.

### Copyright

Copyright 2017-2018 Ian Santopietro <isantop@gmail.com>