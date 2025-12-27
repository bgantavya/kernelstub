# kernelstub

## Folder Structure

```
.
├── bin/
│   └── kernelstub
├── data/
│   ├── config/
│   │   └── kernelstub.SAMPLE
│   ├── initramfs/
│   │   └── zz-kernelstub
│   └── kernel/
│       └── zz-kernelstub
├── kernelstub/
│   └── <python_package_files>
└── setup.py
```

## Description

`kernelstub` is an automatic kernel EFI stub manager designed for UEFI systems. It helps manage kernel EFI stub configurations.

## How to Use

### Installation

To install `kernelstub` and integrate it into your system:

```bash
sudo python3 setup.py install
```

Alternatively, you can install it using pip:

```bash
pip install .
```

This will install the `kernelstub` package, the `kernelstub` executable, and place necessary data files:
-   `/etc/kernel/postinst.d/zz-kernelstub`
-   `/etc/initramfs/post-update.d/zz-kernelstub`
-   `/etc/default/kernelstub.SAMPLE`

### Testing

To run static analysis tests (using `pyflakes3`):

```bash
python3 setup.py test
```

To skip the static analysis tests:

```bash
python3 setup.py test --skip-flakes
```

### Command-Line Interface

The main executable script is `kernelstub`, installed to your system's `PATH` during installation.

```bash
kernelstub --help
```

## Technologies Used

*   **Python**: Primary programming language.
*   **setuptools**: Used for packaging and distribution.
*   **pyflakes3**: Utilized for static code analysis during testing.

## Architecture or Code Overview

The project is structured around a `setuptools`-based distribution system.

*   **`setup.py`**: The core of the project's distribution. It defines:
    *   **Project Metadata**: Name (`kernelstub`), version (`3.1.4`), description, author, and license.
    *   **Packages**: Includes the `kernelstub` Python package.
    *   **Scripts**: Installs `bin/kernelstub` as a system executable.
    *   **Data Files**: Distributes system-level configuration and hook scripts to:
        *   `/etc/kernel/postinst.d/` for kernel post-installation actions.
        *   `/etc/initramfs/post-update.d/` for initramfs update actions.
        *   `/etc/default/` for a sample configuration file.
    *   **Custom `test` Command**: An extension of `setuptools.Command` that runs `pyflakes3` for static code analysis.
*   **`kernelstub/`**: This directory contains the Python package code, which provides the core logic for managing kernel EFI stubs. (Internal details of this package are not directly visible in `setup.py`).
*   **`bin/kernelstub`**: The main entry point script that users will execute.
*   **Data Hooks**: The distributed `zz-kernelstub` scripts in `data/kernel/` and `data/initramfs/` are designed to integrate `kernelstub` with system-level kernel and initramfs update processes.

## Known Issues / Improvements

*   The internal architecture and API of the `kernelstub` Python package are not detailed in this `setup.py`. Further documentation would be beneficial for developers looking to contribute or integrate the core library.

## Additional Notes or References

*   **Copyright**: Copyright 2017-2018 Ian Santopietro.
*   **License**: ISC License.
*   **Project Homepage**: https://launchpad.net/kernelstub
*   **Credits**: Portions of test-related code authored by Jason DeRose.