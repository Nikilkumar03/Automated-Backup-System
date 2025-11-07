Backup Automation Script (backup.sh)

A powerful and flexible shell script for creating, verifying, and managing system backups — complete with logging, configuration, and retention policies.

Features

✅ Automatic Compression — Backs up folders into a single .tar.gz file
✅ Checksums (SHA256) — Ensures backup integrity
✅ Retention Rules — Keeps:

7 daily backups

4 weekly backups

3 monthly backups

✅ Configuration File (backup.config) — Control where and how backups run
✅ Logging (backup.log) — Detailed audit trail
✅ Dry Run Mode — Test actions without making changes
✅ Lock File Protection — Prevent multiple concurrent runs
✅ Verification — Auto-checksum and test extraction
✅ Optional Email Notifications — Simulated with email.txt
✅ Bonus: Restore and list functions included

Requirements

Bash (v4.0 or later)

tar, gzip, sha256sum, date, awk, df

Linux or macOS (tested)

Installation

Clone or download this repository.

Make the script executable:

chmod +x backup.sh

Create a configuration file named backup.config in the same directory.

Folder Structure
 
backup-system/
├── backup.sh # Main script
├── backup.config # Configuration file
├── README.md # Project documentation
├── logs/
│ └── backup.log # Detailed log file
├── backups/
│ ├── daily/ # Daily backups
│ ├── weekly/ # Weekly backups
│ └── monthly/ # Monthly backups
└── test_data/ # Sample data folder for testing
 


Usage

1️⃣ Create a Backup
./backup.sh /home/user/documents

Creates a backup named:

backup-2025-11-03-1630.tar.gz
backup-2025-11-03-1630.tar.gz.sha256
2️⃣ Dry Run (Simulate)
./backup.sh --dry-run /home/user/documents

Shows what would happen, without modifying anything.

3️⃣ List Available Backups
./backup.sh --list

Displays all stored backups with their creation dates and sizes.

4️⃣ Restore a Backup
./backup.sh --restore backup-2025-11-07-1430.tar.gz --to /home/user/restored_files
5️⃣ Delete Old Backups (Automatic)

The script automatically keeps:

Last 7 daily

Last 4 weekly

Last 3 monthly
and deletes older backups during each run.


Verification Steps

After creating a backup:

The script recalculates the checksum and compares it with the saved .sha256 file.

It extracts a test file from the archive to ensure it’s not corrupted.

If successful, you’ll see:

SUCCESS: Backup verified successfully

Example from backup.log:

[2025-11-03 16:30:15] INFO: Starting backup of /home/user/documents
[2025-11-03 16:30:45] SUCCESS: Backup created: backup-2025-11-03-1630.tar.gz
[2025-11-03 16:30:46] INFO: Checksum verified successfully
[2025-11-03 16:30:50] INFO: Deleted old backup: backup-2025-10-05-0900.tar.gz

Safety Mechanisms

Lock file: /tmp/backup.lock prevents overlapping runs

Automatic cleanup: Partial backups removed if interrupted

Permission & existence checks: Stops if source or destination invalid

Disk space check: Ensures enough room before starting

Error Handling  

Error	                        Message

Missing folder	Error:         Source folder not found
Permission denied	Error:      Cannot read folder, permission denied
Disk full	Error:              Not enough disk space for backup
Config missing	Warning:       backup.config not found, using defaults
Interrupted:	                 Partial files cleaned automatically

