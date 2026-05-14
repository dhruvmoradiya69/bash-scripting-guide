# Bash Scripting — Practice Problems

> Solve every problem yourself before scrolling to the hints. The only way to get strong at Bash is to write scripts, break them, and fix them.

**How to use:**
1. Read the problem
2. Try to solve it on your own
3. If stuck for 15+ minutes, scroll to the **Hints** section at the bottom
4. After solving, run `shellcheck yourscript.sh` — fix every warning

---

## Table of Contents

1. [Stage 1 — The Basics](#stage-1--the-basics)
2. [Stage 2 — Control Flow](#stage-2--control-flow)
3. [Stage 3 — Functions & Arguments](#stage-3--functions--arguments)
4. [Stage 4 — Strings & Arrays](#stage-4--strings--arrays)
5. [Stage 5 — Input/Output & Redirection](#stage-5--inputoutput--redirection)
6. [Stage 6 — Text Processing Tools](#stage-6--text-processing-tools)
7. [Stage 7 — Error Handling & Debugging](#stage-7--error-handling--debugging)
8. [Stage 8 — File Permissions & Ownership](#stage-8--file-permissions--ownership)
9. [Stage 9 — User Management](#stage-9--user-management)
10. [Stage 10 — Real-World Scripting](#stage-10--real-world-scripting)
11. [Stage 11 — Arithmetic & Math](#stage-11--arithmetic--math)
12. [Stage 12 — Process Management](#stage-12--process-management)
13. [Stage 13 — System Monitoring](#stage-13--system-monitoring)
14. [Stage 14 — Bash Networking (/dev/tcp)](#stage-14--bash-networking-devtcp)
15. [Stage 15 — Regular Expressions](#stage-15--regular-expressions)
16. [Stage 16 — Networking Tools](#stage-16--networking-tools)
17. [Stage 17 — Package Management](#stage-17--package-management)
18. [Stage 18 — File Compression](#stage-18--file-compression)
19. [Stage 19 — Text Editors](#stage-19--text-editors)
20. [Capstone Challenges](#capstone-challenges)
21. [Answer Checklist](#answer-checklist)

---

## Stage 1 — The Basics

### P1.1 — Hello, You
Ask the user for their name and age, then print:
```
Hello, Dhruv! You are 25 years old.
```

---

### P1.2 — Environment Inspector
Print the following without hardcoding any values:
```
Script name : ./script.sh
Your shell  : /bin/bash
Your home   : /home/dhruv
Bash version: 5.1.16
```

---

### P1.3 — Argument Echo
Accept exactly 3 arguments and print each one with its position. If fewer than 3 are given, print a usage error and exit with code 1.
```
Argument 1: foo
Argument 2: bar
Argument 3: baz
Total args : 3
```

---

### P1.4 — Silent Password Check
Ask the user to enter a password silently (no echo on screen). If it equals `secret123`, print `Access granted`. Otherwise print `Access denied` and exit 1.

---

### P1.5 — System Snapshot
Print a one-line system snapshot using only command substitution (no hardcoding):
```
[2026-05-14 10:00:00] hostname=mypc user=dhruv shell=/bin/bash
```

---

## Stage 2 — Control Flow

### P2.1 — Grade Calculator
Ask for a score (0–100). Print the grade: A (90–100), B (80–89), C (70–79), D (60–69), F (below 60). Reject non-numeric input or out-of-range values with an error message.

---

### P2.2 — FizzBuzz
Print numbers 1 to 50. For multiples of 3 print `Fizz`, multiples of 5 print `Buzz`, multiples of both print `FizzBuzz`.

---

### P2.3 — Multiplication Table
Ask the user for a number, print its multiplication table from 1 to 10 in aligned format:
```
5 x 1  = 5
5 x 10 = 50
```

---

### P2.4 — Menu-Driven Script
Build an interactive menu that keeps looping until the user exits:
```
1) Show date and time
2) Show current directory
3) List files
4) Show disk usage
5) Exit
```

---

### P2.5 — Countdown Timer
Ask for a number of seconds. Count down to 0, updating the same line each second (no scrolling). Print `Done!` at the end.

---

### P2.6 — Number Pattern
Ask for a number `n`. Print this pattern (example n=4):
```
1
1 2
1 2 3
1 2 3 4
```

---

## Stage 3 — Functions & Arguments

### P3.1 — Calculator Functions
Write four functions: `add`, `subtract`, `multiply`, `divide`. Each takes two numbers and echoes the result. `divide` must handle division by zero. Then write a `calculate` wrapper that calls the right function based on an operator argument.
```
calculate 10 + 5   →  15
calculate 10 / 0   →  Error: division by zero
```

---

### P3.2 — Recursive Factorial
Write a recursive function `factorial n` that echoes `n!`. Handle base case (0! = 1) and reject negative input.

---

### P3.3 — Script with getopts
Write a script that accepts:
- `-n name` — name to greet (required)
- `-c count` — how many times (default: 1)
- `-u` — uppercase output
- `-h` — show help and exit

Example: `./greet.sh -n dhruv -c 3 -u` → prints `HELLO, DHRUV!` three times.

---

### P3.4 — Utility Library
Create `utils.sh` with three functions:
- `is_number val` — returns 0 if val is an integer
- `is_file path` — returns 0 if path is a readable file
- `trim str` — echoes string with leading/trailing whitespace removed

Create `main.sh` that sources `utils.sh` and tests all three.

---

### P3.5 — Retry Wrapper
Write a function `retry cmd max_attempts delay` that runs a command up to `max_attempts` times, waiting `delay` seconds between tries. Print which attempt succeeded or print a failure message if all attempts fail.

---

## Stage 4 — Strings & Arrays

### P4.1 — String Dissector
Given `path="/home/dhruv/projects/app.tar.gz"`, extract and print using only `${}` (no external tools):
```
Full path  : /home/dhruv/projects/app.tar.gz
Filename   : app.tar.gz
Basename   : app
Extension  : gz
Directory  : /home/dhruv/projects
Uppercase  : /HOME/DHRUV/PROJECTS/APP.TAR.GZ
```

---

### P4.2 — Array Statistics
Create an array of 10 numbers. Print: all elements, sum, minimum, maximum, and average (with decimals).

---

### P4.3 — Word Frequency Counter
Ask the user to enter a sentence. Count how many times each word appears and print results sorted by frequency (highest first):
```
Input : the cat sat on the mat the cat
Output:
3 the
2 cat
1 sat
1 on
1 mat
```

---

### P4.4 — Array Deduplicator
Write a function `dedup` that takes an array and echoes back only the unique elements, preserving original order.

---

### P4.5 — Palindrome Check
Write a function `is_palindrome str` that returns 0 if the string is a palindrome (ignore case and spaces). Test with: `racecar`, `hello`, `A man a plan a canal Panama`.

---

## Stage 5 — Input/Output & Redirection

### P5.1 — Tee Logger
Run three commands (`date`, `uptime`, `df -h /`). Send all output to both the terminal AND `/tmp/report.log`. Each line must be prefixed with a timestamp.

---

### P5.2 — Error Separator
Run `ls /tmp /nonexistent /etc`. Redirect stdout to `out.txt` and stderr to `err.txt`. Then print the contents of both files with labels.

---

### P5.3 — Pipeline Challenge
Using a single pipeline (no temp files, no loops), find the top 3 most common words in `/etc/os-release`.

---

### P5.4 — Here-Doc Config Generator
Write a script that takes `hostname` and `port` as arguments and generates a config file:
```ini
[server]
host = myserver
port = 8080
log  = /var/log/myserver.log
date = 2026-05-14
```

---

### P5.5 — CSV Parser
Given a CSV file with columns `name,age,city`, read it line by line (skip the header), and print only people older than 25:
```
Dhruv (28) lives in Ahmedabad
```

---

## Stage 6 — Text Processing Tools

### P6.1 — Log Analyzer
Given a log file where lines look like:
```
2026-05-14 10:23:01 ERROR nginx: connection refused
2026-05-14 10:23:05 INFO  nginx: request served
```
Print: total lines, ERROR count, INFO count, last 5 errors, top 3 error sources.

---

### P6.2 — Config File Editor
Given a config file with `KEY=value` lines, write a script that supports:
- `get KEY` — print the value
- `set KEY value` — update or add the key
- `delete KEY` — remove the line

---

### P6.3 — Duplicate File Finder
Find all files in a directory that have identical content (regardless of name). Print each group of duplicates.

---

### P6.4 — Apache Log Top IPs
Given an Apache access log (each line starts with an IP), print the top 10 IPs by request count.

---

### P6.5 — CSV to Table
Read a CSV file and print it as a formatted table with headers and borders:
```
+--------+-----+-----------+
| name   | age | city      |
+--------+-----+-----------+
| Dhruv  | 25  | Ahmedabad |
+--------+-----+-----------+
```

---

## Stage 7 — Error Handling & Debugging

### P7.1 — Safe Directory Cleaner
Write a script that deletes all `.tmp` files older than 7 days in a given directory. Must: validate the directory, use `set -euo pipefail`, log every deletion with timestamp, never delete outside the given directory.

---

### P7.2 — Command Validator
Check if required commands exist (`git`, `curl`, `jq`, `docker`). Warn for each missing one. Exit 1 with a summary if any are missing.

---

### P7.3 — Trap Demo
Write a script that creates a temp file, writes data to it, sleeps 30 seconds, and always deletes the temp file on exit — whether it finishes normally, is Ctrl+C'd, or is killed.

---

### P7.4 — Debug Mode Flag
Add a `-d` debug flag to a script. When passed, enable `set -x` and print extra diagnostic info. When not passed, run silently.

---

### P7.5 — Exit Code Chain
Run 5 commands in sequence. If any fails, log which command failed (with exit code and line number) and skip the rest. Print a pass/fail summary at the end.

---

## Stage 8 — File Permissions & Ownership

### P8.1 — Permission Reader
Write a script that takes a file path as argument and prints a human-readable permission report:
```
File  : /etc/passwd
Perms : -rw-r--r--  (644)
Owner : root:root
Size  : 2.3K
```
**No hint here — scroll to bottom.**

---

### P8.2 — Safe Script Installer
Write a script that takes a `.sh` file and "installs" it by:
- Checking it exists and is a valid file
- Setting permissions to `755`
- Copying it to `~/bin/` (create the dir if missing)
- Verifying the copy is executable

---

### P8.3 — Permission Auditor
Write a script that scans a given directory and reports:
- World-writable files (`-perm -o+w`)
- Files with no owner (orphaned)
- Setuid/setgid files
- Files with `777` permissions

Print each category with a count and list of files.

---

### P8.4 — umask Demo
Write a script that:
1. Shows the current umask
2. Creates a file and directory with the default umask — show their permissions
3. Sets umask to `077`, creates another file and directory — show permissions
4. Explains the difference in output

---

### P8.5 — Root Guard
Write a function `require_root` that exits with a clear error if the script is not run as root. Write a second function `require_not_root` that exits if run AS root. Use both in a script that demonstrates each case.

---

## Stage 9 — User Management

### P9.1 — User Existence Checker
Write a function `user_exists username` that returns 0 if the user exists, 1 if not. Then write a script that takes a list of usernames as arguments and prints `EXISTS` or `NOT FOUND` for each.

---

### P9.2 — User Info Report
Read `/etc/passwd` and print a formatted table of all non-system users (UID >= 1000):
```
USERNAME     UID    HOME                 SHELL
dhruv        1000   /home/dhruv          /bin/bash
```

---

### P9.3 — Idempotent User Creator
Write a script that creates a user with these properties:
- Username from argument
- Home directory created
- Shell set to `/bin/bash`
- Added to `sudo` group

If the user already exists, print a skip message and exit 0. If the group doesn't exist, create it first. Must be safe to run multiple times.

---

### P9.4 — Group Membership Checker
Write a script that takes a username and prints all groups they belong to. Then check if they are in `sudo` or `wheel` — print `Has admin access` or `No admin access`.

---

### P9.5 — Security Audit
Write a script that checks for common user security issues and prints a report:
- Users with UID 0 (other than root)
- Users with empty passwords
- Locked accounts
- Users whose shell is `/bin/false` or `/sbin/nologin`

## Stage 10 — Real-World Scripting

### P10.1 — Disk Usage Monitor
Write a cron-ready script that checks disk usage of `/`. If > 80%, log a warning. If > 90%, also print to stderr. Include timestamps in every log line.

---

### P10.2 — User Account Report
Read `/etc/passwd` and print a formatted report of all non-system users (UID >= 1000):
```
Username   UID   Home Directory      Shell
dhruv      1000  /home/dhruv         /bin/bash
```

---

### P10.3 — Rolling Log Archiver
If a log file exceeds 1MB: compress it with a timestamp, start a fresh empty log, and keep only the last 5 archives (delete older ones).

---

### P10.4 — Health Check Script
Check four things and print `[OK]` or `[FAIL]` for each:
1. Is a given port open?
2. Is a URL reachable?
3. Is disk usage below 90%?
4. Is a specific process running?

Exit 1 if any check fails.

---

### P10.5 — Bulk File Renamer
Rename all files in a directory: lowercase names, spaces → underscores, add date prefix. Default is dry-run (print only). Add `-f` flag to actually rename.

---

## Stage 11 — Arithmetic & Math

### P11.1 — Statistics Calculator
Read numbers from a file (one per line). Print: count, sum, min, max, mean, and median.

---

### P11.2 — Unit Converter
Build a menu-driven converter for: Celsius↔Fahrenheit, Kilometers↔Miles, Kilograms↔Pounds. Use `bc` for decimal math.

---

### P11.3 — Fibonacci Sequence
Print the first N Fibonacci numbers. Implement it two ways: loop and recursive function. Compare speed with `time` for N=30.

---

### P11.4 — Compound Interest Table
Ask for principal, annual rate (%), and years. Print a year-by-year balance table using: `A = P * (1 + r)^t`.

---

### P11.5 — Prime Number Checker
Write `is_prime n` that returns 0 if n is prime. Print all primes up to 100.

---

## Stage 12 — Process Management

### P12.1 — Parallel Pinger
Given a file of hostnames/IPs, ping each one in parallel. Print `UP` or `DOWN` for each. Wait for all to finish before printing a summary.

---

### P12.2 — Process Monitor
Monitor a process by name every 5 seconds. If it stops, log the time and restart it. Run for a maximum of 10 checks.

---

### P12.3 — Timeout Wrapper
Write a function `run_with_timeout cmd seconds` that kills the command if it exceeds the time limit. Print whether it completed or timed out.

---

### P12.4 — Job Queue
Process a list of commands (one per line in a file) with a maximum of 3 running in parallel at any time.

---

### P12.5 — Signal Handler
Write a script with a counting loop that:
- On `SIGINT` (Ctrl+C): prints current count and asks "Really quit? [y/n]"
- On `SIGTERM`: saves count to a file and exits cleanly
- On `SIGHUP`: resets counter to 0 and continues

---

## Stage 13 — System Monitoring

### P13.1 — System Snapshot
Write a script that prints a one-line snapshot of CPU usage, memory usage, and disk usage of `/` — all on one line with percentages:
```
CPU: 23%  MEM: 61%  DISK: 45%  [2026-05-14 10:00:00]
```

---

### P13.2 — Resource Alert Monitor
Write a script that checks CPU, memory, and disk every 10 seconds. If any metric exceeds 80%, append a warning to `/tmp/alerts.log` with a timestamp. Run for 60 seconds total.

---

### P13.3 — Port & Connection Monitor
Write a script that prints:
- All TCP ports currently listening (with the process name)
- Number of established connections
- Refresh every 5 seconds until Ctrl+C

---

### P13.4 — Disk Usage Report
Write a script that:
- Lists all mounted filesystems with usage > 70%
- Finds the top 5 largest directories under `/var/log`
- Finds all files larger than 50MB on the system
- Prints everything as a formatted report

---

### P13.5 — Full System Dashboard
Combine CPU, memory, disk, top 3 processes, and listening ports into a single dashboard script. Use a progress bar function for the metrics. Refresh every 3 seconds. Exit cleanly on Ctrl+C.

## Stage 14 — Bash Networking (/dev/tcp)

### P14.1 — Port Checker
Write a script that accepts a hostname and port as arguments and reports whether the port is open or closed. Add a configurable timeout (default 3 seconds). Exit 0 if open, exit 1 if closed.
```
./portcheck.sh google.com 443   →  google.com:443 OPEN
./portcheck.sh localhost 9999   →  localhost:9999 CLOSED
```

---

### P14.2 — Multi-Service Health Check
Given this list of services hardcoded in an array:
```
google.com:80  google.com:443  localhost:22  localhost:3306
```
Check each one in a loop and print a formatted report:
```
google.com:80       [UP]
localhost:3306      [DOWN]
```
Print a summary at the end: `X/Y services are UP`.

---

### P14.3 — Raw HTTP Status Checker
Write a function `http_status host path` that opens a raw TCP connection to port 80 and sends a minimal HTTP/1.0 GET request. Print only the status line from the response (e.g. `HTTP/1.0 200 OK`). Test it against `example.com /`.

---

### P14.4 — TCP Alert Sender
Write a function `send_alert host port message` that sends a message over a TCP connection. Then write a monitoring script that checks disk usage every 10 seconds and sends an alert to `localhost:9000` if usage exceeds 80%. Test by running `nc -l 9000` in another terminal to receive the alert.

---

### P14.5 — UDP StatsD Metric Sender
Write a function `send_metric name value` that sends a StatsD-format counter metric over UDP to `localhost:8125`:
```
myapp.logins:1|c
```
Then write a script that sends 5 different metrics. Test by running `nc -u -l 8125` in another terminal.

---

## Stage 15 — Regular Expressions

### P15.1 — Regex Validator
Write a script that reads a line of text and validates it against three patterns:
- Is it a valid IPv4 address? (e.g. `192.168.1.1`)
- Is it a valid email address? (e.g. `user@example.com`)
- Is it a valid date in `YYYY-MM-DD` format? (e.g. `2026-05-14`)

Print `VALID` or `INVALID` for each check.

---

### P15.2 — Log Level Filter
Given a log file where each line starts with a level (`INFO`, `WARN`, `ERROR`, `DEBUG`), write a script that accepts a level as argument and prints only lines matching that level. Use `grep -E`.
```
./filter.sh ERROR app.log
```

---

### P15.3 — Date Reformatter
Using `sed -E` with capture groups, read a file containing dates in `YYYY-MM-DD` format and rewrite them as `DD/MM/YYYY`.
```
Input : 2026-05-14
Output: 14/05/2026
```

---

### P15.4 — Word Extractor
Using `grep -oE`, extract all words from a file that:
- Start with a capital letter
- Are at least 5 characters long
- Contain only letters

Print each match on its own line, deduplicated and sorted.

---

### P15.5 — Input Sanitiser
Write a function `sanitise input` that uses `[[ =~ ]]` to:
- Reject if input contains anything other than letters, digits, underscores, hyphens
- Reject if input is shorter than 3 or longer than 20 characters
- Print `OK: <input>` or `REJECTED: <reason>`

---

## Stage 16 — Networking Tools

### P16.1 — Host Reachability Check
Write a script that accepts a list of hostnames as arguments. For each, use `ping -c 1 -W 2` to check if it's reachable. Print `UP` or `DOWN` and a final summary count.

---

### P16.2 — HTTP Status Checker
Write a script that accepts a URL and uses `curl` to print:
- HTTP status code
- Response time (`%{time_total}`)
- Whether it redirected (`%{redirect_url}`)

Exit 0 if status is 2xx, exit 1 otherwise.

---

### P16.3 — File Downloader with Retry
Write a script that downloads a URL using `wget`. If it fails, retry up to 3 times with a 5-second delay between attempts. Log each attempt with a timestamp.

---

### P16.4 — Remote Command Runner
Write a script that accepts a hostname and a command as arguments, runs the command over SSH, and prints the output with a `[hostname]` prefix on each line.
```
./remote.sh server1 "df -h /"
[server1] Filesystem      Size  Used Avail Use% Mounted on
[server1] /dev/sda1        20G   8G   11G  43% /
```

---

### P16.5 — Directory Sync
Write a script that uses `rsync` to sync a local directory to a remote server. Support:
- `-n` flag for dry-run
- `-d` flag for delete (mirror mode)
- Exclude `node_modules` and `.git` always

---

## Stage 17 — Package Management

### P17.1 — Package Checker
Write a script that checks if `git`, `curl`, `jq`, and `vim` are installed. For each missing one, print the install command for the current system's package manager (detect using `command -v apt/dnf/brew`).

---

### P17.2 — Idempotent Installer
Write a function `ensure_installed cmd pkg` that installs `pkg` only if `cmd` is not already in PATH. Test it by ensuring `curl`, `wget`, and `tree` are installed.

---

### P17.3 — Package List Installer
Read a file `packages.txt` (one package name per line, `#` lines are comments). Install each package that isn't already installed. Print a summary at the end: installed count, skipped count.

---

## Stage 18 — File Compression

### P18.1 — Archive Creator
Write a script that takes a directory path and creates a timestamped `.tar.gz` archive of it in `/tmp/`. Print the archive name and size when done.
```
./archive.sh /home/dhruv/projects
Created: /tmp/projects_20260514_120000.tar.gz (4.2M)
```

---

### P18.2 — Compression Comparator
Write a script that takes a file, compresses it with `gzip`, `bzip2`, and `xz` (keeping the original), and prints a comparison table:
```
Format   Size    Time
gzip     1.2M    0.3s
bzip2    1.0M    1.1s
xz       0.8M    3.4s
```

---

### P18.3 — Archive Extractor
Write a script `extract.sh` that detects the archive type from the filename extension and extracts it to a directory named after the archive (without extension). Support: `.tar.gz`, `.tar.bz2`, `.tar.xz`, `.zip`.
```
./extract.sh archive.tar.gz   →  extracts to ./archive/
./extract.sh data.zip         →  extracts to ./data/
```

---

### P18.4 — Log Compressor
Write a script that finds all `.log` files in `/var/log` older than 7 days, compresses each with `gzip`, and prints how many were compressed and total space saved.

---

## Stage 19 — Text Editors

### P19.1 — vim Muscle Memory
Open `vim`, perform these operations without looking up commands, then exit:
1. Open a new file `test.txt`
2. Write 5 lines of text
3. Delete line 3
4. Copy line 1 and paste it after line 4
5. Search for a word and replace all occurrences
6. Save and quit

*(No script needed — this is a hands-on drill)*

---

### P19.2 — nano Config Edit
Using only `nano`, open `/etc/hosts` (read-only with `nano -v`), find the line with `127.0.0.1`, and note what's on it. Then create a new file `hosts_note.txt` with that line copied in.

*(No script needed — hands-on drill)*

---

### P19.3 — Editor Detector
Write a script that checks which editors are available on the system (`vi`, `vim`, `nano`, `emacs`, `nvim`). Print each with its version and full path. At the end, print which one would be used as the default (`$EDITOR` or `$VISUAL` env var, fallback to first found).

---

## Capstone Challenges

### C1 — Mini Deployment Script ⭐⭐
Write `deploy.sh` that accepts `-e env` and `-v version`. Validates inputs, pulls the git tag, runs tests (abort if they fail), backs up current deployment, copies new files, logs every step. On any failure, rolls back and exits 1.

---

### C2 — System Health Dashboard ⭐⭐
Print a live-updating dashboard (refresh every 3 seconds) showing CPU, memory, disk usage as progress bars, top 3 CPU processes, and last 3 syslog lines. Exit cleanly on Ctrl+C.

---

### C3 — Log Rotation System ⭐⭐⭐
A complete log rotation system: monitors a directory, rotates files over a size limit, compresses them, keeps a configurable number of archives, writes a rotation report. Safe to run from cron every hour.

---

### C4 — User Onboarding Script ⭐⭐⭐
Read a CSV of new users (`username,fullname,group,shell`). For each: create account, set random temp password, add to group, create home directory structure, copy skeleton `.bashrc`, log everything, print summary table. Must be idempotent.

---

### C5 — Backup & Restore System ⭐⭐⭐
Two scripts: `backup.sh` (incremental backups, compression, checksum verification, keep last N) and `restore.sh` (list backups, pick one, verify checksum, dry-run by default, `-f` to restore).

---

## Answer Checklist

For every problem you solve, verify:

- [ ] `shellcheck script.sh` passes with no warnings
- [ ] Script handles missing/wrong arguments gracefully
- [ ] Variables are quoted: `"$var"` not `$var`
- [ ] Functions use `local` for their variables
- [ ] `set -euo pipefail` is at the top of every non-trivial script
- [ ] Edge cases work: empty input, spaces in filenames, zero values
- [ ] Script exits with the right code (0 = success, non-zero = failure)

---

---

# Hints

> Only scroll here after you've genuinely tried. Reading hints too early robs you of the learning.

---

> Only scroll here after you've genuinely tried. Reading hints too early robs you of the learning.

---

### Hint P1.1
Use `read -p "Enter name: " name` and `read -p "Enter age: " age`, then `echo "Hello, $name! You are $age years old."`.

### Hint P1.2
`$0` = script name, `$SHELL` = current shell, `$HOME` = home dir, `$BASH_VERSION` = bash version (no command needed).

### Hint P1.3
Check `if [ $# -lt 3 ]` at the top. Use `$1`, `$2`, `$3` and `$#`.

### Hint P1.4
`read -s -p "Password: " pass`. Compare with `[[ "$pass" == "secret123" ]]`.

### Hint P1.5
`echo "[$(date '+%Y-%m-%d %H:%M:%S')] hostname=$(hostname) user=$USER shell=$SHELL"`.

---

### Hint P2.1
Validate with `[[ $score =~ ^[0-9]+$ ]]`. Then check range with `(( score >= 0 && score <= 100 ))`.

### Hint P2.2
Check `(( n % 3 == 0 && n % 5 == 0 ))` first, then `% 3`, then `% 5`, else print the number.

### Hint P2.3
`printf "%d x %-2d = %d\n" "$n" "$i" "$(( n * i ))"` inside a `for i in {1..10}` loop.

### Hint P2.4
`while true; do` → print menu → `read choice` → `case $choice in 1) ... ;; 5) break ;; esac` → `done`.

### Hint P2.5
`printf "\rTime left: %d seconds  " "$i"` then `sleep 1` then `((i--))`. Use `echo` after the loop for `Done!`.

### Hint P2.6
Outer loop `for ((row=1; row<=n; row++))`, inner loop `for ((col=1; col<=row; col++))`, `printf "%d " $col`. `echo` after inner loop.

---

### Hint P3.1
Each function: `add() { echo $(( $1 + $2 )); }`. For divide: `(( $2 == 0 )) && { echo "Error: division by zero" >&2; return 1; }`. Wrapper uses `case "$2"`.

### Hint P3.2
Base case: `(( $1 == 0 )) && echo 1 && return`. Recursive: `echo $(( $1 * $(factorial $(( $1 - 1 ))) ))`.

### Hint P3.3
`while getopts "n:c:uh" opt; do case $opt in ...`. After parsing, loop `count` times. Apply `${name^^}` if `-u` was set.

### Hint P3.4
`source ./utils.sh` in main.sh. `is_number`: `[[ $1 =~ ^-?[0-9]+$ ]]`. `trim`: `echo "$1" | sed 's/^[[:space:]]*//;s/[[:space:]]*$//'`.

### Hint P3.5
```bash
retry() {
    local cmd="$1" max="$2" delay="$3"
    for ((i=1; i<=max; i++)); do
        $cmd && echo "Succeeded on attempt $i" && return 0
        sleep "$delay"
    done
    echo "Failed after $max attempts" >&2; return 1
}
```

---

### Hint P4.1
`${path##*/}` = filename, `${path%/*}` = directory, `${path##*.}` = extension, `${path%.*}` = strip extension, `${path^^}` = uppercase.

### Hint P4.2
Loop `for n in "${arr[@]}"`. Track min/max by comparing each element with `(( n < min ))`. Use `echo "scale=2; $sum / ${#arr[@]}" | bc` for average.

### Hint P4.3
`declare -A freq`. Split sentence into words with `read -ra words <<< "$sentence"`. Loop: `freq[$w]=$(( ${freq[$w]:-0} + 1 ))`. Print with `for w in "${!freq[@]}"; do echo "${freq[$w]} $w"; done | sort -rn`.

### Hint P4.4
`declare -A seen`. Loop array: `[[ -v seen[$el] ]] || { echo "$el"; seen[$el]=1; }`.

### Hint P4.5
Strip spaces: `clean="${str// /}"`. Lowercase: `clean="${clean,,}"`. Reverse: `rev=$(echo "$clean" | rev)`. Compare: `[[ "$clean" == "$rev" ]]`.

---

### Hint P5.1
```bash
log() { echo "[$(date '+%H:%M:%S')] $*" | tee -a /tmp/report.log; }
log "$(date)"
log "$(uptime)"
log "$(df -h /)"
```

### Hint P5.2
`ls /tmp /nonexistent /etc > out.txt 2> err.txt`. Then `echo "=== STDOUT ===" && cat out.txt` etc.

### Hint P5.3
`cat /etc/os-release | tr ' ' '\n' | tr -d '"=' | grep -v '^$' | sort | uniq -c | sort -rn | head -3`.

### Hint P5.4
```bash
cat << EOF > config.ini
[server]
host = $1
port = $2
log  = /var/log/$1.log
date = $(date +%Y-%m-%d)
EOF
```

### Hint P5.5
```bash
while IFS=, read -r name age city; do
    [[ "$name" == "name" ]] && continue   # skip header
    (( age > 25 )) && echo "$name ($age) lives in $city"
done < data.csv
```

---

### Hint P6.1
`grep -c "ERROR"`, `grep -c "INFO"`, `grep "ERROR" | tail -5`, `grep "ERROR" | awk '{print $5}' | sort | uniq -c | sort -rn | head -3`.

### Hint P6.2
`get`: `grep -E "^$key=" file | cut -d= -f2`. `set`: `grep -q "^$key=" file && sed -i "s/^$key=.*/$key=$val/" file || echo "$key=$val" >> file`. `delete`: `sed -i "/^$key=/d" file`.

### Hint P6.3
`md5sum` every file, `sort` by hash, `awk` to group lines with the same hash: `awk '{if ($1==prev) print $2; else prev=$1}'`.

### Hint P6.4
`awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -10`.

### Hint P6.5
Use `awk` in two passes: first pass finds max column widths, second pass prints formatted rows. Or use `column -t -s,` for a quick version.

---

### Hint P7.1
`find "$dir" -maxdepth 1 -name "*.tmp" -mtime +7 -print -delete`. Validate with `[[ -d "$dir" && -w "$dir" ]]`. Use `trap 'log "Script exited"' EXIT`.

### Hint P7.2
```bash
missing=()
for cmd in git curl jq docker; do
    command -v "$cmd" &>/dev/null || missing+=("$cmd")
done
(( ${#missing[@]} > 0 )) && echo "Missing: ${missing[*]}" && exit 1
```

### Hint P7.3
`TMPFILE=$(mktemp)`. `trap 'rm -f "$TMPFILE"; echo "Cleaned up"' EXIT INT TERM`. Then `sleep 30`.

### Hint P7.4
Parse `-d` with `getopts`. Set `DEBUG=false`. After parsing: `[[ $DEBUG == true ]] && set -x`.

### Hint P7.5
Wrap each command in a function. Use `trap 'echo "Failed: $BASH_COMMAND (line $LINENO, exit $?)"' ERR`. Track pass/fail in counters.

---


### Hint P8.1
`stat -c "%A  (%a)" file` for perms. `stat -c "%U:%G" file` for owner. `stat -c "%s" file` for size. Use `ls -lh` for human-readable size.

### Hint P8.2
`[[ -f "$1" ]] || exit 1`. `mkdir -p ~/bin`. `chmod 755 "$1"`. `cp "$1" ~/bin/`. `[[ -x ~/bin/$(basename "$1") ]]`.

### Hint P8.3
`find "$dir" -perm -o+w` for world-writable. `find "$dir" -nouser` for orphaned. `find "$dir" -perm -4000 -o -perm -2000` for setuid/setgid. `find "$dir" -perm 777` for fully open.

### Hint P8.4
`umask` to show. `touch file && ls -l file`. `mkdir dir && ls -ld dir`. Then `umask 077` and repeat.

### Hint P8.5
`require_root() { [[ $EUID -eq 0 ]] || { echo "Must run as root"; exit 1; }; }`. `require_not_root() { [[ $EUID -ne 0 ]] || { echo "Must not run as root"; exit 1; }; }`.

### Hint P9.1
`id "$username" &>/dev/null` returns 0 if user exists. Loop over `"$@"` for multiple usernames.

### Hint P9.2
`awk -F: '$3 >= 1000 {printf "%-12s %-6s %-20s %s\n", $1, $3, $6, $7}' /etc/passwd`.

### Hint P9.3
Check with `id "$user" &>/dev/null`. Create group: `getent group sudo &>/dev/null || groupadd sudo`. Create user: `useradd -m -s /bin/bash -G sudo "$user"`.

### Hint P9.4
`groups "$user"` or `id -nG "$user"`. Check admin: `id -nG "$user" | grep -qw "sudo\|wheel"`.

### Hint P9.5
UID 0: `awk -F: '$3==0 {print $1}' /etc/passwd`. Empty password: `awk -F: '$2=="" {print $1}' /etc/shadow`. Locked: `passwd -S "$user" | grep -q " L "`. No-login shell: `awk -F: '$7 ~ /nologin|false/ {print $1}' /etc/passwd`.
### Hint P10.1
`pct=$(df / | awk 'NR==2{print $5}' | tr -d '%')`. Then `(( pct > 80 ))` and `(( pct > 90 ))` checks.

### Hint P10.2
`awk -F: '$3 >= 1000 {printf "%-10s %-5s %-20s %s\n", $1, $3, $6, $7}' /etc/passwd`.

### Hint P10.3
`size=$(stat -c%s "$logfile")`. `(( size > 1048576 ))` to check. `gzip -c "$logfile" > "${logfile}.$(date +%s).gz"`. `find "$dir" -name "*.gz" | sort | head -n -5 | xargs rm -f`.

### Hint P10.4
`nc -z host port 2>/dev/null` for port check. `curl -sf url` for URL. `df / | awk 'NR==2{print $5}' | tr -d '%'` for disk. `pgrep -x name` for process.

### Hint P10.5
`new="${old,,}"` (lowercase). `new="${new// /_}"` (spaces to underscores). `new="$(date +%Y-%m-%d)_${new}"`. Check `-f` flag before actually calling `mv`.

---

### Hint P14.1
Sort the array for median. `median=${arr[$(( count / 2 ))]}` for odd count. Use `bc` for mean: `echo "scale=2; $sum / $count" | bc`.

### Hint P14.2
C to F: `echo "scale=2; ($c * 9/5) + 32" | bc`. km to miles: `echo "scale=2; $km * 0.621371" | bc`. kg to lbs: `echo "scale=2; $kg * 2.20462" | bc`.

### Hint P14.3
Loop version: `prev=0; curr=1`. Each iteration: `next=$(( prev + curr )); prev=$curr; curr=$next`. Recursive: `fib() { (( $1 <= 1 )) && echo $1 || echo $(( $(fib $(($1-1))) + $(fib $(($1-2))) )); }`.

### Hint P14.4
`echo "scale=2; $p * e($t * l(1 + $r / 100))" | bc -l` for compound interest. Loop from year 1 to N.

### Hint P14.5
Check divisibility from 2 to `sqrt(n)`: `for ((i=2; i*i<=n; i++))`. If `(( n % i == 0 ))` → not prime.

---


### Hint P11.1
Loop array for sum/min/max. Sort array for median: `IFS=$'\n' sorted=($(sort -n <<< "${arr[*]}")); median=${sorted[$((count/2))]}`. Use `echo "scale=2; $sum/$count" | bc` for mean.

### Hint P11.2
C→F: `echo "scale=2; ($c * 9/5) + 32" | bc`. km→miles: `echo "scale=2; $km * 0.621371" | bc`. kg→lbs: `echo "scale=2; $kg * 2.20462" | bc`.

### Hint P11.3
Loop: `prev=0; curr=1; next=$(( prev+curr )); prev=$curr; curr=$next`. Recursive: `fib() { (($1<=1)) && echo $1 || echo $(( $(fib $(($1-1))) + $(fib $(($1-2))) )); }`.

### Hint P11.4
`echo "scale=2; $p * e($t * l(1 + $r/100))" | bc -l`. Loop year 1 to N printing balance each year.

### Hint P11.5
`for ((i=2; i*i<=n; i++)); do (( n%i==0 )) && return 1; done`. Loop 2–100 calling `is_prime`.

### Hint P12.1
`for host in "${hosts[@]}"; do (ping -c1 -W1 "$host" &>/dev/null && echo "$host UP" || echo "$host DOWN") & pids+=($!); done; for p in "${pids[@]}"; do wait "$p"; done`.

### Hint P12.2
`while (( checks < 10 )); do pgrep -x "$proc" > /dev/null || { log "died"; start_cmd & }; sleep 5; ((checks++)); done`.

### Hint P12.3
`$cmd & pid=$!; ( sleep $timeout; kill $pid 2>/dev/null ) & timer=$!; wait $pid && kill $timer 2>/dev/null`.

### Hint P12.4
`while IFS= read -r cmd; do (( ${#pids[@]} >= 3 )) && { wait "${pids[0]}"; pids=("${pids[@]:1}"); }; eval "$cmd" & pids+=($!); done < tasks.txt; wait`.

### Hint P12.5
`trap 'read -p "Count=$count. Quit? [y/n] " a; [[ $a == y ]] && exit 1' INT`. `trap 'echo $count > /tmp/count.txt; exit 0' TERM`. `trap 'count=0' HUP`.

### Hint P13.1
`cpu_idle=$(top -bn1 | grep "Cpu(s)" | awk '{print $8}' | tr -d '%id,')`. `mem_pct=$(echo "scale=0; $used*100/$total" | bc)`. `disk_pct=$(df / | awk 'NR==2{print $5}' | tr -d '%')`.

### Hint P13.2
`ps aux --sort=-%cpu | head -6 | tail -5` for top 5. `ps aux --sort=-%mem | head -6 | tail -5` for memory. Alert: `(( cpu_used > threshold )) && echo "ALERT" >> logfile`.

### Hint P13.3
`ss -tlnp | awk 'NR>1{print $4}' | sort` for ports. `ss -tnp | grep -c ESTABLISHED` for connections. Loop with `sleep 5` and `clear` each iteration.

### Hint P13.4
`df -h | awk 'NR>1 && $5+0 > 80 {print $6, $5}'` for over-threshold mounts. `du -sh /var/log/* | sort -rh | head -5` for largest dirs.

### Hint P13.5
Combine CPU (`/proc/stat`), memory (`free -m`), disk (`df`), top processes (`ps aux --sort=-%cpu`), and network (`ss -s`) into one script. Use a `bar()` function for visual meters.

### Hint P15.1
IPv4: `[[ $input =~ ^([0-9]{1,3}\.){3}[0-9]{1,3}$ ]]`. Email: `[[ $input =~ ^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$ ]]`. Date: `[[ $input =~ ^[0-9]{4}-[0-9]{2}-[0-9]{2}$ ]]`.

### Hint P15.2
`grep -E "^$level" "$logfile"`. Use `case` to validate the level argument first.

### Hint P15.3
`sed -E 's/([0-9]{4})-([0-9]{2})-([0-9]{2})/\3\/\2\/\1/' file.txt`.

### Hint P15.4
`grep -oE '\b[A-Z][a-zA-Z]{4,}\b' file.txt | sort -u`.

### Hint P15.5
`[[ "$input" =~ ^[a-zA-Z0-9_-]{3,20}$ ]]`. Check length separately with `${#input}` if you want specific error messages.

### Hint P16.1
`ping -c 1 -W 2 "$host" &>/dev/null && echo "$host UP" || echo "$host DOWN"`. Track counts with `((up++))` and `((down++))`.

### Hint P16.2
`curl -s -o /dev/null -w "%{http_code} %{time_total} %{redirect_url}" "$url"`. Check `[[ ${code:0:1} == "2" ]]` for 2xx.

### Hint P16.3
`for i in {1..3}; do wget -q "$url" && break || { echo "Attempt $i failed"; sleep 5; }; done`.

### Hint P16.4
`ssh "$host" "$cmd" | while IFS= read -r line; do echo "[$host] $line"; done`.

### Hint P16.5
Parse flags with `getopts`. Build rsync flags array: `flags=(-avz --exclude='node_modules' --exclude='.git')`. Add `--dry-run` for `-n`, `--delete` for `-d`.

### Hint P17.1
`command -v apt &>/dev/null && mgr="apt install"`. Loop over tools, check with `command -v`, print install command if missing.

### Hint P17.2
`ensure_installed() { command -v "$1" &>/dev/null && return 0; install_package "$2"; }`. Reuse the `install_package` function from README Stage 17.4.

### Hint P17.3
`grep -v '^#' packages.txt | grep -v '^$'` to get clean list. Check each with `command -v`, track `installed=0` and `skipped=0` counters.

### Hint P18.1
`name=$(basename "$dir")`. `out="/tmp/${name}_$(date +%Y%m%d_%H%M%S).tar.gz"`. `tar -czf "$out" "$dir"`. `du -sh "$out" | cut -f1` for size.

### Hint P18.2
For each format: `start=$SECONDS; gzip -k file; elapsed=$(( SECONDS - start ))`. Use `du -sh` for size. Print with `printf "%-8s %-8s %s\n"`.

### Hint P18.3
Use `case "${file##*.}" in gz) ... ;; zip) ... ;; esac`. For `.tar.gz` strip two extensions: `base="${file%.tar.gz}"`. Extract to `mkdir "$base" && tar -xzf "$file" -C "$base"`.

### Hint P18.4
`find /var/log -name "*.log" -mtime +7`. For each: `gzip -v "$f"` and capture size before/after with `stat -c%s`.

### Hint P19.1
No hint — the point is to practice from memory. Refer to README Stage 19.2 only after genuinely trying.

### Hint P19.2
No hint — hands-on drill. Use `Ctrl+W` to search in nano.

### Hint P19.3
`for ed in vi vim nano emacs nvim; do command -v "$ed" &>/dev/null && echo "$ed: $(command -v $ed) — $($ed --version 2>&1 | head -1)"; done`. Check `${EDITOR:-${VISUAL:-}}` for default.