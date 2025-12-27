# Kernelstub

The automatic manager for booting Linux on (U)EFI.

## Description

Kernelstub is a utility designed to simplify the management of your OS's EFI System Partition (ESP). It automates the process of copying the current kernel and initramfs image to the ESP, making them discoverable by most EFI bootloaders and firmware. Additionally, it can configure the system's NVRAM to add entries to the firmware boot menu for the kernel, ensuring these entries are updated when new kernel versions are installed.

## How to Use

### Installation

**Debian Packaging:**

```bash
git clone https://github.com/isantop/kernelstub
cd kernelstub
debuild -B
sudo dpkg -i ../kernelstub*.deb
```

**Python Packaging (for non-Debian systems or preference):**

```bash
git clone https://github.com/isantop/kernelstub
cd kernelstub
sudo python3 setup.py install --record > installed_files.txt
```
This will create an `installed_files.txt` file listing all installed files for easy removal.

### Usage

Kernelstub is typically run as root using `su` or `sudo`.

**Specifying Kernel Parameters:**

If your system requires specific kernel parameters, you can provide them as follows:

```bash
sudo kernelstub -c "options_go here wrapped in-quotes"
```

Using the `-o` option will save these parameters to the user configuration file for future use without manual specification.

By default, Kernelstub assumes `quiet splash` kernel options, which are generally compatible with most systems for initial boot.

**Verbosity:**

Control the output level with the `-v`/`--verbose` flag:
- **Default:** Shows only `Warning`, `Error`, and `Critical` messages.
- **`-v`:** Displays progress information.
- **`-vv`:** Includes debugging information for developers.

**Bootloader Management:**

- **`-m`, `--manage-only`:** Copies the kernel and initrd to the ESP but does not set up any NVRAM entries. This is useful when using separate boot managers like `bootctl`/`systemd-boot` or rEFIt/rEFInd.
- **`-l`, `--loader`:** Creates a `systemd-boot`-compatible loader configuration.

**Command-Line Options:**

| Option                   | Action                                            | Notes                               |
|--------------------------|---------------------------------------------------|-------------------------------------|
|`-h`, `--help`            | Display the help text.                            |                                     |
|`-d`, `--dry-run`         | Simulate the process without making changes.      |                                     |
|`-e PATH`, `--esp_path`   | Manually specify the ESP path.                    | Saves to config file.               |
|`-k PATH`, `--kernelpath` | Manually specify the path to the kernel image.    |                                     |
|`-i PATH`, `--initrd_path`| Manually specify the path to the initrd image.    |                                     |
|`-o "options"`,`--options`| Set kernel boot options.                          | Saves to config file.               |
|`-l`, `--loader`          | Create a `systemd-boot`-compatible loader config. | Saves to config file.               |
|`-s`, `--stub`            | Set up NVRAM entries for the copied kernel.       | Saves to config file.               |
|`-m`, `--manage-only`     | Don't set up any NVRAM entries.                   | Saves to config file.               |
|`-f`, `--force-update`    | Forcefully update the main `loader.conf`.         | May overwrite other OS entries.     |
|`-v`, `--verbose`         | Display more information to the command line.     | Multiple flags increase verbosity. |

*Options marked with `*` save their values to the configuration file.

## Configuration

Kernelstub uses a hierarchical configuration system for safety and flexibility.
- **Sample Configuration:** `/etc/kernelstub/SAMPLE` (non-functional, for reference)
- **Main Configuration:** `/etc/kernelstub/configuration` (created by default if missing)
- **Internal Default Configuration:** Used if no other configuration is found.

When options like `-o`, `-l`, `-s`, or `-m` are used, they are saved to the configuration file. Command-line options always take precedence over configuration file settings.

Distributions may provide a configuration template at `/etc/default/kernelstub`. This file should not be edited directly. Kernelstub will load `/etc/kernelstub/configuration` if it exists, overriding any distributor configuration.

## Return Codes

- **0:** Success
- **2:** Error parsing the configuration file.
- **3:** Error copying files required for installation.

## Technologies Used

- **Language:** Python 3
- **Core Functionality:** EFI System Partition (ESP) management, NVRAM configuration, kernel/initrd copying.

## Known Issues / Improvements

*   (No specific issues or planned improvements are detailed in the provided source.)

## Additional Notes

### Licence

Kernelstub is distributed under an ISC-based license.

```
Copyright 2017-2018 Ian Santopietro <isantop@gmail.com>

Permission to use, copy, modify, and/or distribute this software for any purpose
with or without fee is hereby granted, provided that the above copyright notice
and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH
REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND
FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT,
INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS
OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER
TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF
THIS SOFTWARE.