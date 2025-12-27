# kernelstub

The automatic manager for using the Linux Kernel EFI Stub to boot.

This program automatically keeps a copy of the Linux Kernel image and your `initrd.img` located on your EFI System Partition (ESP) on an EFI-compatible system. The benefits of this approach include being able to boot Linux directly and bypass the GRUB bootloader in many cases, which can save 4-6 seconds during boot on a system with a fast drive. Please note that there are no guarantees about boot time saved using this method.

For maximum safety, kernelstub recommends leaving an entry for GRUB in your system's ESP and NVRAM configuration, as GRUB allows you to modify boot parameters per-boot, which loading the kernel directly cannot do. The only other way to do this is to use an EFI shell to manually load the kernel and give it parameters from the EFI Shell. You can do this by using:

```bash
fs0:> vmlinuz-image initrd=EFI/path/to/initrd/stored/on/esp options
```

kernelstub will load parameters from the `/etc/default/kernelstub` config file.

---

## How to Use

### Installation

Kernelstub is typically installed via your distribution's package manager. If you are installing from source, ensure you have the necessary dependencies (like `python3-systemd` if using systemd's journal).

### CLI Usage

```bash
sudo kernelstub [OPTIONS]
```

**Example:**

To install the kernel stub with default settings:
```bash
sudo kernelstub -i
```

To set specific kernel options:
```bash
sudo kernelstub -k "root=UUID=YOUR-ROOT-UUID rw"
```

To add options:
```bash
sudo kernelstub -a "quiet splash"
```

To remove options:
```bash
sudo kernelstub -r "quiet"
```

**Options:**

*   `-i, --install-stub`: Install the kernel stub.
*   `-m, --manage-mode`: Enable management mode.
*   `-o, --off-loader`: Disable loader setup.
*   `-s, --setup-loader`: Enable loader setup.
*   `-p, --print-config`: Print current configuration.
*   `-k, --kernel-path <PATH>`: Specify the path to the kernel image.
*   `--initrd-path <PATH>`: Specify the path to the initrd image.
*   `-f, --force-update`: Force an update of the kernel stub.
*   `--add-options <OPTIONS>`: Add kernel options.
*   `--remove-options <OPTIONS>`: Remove kernel options.
*   `-l, --log-file <PATH>`: Specify a log file path.
*   `-v, --verbosity <LEVEL>`: Set verbosity level (0-2).
*   `--dry-run`: Perform a dry run without making changes.
*   `--preserve-live`: Preserve live mode if running from a live environment.
*   `--esp-path <PATH>`: Specify the path to the ESP.
*   `--root-path <PATH>`: Specify the root path (default is '/').

---

## Technologies Used

*   **Language:** Python 3
*   **Libraries:** `systemd.journal` (optional, for systemd journal logging)

---

## Architecture or Code Overview

The `application.py` script serves as the main entry point for `kernelstub`. It handles command-line argument parsing, logging configuration, and orchestrates the operations performed by other modules.

Key components:

*   **`CmdLineError`**: Custom exception for command-line related errors.
*   **`Kernelstub` Class**:
    *   `parse_options(self, options)`: Parses kernel command line options, handling quoted strings.
    *   `main(self, args)`: The primary function that executes the `kernelstub` logic. It:
        *   Configures logging (console, file, and optionally systemd journal).
        *   Determines runtime options based on arguments and configuration.
        *   Identifies the kernel and initrd image paths.
        *   Loads and updates kernel parameters from configuration.
        *   Initializes core modules: `Drive`, `Nvram`, `Opsys`, `Installer`, `Config`, `KernelOption`.
        *   Logs system information and configuration details.
        *   Calls the `Installer` to perform the actual setup, backup, and stub installation.
        *   Saves the updated configuration.
*   **`Opsys` Module**: Detects and provides information about the operating system.
*   **`Drive` Module**: Interacts with disk drives, specifically identifying the ESP and root partitions.
*   **`Nvram` Module**: Manages NVRAM entries for boot configurations.
*   **`Installer` Module**: Handles the core logic of copying kernel/initrd images, setting up the EFI stub, and backing up previous configurations.
*   **`Config` Module**: Reads and writes `kernelstub` configuration files (e.g., `/etc/default/kernelstub`).
*   **`KernelOption` Module**: Manages the parsing and retrieval of kernel boot options.

---

## Known Issues / Improvements

*   **Error Handling**: While basic error handling is present, more robust checks for filesystem permissions, disk space, and EFI variable access could be implemented.
*   **Distribution Specifics**: The script assumes a standard Linux directory structure (`/boot`, `/etc/default`). Adaptations might be needed for highly customized or non-standard distributions.
*   **NVRAM Variable Management**: The interaction with EFI NVRAM variables can be complex and system-dependent. More detailed logging or explicit user guidance for NVRAM issues could be beneficial.
*   **GRUB Integration**: The recommendation to keep GRUB is sound, but `kernelstub` could potentially offer more advanced integration or fallback mechanisms with GRUB.
*   **Testing**: Comprehensive testing across various UEFI implementations and Linux distributions would improve reliability.

---

## Additional Notes or References

*   **License**: This software is provided "as is" and the author disclaims all warranties with regard to this software including all implied warranties of merchantability and fitness. In no event shall the author be liable for any special, direct, indirect, or consequential damages or any damages whatsoever resulting from loss of use, data or profits, whether in an action of contract, negligence or other tortious action, arising out of or in connection with the use or performance of this software. Please see the provided `LICENSE.txt` file for additional distribution/copyright terms.
*   **Author**: Ian Santopietro `<isantop@gmail.com>`
*   **Copyright**: © 2017-2018