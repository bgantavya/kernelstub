# formatCode

## Folder Structure

* [formatCode/](formatCode/)
    * [formatCode.py](formatCode/formatCode.py)

## Description

This project provides a Python script to format Python code according to standard Python style guides. It aims to improve code readability and maintainability.

## How to Use

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd formatCode
   ```

2. **Run the formatter:**
   Execute the script with the path to the Python file you want to format.
   ```bash
   python formatCode/formatCode.py <path_to_your_python_file.py>
   ```
   The script will overwrite the original file with the formatted code.

## Technologies Used

* Python 3.0+

## Architecture or Code Overview

The `formatCode.py` script iterates through the lines of the input Python file and applies basic formatting rules such as:

*   Ensuring consistent indentation.
*   Adding/removing blank lines for better separation of logical blocks.
*   Standardizing spacing around operators.

## Known Issues / Improvements

*   Currently supports only basic formatting.
*   Could be extended to support more complex PEP 8 rules.
*   Error handling for non-Python files can be improved.

## Additional Notes or References

*   This script is a simplified approach to code formatting. For comprehensive formatting, consider using tools like Black, Flake8, or isort.