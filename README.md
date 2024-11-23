# sqlite3\_backup

Automated SQLite Database Backup Script with Integrity Verification and
Retention Policies.

- - -

## Table of Contents

* [Overview](#overview)
* [Key Features](#key-features)
* [Installation](#installation)
* [Usage](#usage)
  * [Parameters](#parameters)
  * [Options](#options)
  * [Examples](#examples)
* [Directory Structure](#directory-structure)
* [License](#license)
* [Contributing](#contributing)
* [Support](#support)

- - -

## Overview

`sqlite3_backup` is a robust and versatile Bash script designed to automate the
backup process of SQLite databases. Whether you're managing a single database
or multiple databases across various environments, this script ensures your
data is securely backed up, verified for integrity, and maintained with
customizable retention policies.

- - -

## Key Features

* **Multiple Database Support:**  
    Easily back up single or multiple SQLite databases from directories or
    using glob patterns.

* **Integrity Verification:**  
    Automatically verify each backup using SQLite's `PRAGMA integrity_check;`
    to ensure data consistency and reliability.

* **Compression:**  
    Compress backups into efficient `.tar.gz` archives to save storage space
    and streamline backup management.

* **Organized Backup Structure:**  
    Organize backups by year-month and day directories, providing a clear and
    manageable backup history.

* **Retention Policies:**  
    Automatically delete backups older than a specified number of days to manage
    storage effectively and comply with data retention requirements.

* **Verbose Logging:**  
    Optional verbose mode provides detailed logs of backup operations, aiding in
    monitoring and troubleshooting.

* **Error Handling:**  
    Comprehensive error handling ensures reliable backup operations and alerts
    you to any issues during the backup process.

- - -

## Installation

1. **Clone the Repository:**

    `git clone https://github.com/yourusername/sqlite3_backup.git cd sqlite3_backup`

2. **Make the Script Executable:**

    `chmod +x sqlite3_backup`

3. **Ensure Dependencies are Installed:**

    * **sqlite3\_rsync:**  
        Ensure `sqlite3_rsync` is installed and accessible at `/usr/local/bin/sqlite3_rsync`.

        `sqlite3_rsync` can be installed as follows:

        ```bash
        git clone https://github.com/sqlite/sqlite.git
        cd sqlite
        ./configure
        make sqlite3-rsync
        ```

    * **sqlite3:**  
        The script uses `/usr/bin/sqlite3` for database integrity checks.

    * **GNU `tar` and `date`:**  
        Ensure you have GNU versions of `tar` and `date`. On macOS, you might need
        to install GNU Core Utilities via Homebrew:

        `brew install coreutils`

        This installs GNU `date` as `gdate` and GNU `tar` as `gtar`. You may need
        to modify the script to use `gdate` and `gtar` if you're on macOS.

    * **Example for Ubuntu/Debian:**

        `sudo apt-get update sudo apt-get install sqlite3 tar`

- - -

## Usage

`./sqlite3_backup <source> <destination> [options]`

### Parameters

* `<source>`:  
    Path to the source database(s). Can be a single file, a directory, or
    a glob pattern (e.g., `/path/to/*.sqlite3`).

* `<destination>`:  
    Path to the destination directory where backups will be stored. Must be
    an existing directory.

### Options

* `-V`, `--verbose`:  
    Enable verbose output for detailed logging of backup operations.

* `-h`, `--help`:  
    Display the help message with usage instructions.

* `-a`, `--age <days>`:  
    Specify the maximum age of backups to keep, in days. Backups older than
    this will be automatically deleted.

### Examples

1. **Backup a Single Database:**

    `./sqlite3_backup /path/to/database.db /path/to/backups`

2. **Backup All Databases in a Directory with Verbose Output:**

    `./sqlite3_backup /path/to/databases/ /path/to/backups --verbose`

3. **Backup Databases Matching a Glob Pattern and Retain Backups for 30 Days:**

    `./sqlite3_backup "/path/to/*.sqlite3" /path/to/backups --age 30`

4. Example Crontab Usage

    Automate the `sqlite3_backup` script using `cron` by adding the following
    entries to your crontab (`crontab -e`):

    ```bash
    # Daily backups at 2:00 AM
    0 2 * * * /usr/local/bin/sqlite3_backup /path/to/source /path/to/destination --age 30 >> /var/log/sqlite3_backup.log 2>&1

    # Hourly backups at the start of every hour
    0 * * * * /usr/local/bin/sqlite3_backup /path/to/source /path/to/destination --age 15 >> /var/log/sqlite3_backup.log 2>&1

    # Weekly backups every Sunday at 3:30 AM
    30 3 * * 0 /usr/local/bin/sqlite3_backup /path/to/source /path/to/destination --age 60 >> /var/log/sqlite3_backup.log 2>&1`

  **Notes:**

* **Absolute Paths:** Ensure you use absolute paths for both the script
  and log files to avoid issues with `cron`'s limited environment.
* **Permissions:** Verify that the user running the cron job has
  execute permissions for the `sqlite3_backup` script and write
  permissions for the destination and log directories.
* **Logging:** Redirecting both `stdout` and `stderr` to a log file
  helps in monitoring and troubleshooting backup operations.

- - -

Feel free to adjust the schedules and retention periods (`--age` values)
to fit your specific backup requirements.
- - -

## Directory Structure

Backups are organized in the following structure for easy navigation and management:

```
backups/
├── YYYY-MM/
│   ├── DD/
│   │   ├── _working/
│   │   │   ├── <database1>.sqlite3
│   │   │   └── <database2>.sqlite3
│   │   └── backup-YYYY-MM-DD-HH-MM-SS.tar.gz
│   └── ... └── ...
```

* **YYYY-MM:** Year and month of the backup.
* **DD:** Day of the month.
* **\_working/:** Temporary directory where databases are replicated before archiving.
* **backup-YYYY-MM-DD-HH-MM-SS.tar.gz:** Compressed backup archive.

- - -

## License

This project is licensed under the MIT License.

- - -

## Contributing

Contributions are welcome! Please follow these steps to contribute:

1. **Fork the Repository**

2. **Create a New Branch:**

    `git checkout -b feature/YourFeatureName`

3. **Commit Your Changes:**

    `git commit -m "Add your message here"`

4. **Push to the Branch:**

    `git push origin feature/YourFeatureName`

5. **Open a Pull Request**

- - -

## Support

If you encounter any issues or have questions, please open an issue in
the repository or contact the maintainer directly.
