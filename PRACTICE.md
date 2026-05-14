# Bash Scripting — Practice Problems

> Solve every problem yourself before scrolling to the hints. The only way to get strong at Bash is to write scripts, break them, and fix them.

**How to use:**
1. Read the problem
2. Try to solve it on your own
3. If stuck for 15+ minutes, scroll to the **Hints** section at the bottom
4. After solving, run `shellcheck yourscript.sh` — fix every warning

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

## Stage 8 — Real-World Scripting

### P8.1 — Disk Usage Monitor
Write a cron-ready script that checks disk usage of `/`. If > 80%, log a warning. If > 90%, also print to stderr. Include timestamps in every log line.

---

### P8.2 — User Account Report
Read `/etc/passwd` and print a formatted report of all non-system users (UID >= 1000):
```
Username   UID   Home Directory      Shell
dhruv      1000  /home/dhruv         /bin/bash
```

---

### P8.3 — Rolling Log Archiver
If a log file exceeds 1MB: compress it with a timestamp, start a fresh empty log, and keep only the last 5 archives (delete older ones).

---

### P8.4 — Health Check Script
Check four things and print `[OK]` or `[FAIL]` for each:
1. Is a given port open?
2. Is a URL reachable?
3. Is disk usage below 90%?
4. Is a specific process running?

Exit 1 if any check fails.

---

### P8.5 — Bulk File Renamer
Rename all files in a directory: lowercase names, spaces → underscores, add date prefix. Default is dry-run (print only). Add `-f` flag to actually rename.

---

## Stage 9 — Arithmetic & Math

### P9.1 — Statistics Calculator
Read numbers from a file (one per line). Print: count, sum, min, max, mean, and median.

---

### P9.2 — Unit Converter
Build a menu-driven converter for: Celsius↔Fahrenheit, Kilometers↔Miles, Kilograms↔Pounds. Use `bc` for decimal math.

---

### P9.3 — Fibonacci Sequence
Print the first N Fibonacci numbers. Implement it two ways: loop and recursive function. Compare speed with `time` for N=30.

---

### P9.4 — Compound Interest Table
Ask for principal, annual rate (%), and years. Print a year-by-year balance table using: `A = P * (1 + r)^t`.

---

### P9.5 — Prime Number Checker
Write `is_prime n` that returns 0 if n is prime. Print all primes up to 100.

---

## Stage 10 — Process Management

### P10.1 — Parallel Pinger
Given a file of hostnames/IPs, ping each one in parallel. Print `UP` or `DOWN` for each. Wait for all to finish before printing a summary.

---

### P10.2 — Process Monitor
Monitor a process by name every 5 seconds. If it stops, log the time and restart it. Run for a maximum of 10 checks.

---

### P10.3 — Timeout Wrapper
Write a function `run_with_timeout cmd seconds` that kills the command if it exceeds the time limit. Print whether it completed or timed out.

---

### P10.4 — Job Queue
Process a list of commands (one per line in a file) with a maximum of 3 running in parallel at any time.

---

### P10.5 — Signal Handler
Write a script with a counting loop that:
- On `SIGINT` (Ctrl+C): prints current count and asks "Really quit? [y/n]"
- On `SIGTERM`: saves count to a file and exits cleanly
- On `SIGHUP`: resets counter to 0 and continues

---

## Stage 11 — Bash Networking (/dev/tcp)

### P11.1 — Port Checker
Write a script that accepts a hostname and port as arguments and reports whether the port is open or closed. Add a configurable timeout (default 3 seconds). Exit 0 if open, exit 1 if closed.
```
./portcheck.sh google.com 443   →  google.com:443 OPEN
./portcheck.sh localhost 9999   →  localhost:9999 CLOSED
```

---

### P11.2 — Multi-Service Health Check
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

### P11.3 — Raw HTTP Status Checker
Write a function `http_status host path` that opens a raw TCP connection to port 80 and sends a minimal HTTP/1.0 GET request. Print only the status line from the response (e.g. `HTTP/1.0 200 OK`). Test it against `example.com /`.

---

### P11.4 — TCP Alert Sender
Write a function `send_alert host port message` that sends a message over a TCP connection. Then write a monitoring script that checks disk usage every 10 seconds and sends an alert to `localhost:9000` if usage exceeds 80%. Test by running `nc -l 9000` in another terminal to receive the alert.

---

### P11.5 — UDP StatsD Metric Sender
Write a function `send_metric name value` that sends a StatsD-format counter metric over UDP to `localhost:8125`:
```
myapp.logins:1|c
```
Then write a script that sends 5 different metrics. Test by running `nc -u -l 8125` in another terminal.

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
`pct=$(df / | awk 'NR==2{print $5}' | tr -d '%')`. Then `(( pct > 80 ))` and `(( pct > 90 ))` checks.

### Hint P8.2
`awk -F: '$3 >= 1000 {printf "%-10s %-5s %-20s %s\n", $1, $3, $6, $7}' /etc/passwd`.

### Hint P8.3
`size=$(stat -c%s "$logfile")`. `(( size > 1048576 ))` to check. `gzip -c "$logfile" > "${logfile}.$(date +%s).gz"`. `find "$dir" -name "*.gz" | sort | head -n -5 | xargs rm -f`.

### Hint P8.4
`nc -z host port 2>/dev/null` for port check. `curl -sf url` for URL. `df / | awk 'NR==2{print $5}' | tr -d '%'` for disk. `pgrep -x name` for process.

### Hint P8.5
`new="${old,,}"` (lowercase). `new="${new// /_}"` (spaces to underscores). `new="$(date +%Y-%m-%d)_${new}"`. Check `-f` flag before actually calling `mv`.

---

### Hint P9.1
Sort the array for median. `median=${arr[$(( count / 2 ))]}` for odd count. Use `bc` for mean: `echo "scale=2; $sum / $count" | bc`.

### Hint P9.2
C to F: `echo "scale=2; ($c * 9/5) + 32" | bc`. km to miles: `echo "scale=2; $km * 0.621371" | bc`. kg to lbs: `echo "scale=2; $kg * 2.20462" | bc`.

### Hint P9.3
Loop version: `prev=0; curr=1`. Each iteration: `next=$(( prev + curr )); prev=$curr; curr=$next`. Recursive: `fib() { (( $1 <= 1 )) && echo $1 || echo $(( $(fib $(($1-1))) + $(fib $(($1-2))) )); }`.

### Hint P9.4
`echo "scale=2; $p * e($t * l(1 + $r / 100))" | bc -l` for compound interest. Loop from year 1 to N.

### Hint P9.5
Check divisibility from 2 to `sqrt(n)`: `for ((i=2; i*i<=n; i++))`. If `(( n % i == 0 ))` → not prime.

---

### Hint P10.1
```bash
for host in "${hosts[@]}"; do
    (ping -c1 -W1 "$host" &>/dev/null && echo "$host: UP" || echo "$host: DOWN") &
    pids+=($!)
done
for pid in "${pids[@]}"; do wait "$pid"; done
```

### Hint P10.2
`while (( checks < 10 )); do pgrep -x "$proc" > /dev/null || { log "died, restarting"; start_proc; }; sleep 5; ((checks++)); done`.

### Hint P10.3
`$cmd & pid=$!; sleep $timeout & spid=$!; wait -n $pid $spid`. Check which finished first with `kill -0`.

### Hint P10.4
```bash
while IFS= read -r cmd; do
    (( ${#pids[@]} >= 3 )) && wait "${pids[0]}" && pids=("${pids[@]:1}")
    eval "$cmd" & pids+=($!)
done < tasks.txt
wait
```

### Hint P10.5
```bash
trap 'read -p "Count=$count. Really quit? [y/n] " a; [[ $a == y ]] && exit 1' INT
trap 'echo $count > /tmp/count.txt; exit 0' TERM
trap 'count=0' HUP
```

---

### Hint P11.1
`(echo > /dev/tcp/"$host"/"$port") &>/dev/null` returns 0 if open. Wrap in `timeout 3 bash -c "echo > /dev/tcp/$host/$port"` for the timeout.

### Hint P11.2
Reuse the port check from P11.1. Split each service string: `host="${svc%%:*}"` and `port="${svc##*:}"`. Track a counter for UP services.

### Hint P11.3
```bash
http_status() {
    exec 3<>/dev/tcp/"$1"/80
    printf "GET %s HTTP/1.0\r\nHost: %s\r\nConnection: close\r\n\r\n" "$2" "$1" >&3
    read -r status <&3
    exec 3>&-
    echo "$status"
}
```

### Hint P11.4
```bash
send_alert() { echo "$3" > /dev/tcp/"$1"/"$2"; }
```
For the monitor loop: `pct=$(df / | awk 'NR==2{print $5}' | tr -d '%')` then `(( pct > 80 )) && send_alert localhost 9000 "ALERT: disk at ${pct}%"`.

### Hint P11.5
```bash
send_metric() { echo "$1:$2|c" > /dev/udp/localhost/8125; }
```
Call it 5 times: `send_metric "myapp.logins" 1`, `send_metric "myapp.errors" 0`, etc.
