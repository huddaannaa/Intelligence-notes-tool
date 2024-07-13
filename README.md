# Documentation for `hudsnote` Tool

## Overview

The `hudsnote` tool is designed to facilitate data entry through a command-line interface (CLI) by reading questions or fields stored in a file and presenting them to a user. The tool collects the questions as keys and the corresponding answers as values, formats them into JSON, and writes the data to a specified output file.

## License

This script is licensed under the BSD 3-Clause License. See the script header for details.

## Table of Contents

1. [Setup](#setup)
2. [Usage](#usage)
3. [User Guide](#user-guide)
4. [Script Breakdown](#script-breakdown)
   - [Imports](#imports)
   - [Class Definitions](#class-definitions)
   - [Signal Handling](#signal-handling)
   - [NTP Time Retrieval](#ntp-time-retrieval)
   - [Validation Functions](#validation-functions)
   - [Argument Parsing](#argument-parsing)
   - [Main Execution](#main-execution)
5. [Additional Notes](#additional-notes)

## Setup

### Requirements

- Python 3.x
- Required Libraries: `json`, `sys`, `time`, `datetime`, `signal`, `argparse`, `os`, `uuid`, `ntplib`, `hashlib`

### Installation

Make sure you have Python 3.x installed on your machine. You can install the required library `ntplib` using pip:

```bash
pip install ntplib
```

## Usage

### Command-Line Arguments

The script accepts the following command-line arguments:

- `-s, --source_input_file`: Specify a source input file name or path (required).
- `-fd, --field_delimiter`: Specify field delimiter (default: `_`).
- `-d, --destination_output_file`: Specify a destination output file name or path (optional).
- `-V, --validate_destination_output_file`: Validate destination output file name or path.
- `-c, --create_new_qb_file`: Create a new question book `.qb` file.
- `-R, --readme`: Short notes on how to use the app.
- `-D, --delete_file_after_run`: Delete the file after execution.
- `-N, --ntp_server`: Specify an NTP server for configuration (default: `1.asia.pool.ntp.org`).
- `-L, --license_token`: Supply license token from author.
- `-S, --make_destination_file_static`: Make the destination file static.

### Running the Script

To run the script, use the following command:

```bash
python hudsnote.py -s <source_input_file>
```

For example:

```bash
python hudsnote.py -s questions.qb -d output.json -N 0.pool.ntp.org -L <license_token>
```

## User Guide

### Preparing the Source Input File

Create a source input file (e.g., `questions.qb`) containing the fields or questions you want to collect answers for. The format should follow these guidelines:

- Lines starting with `#` are comments and will be ignored.
- Lines starting with `###` are mandatory fields and must be completed by the user.
- Other lines are considered optional fields.

#### Example:

```
# These fields are required and must be completed
### alert name, alert id, timestamp, user name, host name, source ip, source domain, revision status

# Data source: Endgame(EDR)
#-------------------------------------------------------
alert name
alert id
alert timestamp
alert revision status
user name
host name
source ip
source domain
destination ip
destination domain
source process name
source process path
target process name
target process path
target process path hash
process command line args
processes in process tree
file name
file hash
file path
file source
registry keys
other information1
other information2
other information3
analysis
conclusion
```

### Running the Script

To start the data entry process, run the script with the required arguments. For example:

```bash
python hudsnote.py -s questions.qb
```

### Data Entry Process

1. **Starting Data Entry:**
   - The script will prompt you to type `yes` to start data entry or `exit` to terminate the script.

2. **Entering Data:**
   - For each field, you will be prompted to provide an answer.
   - Hit the `[ENTER]` key once to move to the next field/question when the field/question is empty.
   - Hit the `[ENTER]` key twice to move to the next field/question when the field/question is not empty.

3. **Special Commands During Data Entry:**
   - `=back`: Move back to the previous field/question. Confirm by hitting the `[Enter]` key twice.
   - `=clear`: Clear the screen.
   - `=save`: Save the current content mid-way. At the end of the input process, a prompt will request you to confirm saving.
   - `=usage`: Display the help menu.

### Saving Data

At the end of the data entry process, the script will prompt you to confirm if the collected data inputs are valid. Type `yes` to save the data to the specified output file or `no` to exit without saving.

### Validating Output File

If the `-V` or `--validate_destination_output_file` argument is provided, the script will validate the JSON content in the specified output file.

### Self-Deletion

If the `-D` or `--delete_file_after_run` argument is provided, the script will delete itself after execution.

## Script Breakdown

### Imports

The script imports necessary libraries for various functionalities such as JSON handling, time management, signal handling, and argument parsing.

### Class Definitions

#### `Colors`

A class defining color codes for terminal output to enhance readability.

### Signal Handling

#### `handler(signum, frame)`

A function to handle the `SIGINT` signal (Ctrl+C), ensuring the program terminates gracefully.

### NTP Time Retrieval

#### `get_ntp_time(ntp_server)`

A function to retrieve the current time from an NTP server.

#### `hudpass(ntp_server, token="")`

A function to validate the license key based on the current NTP time.

### Validation Functions

#### `is_qb_file(filepath)`

Checks if the provided file has a `.qb` extension.

#### `create_qb_file(filepath=None)`

Creates an empty `.qb` file.

#### `validate_json_file(file_path)`

Validates the JSON content in the specified file.

### Argument Parsing

The script uses the `argparse` module to handle command-line arguments.

### Main Execution

The main execution flow includes reading the source input file, validating fields, collecting user inputs, and saving the data to a JSON file. It also includes options to create new `.qb` files, validate JSON output files, and delete the script after execution.

## Additional Notes

- Ensure the source input file contains valid fields or questions.
- Mandatory fields should be marked with `###`.
- The script supports various commands during data entry, such as `=back`, `=clear`, `=save`, and `=usage`.

For further assistance or inquiries, please contact the developer.
