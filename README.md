# Bash Scripting — Complete Reference

> This document is your single source of truth for Bash scripting. Every concept, pattern, gotcha, and project you need is here.

---

## Table of Contents

1. [Stage 1 — The Basics](#stage-1--the-basics-week-1)
2. [Stage 2 — Control Flow](#stage-2--control-flow-week-2)
3. [Stage 3 — Functions & Arguments](#stage-3--functions--arguments-week-3)
4. [Stage 4 — Strings & Arrays](#stage-4--strings--arrays-week-4)
5. [Stage 5 — Input/Output & Redirection](#stage-5--inputoutput--redirection-week-5)
6. [Stage 6 — Text Processing Tools](#stage-6--text-processing-tools-week-6)
7. [Stage 7 — Error Handling & Debugging](#stage-7--error-handling--debugging-week-7)
8. [Stage 8 — Real-World Scripting](#stage-8--real-world-scripting-week-8)
9. [Stage 9 — Arithmetic & Math](#stage-9--arithmetic--math)
10. [Stage 10 — Process Management & Signals](#stage-10--process-management--signals)
11. [Stage 11 — Bash Networking (/dev/tcp)](#stage-11--bash-networking-devtcp)
12. [Common Gotchas](#common-gotchas)
13. [Troubleshooting Guide](#troubleshooting-guide)
14. [Practice Projects](#practice-projects)
15. [Quick Reference Card](#quick-reference-card)

---

## Stage 1 — The Basics (Week 1)

**What:** The foundational building blocks — how to create a script, store data in variables, and take input from the user.  
**Why:** Every script you ever write will use these. Without variables and input, your script can only do one hardcoded thing.  
**How:** Write a `.sh` file, add a shebang line, make it executable, and run it.

### 1.1 What is Bash?

- Bash = **B**ourne **A**gain **SH**ell
- It's both a command-line interpreter and a scripting language
- Default shell on most Linux distros and macOS
- Scripts are plain `.sh` text files executed by the shell

```bash
echo $SHELL          # see your current shell
bash --version       # check bash version
```

### 1.2 Your First Script

**What:** A minimal working script.  
**Why:** Understand the shebang, file permissions, and how to run a script.  
**How:** Create a `.sh` file, add `#!/bin/bash` on line 1, make executable, run.

```bash
#!/bin/bash
# The shebang line tells the OS: "use /bin/bash to run this file"

echo "Hello, World!"
```

```bash
touch hello.sh          # create file
chmod +x hello.sh       # make it executable
./hello.sh              # run it
bash hello.sh           # alternative: run without chmod
```

### 1.3 Variables

**What:** Named containers that hold values (strings, numbers).  
**Why:** Avoid repeating values; make scripts dynamic and reusable.  
**How:** Assign with `=` (no spaces!), access with `$`.

```bash
name="Dhruv"             # string — no spaces around =
count=42                 # number (still stored as string)
echo $name               # access with $
echo "${name}_dev"       # use {} to avoid ambiguity

readonly PI=3.14         # constant — cannot be changed
unset name               # delete a variable

# Environment variables (available to child processes)
export DB_HOST="localhost"
```

> **Rule:** Never put spaces around `=`. `name = "Dhruv"` is a syntax error.

### 1.4 User Input

**What:** Read values typed by the user at runtime.  
**Why:** Makes scripts interactive instead of hardcoded.  
**How:** Use `read` to capture input into a variable.

```bash
read -p "Enter your name: " name          # prompt + read
read -s -p "Enter password: " pass        # -s = silent (no echo)
read -t 10 -p "Answer (10s timeout): " x  # -t = timeout in seconds
read -n 1 -p "Press any key..."           # -n = read N chars only

echo "Hello, $name"
```

### 1.5 Special Variables

**What:** Variables Bash sets automatically.  
**Why:** You'll use these constantly — script name, arguments, exit status, PID.  
**How:** They're always available; just reference them.

```bash
echo $0    # script name
echo $1    # first argument
echo $@    # all arguments (as separate words)
echo $*    # all arguments (as one string)
echo $#    # number of arguments
echo $?    # exit status of last command (0 = success)
echo $$    # current script's PID
echo $!    # PID of last background process
echo $-    # current shell options
```

### 1.6 Comments

```bash
# This is a single-line comment

: '
This is a
multi-line comment block
(actually a no-op command with a string argument)
'
```

---

## Stage 2 — Control Flow (Week 2)

**What:** Making decisions and repeating actions — if/else, case, and loops.  
**Why:** Real scripts need to react to conditions (file exists? user is root?) and repeat tasks (process 100 files).  
**How:** Use `[ ]` or `[[ ]]` for tests, `if/fi` for branching, `for/while/until` for loops.

### 2.1 if / elif / else

**What:** Run different code depending on a condition.  
**Why:** Scripts need to handle different situations — "if backup succeeded, notify; else alert."  
**How:** Wrap condition in `[ ]`, end block with `fi`.

```bash
age=20

if [ $age -ge 18 ]; then
    echo "Adult"
elif [ $age -ge 13 ]; then
    echo "Teenager"
else
    echo "Child"
fi
```

**`[[ ]]` vs `[ ]`** — prefer `[[ ]]` in modern scripts:

```bash
# [[ ]] supports regex, no word-splitting issues, safer with strings
[[ $name == "Dhruv" ]]       # string match
[[ $name =~ ^D.*v$ ]]        # regex match
[[ -f file && -r file ]]     # combine with && instead of -a
```

**Numeric comparison operators:**
| Operator | Meaning |
|----------|---------|
| `-eq` | equal |
| `-ne` | not equal |
| `-gt` | greater than |
| `-lt` | less than |
| `-ge` | greater or equal |
| `-le` | less or equal |

**String comparison operators:**
| Operator | Meaning |
|----------|---------|
| `=` or `==` | equal |
| `!=` | not equal |
| `<` | less than (alphabetical) |
| `>` | greater than (alphabetical) |
| `-z "$var"` | string is empty |
| `-n "$var"` | string is not empty |

**File test operators:**
```bash
[ -f file ]    # is a regular file
[ -d path ]    # is a directory
[ -e path ]    # exists (file or dir)
[ -r file ]    # readable
[ -w file ]    # writable
[ -x file ]    # executable
[ -s file ]    # file exists and is non-empty
[ -L file ]    # is a symbolic link
```

### 2.2 case Statement

**What:** Match a variable against multiple patterns.  
**Why:** Cleaner than 5+ `elif` chains for menu options, day names, file extensions.  
**How:** Match patterns with `)`, end each branch with `;;`.

```bash
day="Mon"

case $day in
    Mon|Tue|Wed|Thu|Fri) echo "Weekday" ;;
    Sat|Sun)             echo "Weekend" ;;
    *)                   echo "Unknown" ;;   # default / catch-all
esac

# Practical example: handle script options
case "$1" in
    start)   echo "Starting service..." ;;
    stop)    echo "Stopping service..." ;;
    restart) echo "Restarting..."       ;;
    *)       echo "Usage: $0 {start|stop|restart}"; exit 1 ;;
esac
```

### 2.3 Loops

**What:** Repeat a block of code multiple times.  
**Why:** Automate repetitive tasks — rename 1000 files, check 50 servers, process every log line.  
**How:** `for` when you know the list; `while` when looping until a condition changes.

**for loop:**
```bash
# Over a list
for i in 1 2 3 4 5; do
    echo "Number: $i"
done

# Over a range
for i in {1..10}; do echo $i; done
for i in {0..20..2}; do echo $i; done   # step by 2

# C-style
for ((i=0; i<5; i++)); do
    echo $i
done

# Over files
for file in /var/log/*.log; do
    echo "Processing: $file"
done

# Over command output
for user in $(cut -d: -f1 /etc/passwd); do
    echo $user
done
```

**while loop:**
```bash
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    ((count++))
done

# Read file line by line (best practice)
while IFS= read -r line; do
    echo "$line"
done < file.txt
```

**until loop:**
```bash
# Opposite of while — runs UNTIL condition becomes true
x=1
until [ $x -gt 5 ]; do
    echo $x
    ((x++))
done
```

**Loop control:**
```bash
break           # exit the loop entirely
continue        # skip rest of this iteration, go to next
break 2         # break out of 2 nested loops
```

### 2.4 select — Interactive Menus

**What:** Auto-generate a numbered menu from a list.  
**Why:** Quick way to build interactive scripts without manually printing options.  
**How:** `select` prints the list, reads the user's choice into the variable.

```bash
select option in "Start" "Stop" "Quit"; do
    case $option in
        Start) echo "Starting..."; break ;;
        Stop)  echo "Stopping..."; break ;;
        Quit)  exit 0 ;;
        *)     echo "Invalid option" ;;
    esac
done
```

---

## Stage 3 — Functions & Arguments (Week 3)

**What:** Reusable blocks of code (functions) and values passed into your script from the terminal (arguments).  
**Why:** Without functions, you copy-paste the same logic everywhere. Arguments let one script handle many different inputs.  
**How:** Define with `name() { }`, call by name, pass data via positional parameters.

### 3.1 Functions

**What:** A named, reusable block of commands.  
**Why:** Write once, call many times. Keeps scripts organized and DRY.  
**How:** Define before calling. Use `$1`, `$2` for arguments passed to the function.

```bash
greet() {
    local name="$1"          # always use local for function vars
    echo "Hello, $name"
}

greet "Dhruv"
greet "World"
```

**Return values:**

> Bash functions don't return values like other languages — they `echo` output and you capture it with `$()`. The `return` statement only sets an exit code (0–255).

```bash
add() {
    echo $(( $1 + $2 ))      # "return" a value via echo
}

result=$(add 3 5)
echo $result                 # 8

# Return codes (for success/failure, not data)
is_even() {
    (( $1 % 2 == 0 ))        # returns 0 (true) if even
}

is_even 4 && echo "even" || echo "odd"
```

### 3.2 Local Variables

**What:** Variables scoped only inside a function.  
**Why:** Prevents functions from accidentally overwriting global variables.  
**How:** Use the `local` keyword inside a function.

```bash
x=100   # global

my_func() {
    local x=10    # local — does not affect global x
    echo "Inside: $x"
}

my_func
echo "Outside: $x"   # still 100
```

### 3.3 Script Arguments

**What:** Values passed to the script when you run it from the terminal.  
**Why:** Makes your script flexible — same script, different behavior based on input.  
**How:** Bash auto-populates `$1`, `$2`, etc. from the command line.

```bash
#!/bin/bash
# Usage: ./deploy.sh production v1.2.3

environment="$1"
version="$2"

echo "Deploying $version to $environment"
```

**Argument parsing with getopts:**
```bash
# Usage: ./script.sh -u dhruv -p 8080
while getopts "u:p:h" opt; do
    case $opt in
        u) user="$OPTARG" ;;
        p) port="$OPTARG" ;;
        h) echo "Usage: $0 -u user -p port"; exit 0 ;;
        *) echo "Unknown option"; exit 1 ;;
    esac
done

echo "User: $user, Port: $port"
```

### 3.4 Default & Validation Patterns

**What:** Handle missing or invalid arguments gracefully.  
**Why:** Scripts should fail with a clear message, not a cryptic error deep in the code.  
**How:** Use parameter expansion for defaults, explicit checks for required args.

```bash
# Default value if argument not provided
name="${1:-World}"
echo "Hello, $name"

# Require an argument
if [ $# -lt 1 ]; then
    echo "Usage: $0 <filename>"
    exit 1
fi

# Validate argument
if [ ! -f "$1" ]; then
    echo "Error: '$1' is not a file"
    exit 1
fi
```
## Stage 4 — Strings & Arrays (Week 4)

**What:** Manipulating text (strings) and storing multiple values (arrays).  
**Why:** Most real scripts deal with text — filenames, log lines, user input. Arrays let you handle lists cleanly.  
**How:** Bash has built-in string operators `${}` and array syntax `()`.

### 4.1 String Operations

**What:** Slice, replace, measure, and transform strings without external tools.  
**Why:** Faster than calling `sed`/`awk` for simple string work; keeps logic inside the script.  
**How:** All done with `${}` parameter expansion syntax.

```bash
str="Hello, World"

# Length
echo ${#str}                  # 13

# Substring: ${var:start:length}
echo ${str:7}                 # World  (from index 7)
echo ${str:7:5}               # World  (5 chars from index 7)
echo ${str: -5}               # World  (last 5 chars)

# Replace
echo ${str/World/Bash}        # Hello, Bash  (first match)
echo ${str//l/L}              # HeLLo, WorLd (all matches)

# Case conversion
echo ${str,,}                 # hello, world  (all lowercase)
echo ${str^^}                 # HELLO, WORLD  (all uppercase)
echo ${str^}                  # Hello, World  (capitalize first)

# Strip prefix/suffix
path="/home/dhruv/file.txt"
echo ${path##*/}              # file.txt       (strip longest prefix up to /)
echo ${path%/*}               # /home/dhruv    (strip shortest suffix from /)
echo ${path##*.}              # txt            (get extension)
echo ${path%.*}               # /home/dhruv/file (strip extension)
```

**Default / fallback patterns:**
```bash
echo ${var:-"default"}        # use "default" if var is unset or empty
echo ${var:="default"}        # assign "default" if var is unset or empty
echo ${var:?"Error: var not set"}  # exit with error if var is unset
echo ${var:+"set"}            # use "set" only if var IS set
```

### 4.2 Arrays

**What:** An ordered list of values stored in one variable.  
**Why:** Instead of `file1`, `file2`, `file3`... use one array and loop over it.  
**How:** Declare with `()`, access elements with `[index]`, use `[@]` for all elements.

```bash
# Declare
fruits=("apple" "banana" "cherry")

# Access
echo ${fruits[0]}             # apple  (0-indexed)
echo ${fruits[-1]}            # cherry (last element)
echo ${fruits[@]}             # all elements
echo ${#fruits[@]}            # length: 3

# Modify
fruits+=("mango")             # append
fruits[1]="blueberry"         # update by index
unset fruits[2]               # remove element (leaves gap)

# Slice: ${array[@]:start:count}
echo ${fruits[@]:1:2}         # elements 1 and 2

# Loop
for fruit in "${fruits[@]}"; do
    echo "$fruit"
done

# Loop with index
for i in "${!fruits[@]}"; do
    echo "$i: ${fruits[$i]}"
done
```

### 4.3 Associative Arrays (dictionaries)

**What:** Key-value pairs, like a dictionary or hashmap.  
**Why:** Store structured data — `config[host]="localhost"` instead of separate variables.  
**How:** Declare with `declare -A`, then use string keys.

```bash
declare -A person
person[name]="Dhruv"
person[age]=25
person[city]="Ahmedabad"

echo ${person[name]}          # Dhruv
echo ${!person[@]}            # all keys: name age city
echo ${person[@]}             # all values

# Loop over key-value pairs
for key in "${!person[@]}"; do
    echo "$key = ${person[$key]}"
done

# Check if key exists
if [[ -v person[name] ]]; then
    echo "name key exists"
fi
```

---

## Stage 5 — Input/Output & Redirection (Week 5)

**What:** Controlling where data flows — into files, between commands, or to/from the terminal.  
**Why:** Scripts need to save output, read files, chain commands, and separate errors from normal output.  
**How:** Use `>`, `>>`, `<`, `|`, and `2>` operators to redirect data streams.

### 5.1 File Descriptors

**What:** Every process has 3 standard streams, identified by numbers.  
**Why:** Understanding FDs lets you redirect exactly the right stream.

| FD | Name | Default destination |
|----|------|-------------------|
| 0 | stdin | keyboard |
| 1 | stdout | terminal |
| 2 | stderr | terminal |

### 5.2 Redirection

**What:** Send stdout/stderr to files, or read stdin from a file.  
**Why:** Save logs, suppress noise, feed data into scripts automatically.  
**How:** `>` overwrites, `>>` appends, `2>` captures errors.

```bash
command > file.txt            # stdout → file (overwrite)
command >> file.txt           # stdout → file (append)
command < file.txt            # stdin ← file
command 2> error.txt          # stderr → file
command 2>&1                  # stderr → wherever stdout goes
command > out.txt 2>&1        # both → file (order matters!)
command &> all.txt            # both → file (shorthand, bash 4+)
command > /dev/null           # discard stdout
command &> /dev/null          # discard everything
```

### 5.3 Pipes

**What:** Send the output of one command directly as input to the next.  
**Why:** Chain small, focused tools to build powerful pipelines without temp files.  
**How:** Use `|` between commands.

```bash
ls -la | grep ".sh"
cat /etc/passwd | cut -d: -f1 | sort | uniq
ps aux | grep nginx | grep -v grep
cat access.log | awk '{print $1}' | sort | uniq -c | sort -rn | head -10
```

### 5.4 Here Document & Here String

**What:** Embed multi-line or single-line input directly in your script.  
**Why:** Pass a block of text to a command without a separate file.  
**How:** `<< EOF` for multi-line, `<<<` for a single string.

```bash
# Here document
cat << EOF
Server: localhost
Port: 8080
EOF

# Suppress tab indentation with <<-
cat <<- EOF
    This line is indented in source
    but leading tabs are stripped
EOF

# Here string — pass a single string as stdin
grep "error" <<< "this is an error message"
base64 <<< "hello"
```

### 5.5 Command Substitution

**What:** Capture the output of a command and use it as a value.  
**Why:** Store dynamic data — today's date, file counts, command results — into variables.  
**How:** Wrap the command in `$()`. Prefer `$()` over backticks.

```bash
today=$(date +%Y-%m-%d)
echo "Today is $today"

file_count=$(ls *.sh | wc -l)
echo "Found $file_count scripts"

# Nested substitution
echo "Oldest file: $(ls -t | tail -1)"
```

### 5.6 Process Substitution

**What:** Treat the output of a command as a file.  
**Why:** Pass command output to tools that expect a filename, not stdin.  
**How:** Use `<(command)` — Bash creates a temporary named pipe.

```bash
# diff two command outputs without temp files
diff <(sort file1.txt) <(sort file2.txt)

# Read from two sources simultaneously
while read line; do
    echo "$line"
done < <(find . -name "*.sh")
```

---

## Stage 6 — Text Processing Tools (Week 6)

**What:** External CLI tools that work alongside Bash to search, filter, and transform text.  
**Why:** Bash alone is weak at text processing. These tools are purpose-built and handle millions of lines efficiently.  
**How:** Pipe data into them or pass a file as an argument.

### 6.1 grep — search text

**What:** Find lines in a file (or stream) that match a pattern.  
**Why:** Instantly filter logs, find errors, search codebases.  
**How:** `grep "pattern" file` — returns matching lines.

```bash
grep "error" logfile.txt           # lines containing "error"
grep -i "error" logfile.txt        # case-insensitive
grep -r "TODO" ./src               # recursive through directories
grep -n "error" logfile.txt        # show line numbers
grep -v "debug" logfile.txt        # invert — exclude matching lines
grep -c "error" logfile.txt        # count matching lines
grep -l "error" *.log              # list files that match
grep -w "error" logfile.txt        # whole word only
grep -A 3 "error" logfile.txt      # 3 lines After match
grep -B 3 "error" logfile.txt      # 3 lines Before match
grep -E "err|warn|crit" log.txt    # extended regex (alternation)
grep -P "\d{3}-\d{4}" file.txt     # Perl-compatible regex
```

### 6.2 sed — stream editor

**What:** Find and replace text in a stream or file, line by line.  
**Why:** Bulk-edit config files, transform log formats, strip characters — without opening an editor.  
**How:** `sed 's/find/replace/flags'` — `s` means substitute.

```bash
sed 's/old/new/' file.txt          # replace first match per line
sed 's/old/new/g' file.txt         # replace all matches
sed 's/old/new/gi' file.txt        # case-insensitive replace all
sed -i 's/old/new/g' file.txt      # edit file in-place
sed -i.bak 's/old/new/g' file.txt  # in-place with backup

sed '3d' file.txt                  # delete line 3
sed '/pattern/d' file.txt          # delete lines matching pattern
sed -n '5,10p' file.txt            # print only lines 5–10
sed -n '/start/,/end/p' file.txt   # print between two patterns

# Add line before/after match
sed '/pattern/i\New line before' file.txt
sed '/pattern/a\New line after' file.txt
```

### 6.3 awk — text processing

**What:** A mini programming language for processing structured text (columns/rows).  
**Why:** Perfect for CSV-like data — extract columns, compute sums, filter rows.  
**How:** `awk '{action}' file` — runs action on every line. `$1`, `$2` are column values.

```bash
awk '{print $1}' file.txt          # print first column
awk '{print $1, $3}' file.txt      # print columns 1 and 3
awk -F: '{print $1}' /etc/passwd   # use : as field delimiter
awk -F, '{print $2}' data.csv      # CSV second column

# Conditions
awk '$3 > 100 {print $1}' file.txt          # rows where col 3 > 100
awk '/error/ {print $0}' logfile.txt        # lines matching pattern
awk 'NR==5' file.txt                        # print line 5
awk 'NR>=5 && NR<=10' file.txt              # print lines 5–10

# Built-in variables
# NR = current line number
# NF = number of fields on current line
# FS = field separator (default: whitespace)
# OFS = output field separator

# Aggregation
awk '{sum += $1} END {print "Total:", sum}' numbers.txt
awk 'END {print NR, "lines"}' file.txt

# BEGIN and END blocks
awk 'BEGIN {print "Start"} {print $1} END {print "Done"}' file.txt
```

### 6.4 cut, sort, uniq, wc, tr

**What:** Small, focused tools for slicing, sorting, deduplicating, counting, and translating.  
**Why:** Each does one thing perfectly — combine with pipes for powerful pipelines.  
**How:** Chain with `|`.

```bash
# cut — extract fields or characters
cut -d: -f1 /etc/passwd            # field 1, delimiter :
cut -d, -f2,4 data.csv             # fields 2 and 4
cut -c1-10 file.txt                # characters 1–10

# sort
sort file.txt                      # alphabetical
sort -n numbers.txt                # numeric
sort -r file.txt                   # reverse
sort -k2 file.txt                  # sort by column 2
sort -t: -k3 -n /etc/passwd        # sort /etc/passwd by UID
sort -u file.txt                   # sort + remove duplicates

# uniq (input must be sorted first)
sort file.txt | uniq               # remove duplicates
sort file.txt | uniq -c            # count occurrences
sort file.txt | uniq -d            # show only duplicates
sort file.txt | uniq -u            # show only unique lines

# wc — word count
wc -l file.txt                     # count lines
wc -w file.txt                     # count words
wc -c file.txt                     # count bytes
wc -m file.txt                     # count characters

# tr — translate/delete characters
echo "hello" | tr 'a-z' 'A-Z'     # uppercase
echo "hello world" | tr -d 'aeiou' # delete vowels
echo "a::b::c" | tr -s ':'        # squeeze repeated chars
cat file.txt | tr '\n' ' '        # replace newlines with spaces
```
## Stage 7 — Error Handling & Debugging (Week 7)

**What:** Making scripts fail safely, report errors clearly, and clean up after themselves.  
**Why:** Scripts run unattended (cron jobs, CI pipelines). A silent failure is worse than a loud one.  
**How:** Use exit codes, `set` options, `trap`, and Bash's built-in debug flags.

### 7.1 Exit Codes

**What:** Every command returns a number when it finishes — `0` = success, non-zero = failure.  
**Why:** This is how you detect whether a command worked and decide what to do next.  
**How:** Check `$?` after a command, or use `&&` / `||` shorthand.

```bash
# Check exit code explicitly
cp file.txt /backup/
if [ $? -ne 0 ]; then
    echo "Copy failed!"
    exit 1
fi

# Inline shorthand
cp file.txt /backup/ || { echo "Copy failed!"; exit 1; }
cp file.txt /backup/ && echo "Copy succeeded"

# Custom exit codes (use 1–125; 126/127/128+ are reserved)
exit 0    # success
exit 1    # general error
exit 2    # misuse of shell command
```

### 7.2 set Options

**What:** Shell options that change how Bash handles errors.  
**Why:** By default, Bash keeps running even after errors — dangerous in production.  
**How:** Add `set -euo pipefail` at the top of every script.

```bash
set -e            # exit immediately if any command fails
set -u            # treat unset variables as errors (not empty string)
set -o pipefail   # catch failures inside pipes (not just last command)
set -x            # print each command before executing (debug trace)
set -v            # print each line as it's read (verbose)

# The standard combo — put this at the top of every script:
set -euo pipefail
```

**When `set -e` doesn't trigger:**
```bash
# These patterns suppress -e:
if failing_command; then ...   # checked in if — won't exit
failing_command || true        # || true suppresses exit
failing_command && next        # only exits if && chain fails
```

### 7.3 trap — cleanup on exit

**What:** Register a function to run automatically when the script exits.  
**Why:** Ensures temp files are deleted, locks released, notifications sent — even if the script crashes.  
**How:** `trap 'command' SIGNAL` — common signals: `EXIT`, `INT`, `TERM`, `ERR`.

```bash
# Basic cleanup
cleanup() {
    echo "Cleaning up..."
    rm -f /tmp/mylock /tmp/tempfile
}
trap cleanup EXIT           # runs on ANY exit (normal or error)
trap cleanup INT TERM       # also runs on Ctrl+C or kill

# Trap ERR — runs when any command fails (with set -e)
on_error() {
    echo "Error on line $LINENO"
}
trap on_error ERR

# Full pattern used in production scripts:
TMPFILE=$(mktemp)
trap 'rm -f "$TMPFILE"' EXIT
```

**Signal reference:**
| Signal | When triggered |
|--------|---------------|
| `EXIT` | Script exits for any reason |
| `INT` | Ctrl+C |
| `TERM` | `kill` command |
| `ERR` | Any command returns non-zero |
| `HUP` | Terminal closed |

### 7.4 Debugging

**What:** Tools to trace what your script is doing, line by line.  
**Why:** When a script misbehaves, you need to see exactly which command ran and what it produced.  
**How:** Use `bash -x` to trace execution, `bash -n` to check syntax without running.

```bash
bash -n script.sh          # syntax check only — no execution
bash -x script.sh          # trace: print each command before running
bash -v script.sh          # verbose: print each line as read
bash -xv script.sh         # both trace and verbose

# Enable/disable trace inside the script
set -x                     # start tracing here
some_command
set +x                     # stop tracing here

# Print line number in error messages
echo "Error at line $LINENO"

# Redirect trace output to a file
exec 2> /tmp/debug.log
set -x
```

**shellcheck — static analysis:**
```bash
sudo apt install shellcheck
shellcheck script.sh       # catches bugs before you run the script
```

---

## Stage 8 — Real-World Scripting (Week 8)

**What:** Applying everything to practical, production-style scripts.  
**Why:** The gap between "works on my machine" and "runs reliably in production" is error handling, logging, and structure.  
**How:** Use the script template below as your starting point for every real script.

### 8.1 Working with Files

**What:** Reading files line by line, checking paths, and finding/processing files in bulk.  
**Why:** Most DevOps scripts deal with files — logs, configs, backups.  
**How:** Use `while read` for line-by-line, `find` for bulk operations.

```bash
# Read file line by line (correct way — preserves whitespace)
while IFS= read -r line; do
    echo "$line"
done < file.txt

# Read into an array
mapfile -t lines < file.txt      # bash 4+
echo "${lines[0]}"               # first line

# Check before acting
[ -f "$file" ] || { echo "File not found: $file"; exit 1; }
[ -d "$dir"  ] || mkdir -p "$dir"
[ -r "$file" ] || { echo "Cannot read: $file"; exit 1; }

# Find and process files
find /var/log -name "*.log" -mtime +7 -exec rm {} \;
find . -name "*.sh" -exec chmod +x {} \;
find . -type f -name "*.txt" | while read -r f; do
    echo "Processing: $f"
done

# Safe file operations
cp -v file.txt /backup/                    # verbose copy
mv -i file.txt newname.txt                 # interactive (ask before overwrite)
rm -i file.txt                             # ask before delete
```

### 8.2 Logging

**What:** Writing timestamped messages to a log file and the terminal.  
**Why:** When a cron job fails at 3 AM, logs are the only way to know what happened.  
**How:** A simple `log()` function that writes to both terminal and file.

```bash
LOG_FILE="/var/log/myscript.log"

log()  { echo "[$(date '+%Y-%m-%d %H:%M:%S')] [INFO]  $*" | tee -a "$LOG_FILE"; }
warn() { echo "[$(date '+%Y-%m-%d %H:%M:%S')] [WARN]  $*" | tee -a "$LOG_FILE" >&2; }
err()  { echo "[$(date '+%Y-%m-%d %H:%M:%S')] [ERROR] $*" | tee -a "$LOG_FILE" >&2; }
die()  { err "$*"; exit 1; }

log "Script started"
warn "Disk usage above 80%"
err "Failed to connect to database"
die "Cannot continue — aborting"   # logs error and exits
```

### 8.3 Scheduling with cron

**What:** Run scripts automatically on a schedule.  
**Why:** Backups, health checks, cleanup jobs — these should run without human intervention.  
**How:** Edit the crontab with `crontab -e`.

```bash
crontab -e      # edit your cron jobs
crontab -l      # list current cron jobs
crontab -r      # remove all cron jobs

# Format: minute hour day-of-month month day-of-week command
# ┌─ minute (0-59)
# │ ┌─ hour (0-23)
# │ │ ┌─ day of month (1-31)
# │ │ │ ┌─ month (1-12)
# │ │ │ │ ┌─ day of week (0-7, 0 and 7 = Sunday)
# │ │ │ │ │
# * * * * * command

30 2 * * *     /scripts/backup.sh          # daily at 2:30 AM
*/5 * * * *    /scripts/healthcheck.sh     # every 5 minutes
0 9 * * 1-5    /scripts/report.sh          # weekdays at 9 AM
0 0 1 * *      /scripts/monthly.sh         # 1st of every month

# Always use full paths in cron — it has a minimal PATH
# Redirect output to avoid email spam:
30 2 * * * /scripts/backup.sh >> /var/log/backup.log 2>&1
```

### 8.4 Production Script Template

**What:** A production-ready skeleton with logging, error handling, and cleanup built in.  
**Why:** Starting from a solid template means you never forget `set -euo pipefail` or a `trap`.  
**How:** Copy this, fill in `main()`, and you're ready.

```bash
#!/bin/bash
set -euo pipefail

# ── constants ──────────────────────────────────────────────────────────
readonly SCRIPT_NAME="$(basename "$0")"
readonly SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
readonly LOG_FILE="/tmp/${SCRIPT_NAME%.sh}.log"
readonly TIMESTAMP="$(date +%Y%m%d_%H%M%S)"

# ── logging ────────────────────────────────────────────────────────────
log()  { echo "[$(date '+%H:%M:%S')] [INFO]  $*" | tee -a "$LOG_FILE"; }
warn() { echo "[$(date '+%H:%M:%S')] [WARN]  $*" | tee -a "$LOG_FILE" >&2; }
err()  { echo "[$(date '+%H:%M:%S')] [ERROR] $*" | tee -a "$LOG_FILE" >&2; }
die()  { err "$*"; exit 1; }

# ── cleanup ────────────────────────────────────────────────────────────
cleanup() {
    local exit_code=$?
    if [ $exit_code -ne 0 ]; then
        err "Script failed with exit code $exit_code"
    fi
    log "Script finished."
    # rm -f /tmp/tempfile   # add cleanup tasks here
}
trap cleanup EXIT

# ── usage ──────────────────────────────────────────────────────────────
usage() {
    cat << EOF
Usage: $SCRIPT_NAME [OPTIONS] <argument>

Options:
  -h    Show this help
  -v    Verbose output

Example:
  $SCRIPT_NAME -v myfile.txt
EOF
    exit 0
}

# ── argument parsing ───────────────────────────────────────────────────
VERBOSE=false
while getopts "hv" opt; do
    case $opt in
        h) usage ;;
        v) VERBOSE=true ;;
        *) die "Unknown option. Use -h for help." ;;
    esac
done
shift $((OPTIND - 1))

# ── main ───────────────────────────────────────────────────────────────
main() {
    log "Script started (PID: $$)"
    [[ $VERBOSE == true ]] && log "Verbose mode enabled"

    # your logic here

    log "Done."
}

main "$@"
```
## Stage 9 — Arithmetic & Math

**What:** Performing numeric calculations inside Bash scripts.  
**Why:** Scripts constantly need counters, percentages, port numbers, file sizes — you need to do math without calling Python.  
**How:** Use `$(( ))` for integer arithmetic, `bc` for decimals.

### 9.1 Integer Arithmetic

**What:** Basic math operations on whole numbers.  
**Why:** Bash only natively handles integers — fast and no dependencies.  
**How:** Wrap expressions in `$(( ))`.

```bash
a=10
b=3

echo $(( a + b ))     # 13  — addition
echo $(( a - b ))     # 7   — subtraction
echo $(( a * b ))     # 30  — multiplication
echo $(( a / b ))     # 3   — integer division (truncates)
echo $(( a % b ))     # 1   — modulo (remainder)
echo $(( a ** b ))    # 1000 — exponentiation

# Increment / decrement
((a++))               # post-increment
((a--))               # post-decrement
((a += 5))            # add 5 to a
((a *= 2))            # multiply a by 2

# Assign result to variable
result=$(( a * b + 10 ))
echo $result
```

### 9.2 Arithmetic in Conditions

**What:** Use math comparisons directly in if statements.  
**Why:** `(( ))` is cleaner than `[ $a -gt $b ]` for numeric tests.  
**How:** `(( expression ))` returns 0 (true) if non-zero, 1 (false) if zero.

```bash
a=10

if (( a > 5 )); then
    echo "greater"
fi

if (( a % 2 == 0 )); then
    echo "even"
fi

# In a loop
for (( i=1; i<=10; i++ )); do
    (( i % 2 == 0 )) && echo "$i is even"
done
```

### 9.3 Floating Point with bc

**What:** Decimal/floating-point arithmetic.  
**Why:** Bash `$(( ))` only does integers — use `bc` for anything with decimals.  
**How:** Pipe the expression to `bc -l` (`-l` loads the math library).

```bash
# Basic decimal math
echo "scale=2; 10 / 3" | bc          # 3.33
echo "scale=4; 22 / 7" | bc          # 3.1428

# Math functions (requires -l flag)
echo "scale=4; sqrt(2)" | bc -l      # 1.4142
echo "scale=4; s(1)" | bc -l         # sin(1 radian)
echo "scale=4; l(10)" | bc -l        # natural log of 10

# Store result
pi=$(echo "scale=10; 4*a(1)" | bc -l)
echo "Pi = $pi"

# Percentage calculation
used=75
total=100
pct=$(echo "scale=1; $used * 100 / $total" | bc)
echo "Usage: $pct%"
```

### 9.4 Useful Math Patterns

```bash
# Random number in a range (min to max)
min=1
max=100
echo $(( RANDOM % (max - min + 1) + min ))

# Check if number is in range
num=42
if (( num >= 1 && num <= 100 )); then
    echo "In range"
fi

# Absolute value
abs() { (( $1 < 0 )) && echo $(( -$1 )) || echo $1; }

# Min / Max of two numbers
max() { (( $1 > $2 )) && echo $1 || echo $2; }
min() { (( $1 < $2 )) && echo $1 || echo $2; }

# Convert bytes to human-readable
bytes=1048576
echo "$(( bytes / 1024 / 1024 )) MB"
```

---

## Stage 10 — Process Management & Signals

**What:** Running commands in the background, managing jobs, and handling OS signals.  
**Why:** Real scripts need to run tasks in parallel, wait for them, kill runaway processes, and respond to Ctrl+C cleanly.  
**How:** Use `&`, `wait`, `jobs`, `kill`, and `trap` for signal handling.

### 10.1 Background Processes

**What:** Run a command without waiting for it to finish.  
**Why:** Run multiple tasks in parallel to speed up scripts.  
**How:** Append `&` to a command.

```bash
# Run in background
sleep 10 &
echo "Sleep is running in background"

# Get PID of last background process
sleep 10 &
bg_pid=$!
echo "PID: $bg_pid"

# Wait for a specific background process
wait $bg_pid
echo "Sleep finished"

# Wait for ALL background processes
task1 &
task2 &
task3 &
wait
echo "All tasks done"
```

### 10.2 Parallel Execution Pattern

**What:** Run multiple tasks simultaneously and collect results.  
**Why:** Process 10 servers in parallel instead of sequentially — 10x faster.  
**How:** Launch with `&`, store PIDs, then `wait` for each.

```bash
#!/bin/bash
set -euo pipefail

pids=()

for server in server1 server2 server3 server4; do
    (
        echo "Checking $server..."
        ping -c 1 "$server" &>/dev/null && echo "$server: UP" || echo "$server: DOWN"
    ) &
    pids+=($!)
done

# Wait for all and check exit codes
for pid in "${pids[@]}"; do
    wait "$pid"
done

echo "All checks complete"
```

### 10.3 Job Control

**What:** Manage foreground and background jobs in the shell.  
**Why:** Pause, resume, and switch between running tasks.  
**How:** Use `jobs`, `fg`, `bg`, and Ctrl+Z.

```bash
jobs            # list all background jobs
fg              # bring last background job to foreground
fg %1           # bring job 1 to foreground
bg              # resume last stopped job in background
bg %2           # resume job 2 in background

# Ctrl+Z        # suspend (pause) the current foreground job
# Ctrl+C        # terminate the current foreground job
```

### 10.4 Signals

**What:** OS messages sent to processes to notify or control them.  
**Why:** You need to handle Ctrl+C gracefully, respond to `kill`, or clean up on termination.  
**How:** Use `trap` to intercept signals; use `kill` to send them.

**Common signals:**
| Signal | Number | Default action | When used |
|--------|--------|---------------|-----------|
| `SIGHUP` | 1 | Terminate | Terminal closed; reload config |
| `SIGINT` | 2 | Terminate | Ctrl+C |
| `SIGQUIT` | 3 | Core dump | Ctrl+\ |
| `SIGKILL` | 9 | Terminate | Force kill — cannot be caught |
| `SIGTERM` | 15 | Terminate | Graceful shutdown request |
| `SIGSTOP` | 19 | Stop | Pause process — cannot be caught |
| `SIGCONT` | 18 | Continue | Resume stopped process |

```bash
# Send signals
kill -15 $pid          # SIGTERM — ask process to stop gracefully
kill -9 $pid           # SIGKILL — force kill (last resort)
kill -1 $pid           # SIGHUP — reload config
kill -0 $pid           # check if process exists (no signal sent)

# Check if process is running
if kill -0 "$pid" 2>/dev/null; then
    echo "Process $pid is running"
fi

# Trap signals in your script
trap 'echo "Caught Ctrl+C — cleaning up..."; exit 1' INT
trap 'echo "Received TERM signal"; exit 0' TERM
trap 'echo "Terminal closed"; exit 1' HUP
```

### 10.5 Process Information

```bash
ps aux                          # all running processes
ps aux | grep nginx             # find a specific process
pgrep nginx                     # get PID(s) of process by name
pkill nginx                     # kill process by name
pidof nginx                     # get PID of running program

# Check if a process is running
if pgrep -x "nginx" > /dev/null; then
    echo "nginx is running"
fi

# Wait for a process to finish (by PID)
wait $pid && echo "Success" || echo "Failed"

# Timeout — kill if takes too long
timeout 30 ./long_script.sh || echo "Timed out after 30s"
```

### 10.6 Subshells

**What:** A child shell that inherits the parent's environment but can't modify it.  
**Why:** Isolate side effects — variable changes, `cd`, `set` options — without affecting the parent.  
**How:** Wrap commands in `( )`.

```bash
# Changes inside () don't affect the parent
(
    cd /tmp
    export TEMP_VAR="hello"
    echo "Inside subshell: $PWD"
)
echo "Outside subshell: $PWD"    # unchanged
echo $TEMP_VAR                   # empty — not exported back

# Useful for temporary directory changes
(cd /some/dir && make build)
```
## Stage 11 — Bash Networking (/dev/tcp)

**What:** Making network connections directly from Bash using the built-in `/dev/tcp` and `/dev/udp` pseudo-devices — no `curl`, `nc`, or any external tool required.  
**Why:** On minimal systems (containers, embedded, rescue environments) you may not have `curl` or `nc`. `/dev/tcp` is always available in Bash. It's also great for quick port checks and lightweight HTTP requests in scripts.  
**How:** Open a connection by redirecting to/from `/dev/tcp/host/port`. Bash handles the TCP socket transparently.

### 11.1 How /dev/tcp Works

**What:** A virtual file path that Bash intercepts — it never exists on disk.  
**Why:** When Bash sees `exec 3<>/dev/tcp/host/port`, it opens a real TCP socket and attaches it to file descriptor 3.  
**How:** Use `exec` to open the socket, then read/write using that file descriptor.

```bash
# Open a TCP connection to host:port on file descriptor 3
exec 3<>/dev/tcp/google.com/80

# Write to the socket (send HTTP request)
echo -e "GET / HTTP/1.0\r\nHost: google.com\r\n\r\n" >&3

# Read the response
cat <&3

# Close the connection
exec 3>&-
```

### 11.2 Port Checking

**What:** Test whether a TCP port is open on a remote host.  
**Why:** Useful in health checks and deployment scripts when `nc` is not available.  
**How:** Try to open `/dev/tcp/host/port` — it fails immediately if the port is closed.

```bash
# Check if a port is open (returns 0 = open, 1 = closed)
is_port_open() {
    local host="$1" port="$2" timeout="${3:-3}"
    (echo > /dev/tcp/"$host"/"$port") &>/dev/null
}

# Usage
if is_port_open "google.com" 443; then
    echo "Port 443 is open"
else
    echo "Port 443 is closed"
fi

# With timeout (bash built-in timeout via subshell)
check_port() {
    local host="$1" port="$2"
    timeout 3 bash -c "echo > /dev/tcp/$host/$port" 2>/dev/null \
        && echo "$host:$port OPEN" \
        || echo "$host:$port CLOSED"
}

check_port localhost 22
check_port localhost 8080
```

### 11.3 Simple HTTP Request

**What:** Send a raw HTTP GET request and read the response.  
**Why:** Fetch a URL, hit an API endpoint, or check a web service — without `curl` or `wget`.  
**How:** Send a valid HTTP/1.0 request over the TCP socket, read until the connection closes.

```bash
http_get() {
    local host="$1"
    local path="${2:-/}"
    local port="${3:-80}"

    exec 3<>/dev/tcp/"$host"/"$port"
    printf "GET %s HTTP/1.0\r\nHost: %s\r\nConnection: close\r\n\r\n" "$path" "$host" >&3
    cat <&3
    exec 3>&-
}

# Get just the HTTP status code
http_status() {
    local host="$1" path="${2:-/}" port="${3:-80}"
    exec 3<>/dev/tcp/"$host"/"$port"
    printf "GET %s HTTP/1.0\r\nHost: %s\r\nConnection: close\r\n\r\n" "$path" "$host" >&3
    read -r status_line <&3
    exec 3>&-
    echo "$status_line"
}

http_status "example.com" "/"
# Output: HTTP/1.0 200 OK
```

### 11.4 Checking Multiple Services

**What:** Loop over a list of host:port pairs and report which are reachable.  
**Why:** A quick service availability check in a deployment or monitoring script.  
**How:** Combine the port check function with an array of services.

```bash
#!/bin/bash
set -euo pipefail

services=(
    "google.com:80"
    "google.com:443"
    "localhost:22"
    "localhost:3306"
)

check_port() {
    local host="$1" port="$2"
    timeout 2 bash -c "echo > /dev/tcp/$host/$port" 2>/dev/null
}

echo "Service Availability Check"
echo "=========================="
for svc in "${services[@]}"; do
    host="${svc%%:*}"
    port="${svc##*:}"
    if check_port "$host" "$port"; then
        printf "%-25s [UP]\n" "$svc"
    else
        printf "%-25s [DOWN]\n" "$svc"
    fi
done
```

### 11.5 Simple TCP Chat / Listener

**What:** Send arbitrary data over a TCP connection.  
**Why:** Useful for sending alerts to a listening service (e.g., a log aggregator, a simple TCP server).  
**How:** Open the socket and write to it.

```bash
# Send a message to a TCP listener (e.g., netcat listening on port 9000)
send_alert() {
    local host="$1" port="$2" message="$3"
    echo "$message" > /dev/tcp/"$host"/"$port"
}

send_alert "localhost" 9000 "ALERT: disk usage above 90% on $(hostname)"
```

### 11.6 UDP with /dev/udp

**What:** Send UDP datagrams from Bash.  
**Why:** Some monitoring systems (e.g., StatsD, syslog) use UDP.  
**How:** Same syntax as `/dev/tcp` but use `/dev/udp`.

```bash
# Send a UDP packet
echo "myapp.requests:1|c" > /dev/udp/localhost/8125   # StatsD metric

# Send a syslog UDP message
echo "<14>$(date) $(hostname) myscript: deployment complete" > /dev/udp/localhost/514
```

### 11.7 Limitations & When NOT to Use /dev/tcp

| Situation | Use instead |
|-----------|-------------|
| HTTPS / TLS connections | `curl`, `openssl s_client` |
| Large file downloads | `curl`, `wget` |
| Complex HTTP (headers, auth, JSON) | `curl` |
| Production-grade networking | `curl`, `nc`, proper tools |
| UDP is unreliable for your use case | TCP with `/dev/tcp` |

> **Rule:** Use `/dev/tcp` for simple port checks and lightweight plaintext TCP. For anything involving TLS, authentication, or complex protocols, use `curl`.

---

## Common Gotchas

These are the mistakes every Bash beginner makes. Read this once, save yourself hours.

### Gotcha 1: Spaces around `=`
```bash
name="Dhruv"    # correct
name = "Dhruv"  # WRONG — bash tries to run a command called "name"
```

### Gotcha 2: Always quote your variables
```bash
file="my file.txt"
rm $file          # WRONG — runs: rm my file.txt (two args!)
rm "$file"        # correct — runs: rm "my file.txt"

# Rule: always use "$var" not $var, especially with filenames
```

### Gotcha 3: `[ ]` vs `[[ ]]`
```bash
# [ ] is POSIX sh — limited, has edge cases
# [[ ]] is bash-only — safer, supports regex, no word splitting

[ $var == "hello" ]     # breaks if $var is empty or has spaces
[[ $var == "hello" ]]   # safe — always use this in bash scripts
```

### Gotcha 4: Integer vs string comparison
```bash
[ "10" -gt "9" ]    # correct — numeric comparison
[ "10" > "9" ]      # WRONG — string comparison, "10" < "9" alphabetically
[[ "10" > "9" ]]    # also wrong for numbers — use (( )) for math
(( 10 > 9 ))        # correct for numbers
```

### Gotcha 5: `set -e` and command substitution
```bash
set -e
count=$(grep -c "error" file.txt)   # if grep finds 0 matches, exits with 1 → script dies!

# Fix: use || true to suppress the exit
count=$(grep -c "error" file.txt || true)
```

### Gotcha 6: Forgetting `IFS=` when reading files
```bash
# WRONG — strips leading/trailing whitespace
while read -r line; do echo "$line"; done < file.txt

# CORRECT — preserves all whitespace
while IFS= read -r line; do echo "$line"; done < file.txt
```

### Gotcha 7: `cd` failure in scripts
```bash
cd /some/dir
rm -rf *          # DANGEROUS if cd fails — deletes current dir!

# Safe pattern:
cd /some/dir || { echo "Cannot cd"; exit 1; }
rm -rf *

# Or with set -e:
set -e
cd /some/dir      # script exits here if it fails
rm -rf *
```

### Gotcha 8: Subshell variable scope
```bash
count=0
cat file.txt | while read line; do
    ((count++))          # this runs in a subshell — count stays 0 outside!
done
echo $count              # prints 0, not the line count

# Fix: use process substitution or redirect
while IFS= read -r line; do
    ((count++))
done < file.txt
echo $count              # correct
```

### Gotcha 9: Unquoted glob expansion
```bash
files="*.txt"
ls $files        # expands glob — might work
ls "$files"      # passes literal "*.txt" — probably not what you want

# For globs, don't quote:
for f in *.txt; do echo "$f"; done    # correct
```

### Gotcha 10: `[[ ]]` with `-eq` vs `==`
```bash
[[ $a -eq $b ]]    # numeric comparison
[[ $a == $b ]]     # string comparison
# Don't mix them up — "10" == "10.0" is false, but 10 -eq 10 is true
```

---

## Troubleshooting Guide

### "command not found"
```bash
# 1. Check if the command exists
which mycommand
type mycommand

# 2. Check PATH
echo $PATH

# 3. In cron — cron has minimal PATH, use full paths
/usr/bin/python3 instead of python3
```

### "Permission denied"
```bash
# Script not executable
chmod +x script.sh

# File not readable
chmod +r file.txt

# Need root
sudo ./script.sh
```

### "bad substitution" or "syntax error"
```bash
# Wrong shebang — using sh instead of bash
#!/bin/sh          # sh doesn't support [[ ]], arrays, etc.
#!/bin/bash        # use this

# Check syntax without running
bash -n script.sh
```

### Variable is empty when it shouldn't be
```bash
# 1. Check if it's set
echo "Value: '${myvar}'"

# 2. Subshell scope issue (see Gotcha 8)
# 3. Typo in variable name
# 4. Variable set in if block that didn't execute

# Debug: print all variables
set | grep myvar
```

### Script works interactively but fails in cron
```bash
# Cron issues:
# 1. Minimal PATH — use full paths (/usr/bin/python3)
# 2. No HOME set — set it explicitly
# 3. No terminal — don't use read -p in cron
# 4. Redirect output: command >> /var/log/out.log 2>&1

# Debug cron: add this to your script
env > /tmp/cron_env.txt    # see what environment cron provides
```

### "unbound variable" with `set -u`
```bash
set -u
echo $undefined_var    # error: unbound variable

# Fix: provide a default
echo "${undefined_var:-}"          # default to empty string
echo "${undefined_var:-default}"   # default to "default"
```

### Pipe not catching errors with `set -e`
```bash
set -e
cat nonexistent.txt | grep "pattern"   # cat fails but grep succeeds — no exit!

# Fix: add pipefail
set -eo pipefail
cat nonexistent.txt | grep "pattern"   # now exits on cat failure
```

### Script runs differently on different machines
```bash
# 1. Check bash version
bash --version

# 2. Associative arrays need bash 4+
declare -A map    # fails on bash 3 (macOS default)

# 3. Use shellcheck to find portability issues
shellcheck -s bash script.sh
```

---

## Practice Projects

Do these in order — each one builds on the previous.

### Project 1: Calculator
```bash
#!/bin/bash
# Practice: variables, arithmetic, input, case

read -p "Enter first number: " a
read -p "Enter operator (+, -, *, /): " op
read -p "Enter second number: " b

case $op in
    +) echo "Result: $(( a + b ))" ;;
    -) echo "Result: $(( a - b ))" ;;
    \*) echo "Result: $(( a * b ))" ;;
    /) 
        (( b == 0 )) && { echo "Error: division by zero"; exit 1; }
        echo "Result: $(echo "scale=4; $a / $b" | bc)"
        ;;
    *) echo "Unknown operator: $op"; exit 1 ;;
esac
```

### Project 2: Number Guessing Game
```bash
#!/bin/bash
# Practice: loops, conditionals, random numbers

secret=$(( RANDOM % 100 + 1 ))
attempts=0
max_attempts=7

echo "Guess the number (1-100). You have $max_attempts tries."

while (( attempts < max_attempts )); do
    read -p "Guess: " guess
    (( attempts++ ))

    if (( guess < secret )); then
        echo "Too low! (${attempts}/${max_attempts})"
    elif (( guess > secret )); then
        echo "Too high! (${attempts}/${max_attempts})"
    else
        echo "Correct! You got it in $attempts tries."
        exit 0
    fi
done

echo "Out of tries! The number was $secret."
exit 1
```

### Project 3: File Organizer
```bash
#!/bin/bash
# Practice: file ops, arrays, loops, find
set -euo pipefail

src="${1:-.}"    # default to current directory

declare -A ext_map=(
    [jpg]="images" [jpeg]="images" [png]="images" [gif]="images"
    [mp4]="videos" [mkv]="videos" [avi]="videos"
    [pdf]="docs"   [docx]="docs"  [txt]="docs"
    [zip]="archives" [tar]="archives" [gz]="archives"
)

moved=0
for file in "$src"/*; do
    [[ -f "$file" ]] || continue
    ext="${file##*.}"
    ext="${ext,,}"    # lowercase

    folder="${ext_map[$ext]:-other}"
    mkdir -p "$src/$folder"
    mv -v "$file" "$src/$folder/"
    (( moved++ ))
done

echo "Organized $moved files."
```

### Project 4: Log Parser
```bash
#!/bin/bash
# Practice: grep, awk, sed, pipes, text processing
set -euo pipefail

log_file="${1:-/var/log/syslog}"
[[ -f "$log_file" ]] || { echo "File not found: $log_file"; exit 1; }

echo "=== Log Analysis: $log_file ==="
echo "Total lines:   $(wc -l < "$log_file")"
echo "Errors:        $(grep -c -i "error" "$log_file" || true)"
echo "Warnings:      $(grep -c -i "warn"  "$log_file" || true)"

echo ""
echo "=== Top 5 Error Sources ==="
grep -i "error" "$log_file" \
    | awk '{print $5}' \
    | sort | uniq -c | sort -rn \
    | head -5

echo ""
echo "=== Last 5 Errors ==="
grep -i "error" "$log_file" | tail -5
```

### Project 5: System Info Script
```bash
#!/bin/bash
# Practice: command substitution, formatting, text processing
set -euo pipefail

bar() {
    local pct=$1 width=30
    local filled=$(( pct * width / 100 ))
    local empty=$(( width - filled ))
    printf "[%s%s] %d%%" "$(printf '#%.0s' $(seq 1 $filled))" \
           "$(printf '.%.0s' $(seq 1 $empty))" "$pct"
}

echo "╔══════════════════════════════╗"
echo "║       SYSTEM INFORMATION     ║"
echo "╚══════════════════════════════╝"
echo "Hostname:  $(hostname)"
echo "OS:        $(grep PRETTY_NAME /etc/os-release | cut -d= -f2 | tr -d '"')"
echo "Kernel:    $(uname -r)"
echo "Uptime:    $(uptime -p)"
echo "Date:      $(date '+%Y-%m-%d %H:%M:%S')"
echo ""

cpu_idle=$(top -bn1 | grep "Cpu(s)" | awk '{print $8}' | tr -d '%')
cpu_used=$(echo "scale=0; 100 - $cpu_idle" | bc)
echo "CPU:       $(bar $cpu_used)"

mem_total=$(free -m | awk '/^Mem:/{print $2}')
mem_used=$(free -m  | awk '/^Mem:/{print $3}')
mem_pct=$(echo "scale=0; $mem_used * 100 / $mem_total" | bc)
echo "Memory:    $(bar $mem_pct) (${mem_used}MB / ${mem_total}MB)"

disk_pct=$(df / | awk 'NR==2{print $5}' | tr -d '%')
echo "Disk (/):  $(bar $disk_pct)"
```

### Project 6: Backup Script
```bash
#!/bin/bash
# Practice: find, tar, error handling, logging, cron-ready
set -euo pipefail

SOURCE_DIR="${1:-$HOME}"
BACKUP_DIR="${2:-/tmp/backups}"
KEEP_DAYS=7
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="$BACKUP_DIR/backup_$TIMESTAMP.tar.gz"

log() { echo "[$(date '+%H:%M:%S')] $*"; }

mkdir -p "$BACKUP_DIR"

log "Starting backup of $SOURCE_DIR"
tar -czf "$BACKUP_FILE" \
    --exclude="$SOURCE_DIR/.cache" \
    --exclude="$SOURCE_DIR/node_modules" \
    "$SOURCE_DIR" 2>/dev/null

size=$(du -sh "$BACKUP_FILE" | cut -f1)
log "Backup created: $BACKUP_FILE ($size)"

# Remove old backups
deleted=$(find "$BACKUP_DIR" -name "backup_*.tar.gz" -mtime +$KEEP_DAYS -print -delete | wc -l)
log "Removed $deleted old backup(s) older than $KEEP_DAYS days"
```

---

## Quick Reference Card

### Variables & Strings
```bash
var="value"                    # assign
echo "$var"                    # access (always quote)
echo "${var:-default}"         # default if unset
echo "${#var}"                 # string length
echo "${var:2:5}"              # substring (start:length)
echo "${var/old/new}"          # replace first
echo "${var//old/new}"         # replace all
echo "${var,,}"                # lowercase
echo "${var^^}"                # uppercase
echo "${var##*/}"              # strip longest prefix (basename)
echo "${var%/*}"               # strip shortest suffix (dirname)
```

### Arithmetic
```bash
$(( a + b ))                   # add
$(( a - b ))                   # subtract
$(( a * b ))                   # multiply
$(( a / b ))                   # divide (integer)
$(( a % b ))                   # modulo
$(( a ** b ))                  # power
(( a++ ))                      # increment
echo "scale=2; $a / $b" | bc   # decimal division
echo $(( RANDOM % 100 ))       # random 0–99
```

### Tests
```bash
[[ -f file ]]                  # is a file
[[ -d dir ]]                   # is a directory
[[ -e path ]]                  # exists
[[ -z "$var" ]]                # string is empty
[[ -n "$var" ]]                # string is not empty
[[ "$a" == "$b" ]]             # strings equal
(( a == b ))                   # numbers equal
(( a > b ))                    # number greater than
```

### Arrays
```bash
arr=("a" "b" "c")              # declare
${arr[0]}                      # first element
${arr[-1]}                     # last element
${arr[@]}                      # all elements
${#arr[@]}                     # length
arr+=("d")                     # append
unset arr[1]                   # remove element
for x in "${arr[@]}"; do ...   # loop
for i in "${!arr[@]}"; do ...  # loop with index
```

### I/O
```bash
cmd > file                     # stdout to file (overwrite)
cmd >> file                    # stdout to file (append)
cmd 2> file                    # stderr to file
cmd &> file                    # both to file
cmd1 | cmd2                    # pipe stdout to next command
result=$(cmd)                  # capture output
read -p "Prompt: " var         # read user input
```

### Control Flow
```bash
if [[ condition ]]; then ... fi
for i in list; do ... done
for ((i=0; i<n; i++)); do ... done
while [[ condition ]]; do ... done
case $var in pattern) ... ;; esac
```

### Functions
```bash
myfunc() {
    local var="$1"             # always use local
    echo "result"              # "return" via echo
}
result=$(myfunc arg)           # capture return value
```

### Error Handling
```bash
set -euo pipefail              # strict mode — top of every script
cmd || { echo "failed"; exit 1; }
trap 'cleanup' EXIT            # always run cleanup
$?                             # exit code of last command
```

### Useful One-Liners
```bash
# Check if command exists
command -v git &>/dev/null && echo "installed"

# Current date/time
date '+%Y-%m-%d %H:%M:%S'

# Count lines in file
wc -l < file.txt

# Get script's own directory
dir="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

# Confirm before proceeding
read -p "Are you sure? [y/N] " ans
[[ "$ans" =~ ^[Yy]$ ]] || exit 0

# Retry a command up to N times
for i in {1..3}; do cmd && break || sleep 2; done

# Run command with timeout
timeout 30 ./script.sh || echo "Timed out"

# Lock file — prevent duplicate runs
exec 9>/tmp/mylock
flock -n 9 || { echo "Already running"; exit 1; }
```

---

## Recommended Resources

- **Man pages:** `man bash` — the complete reference (search with `/term`)
- **Linter:** [shellcheck.net](https://www.shellcheck.net) — catches bugs before you run
- **Book:** *The Linux Command Line* by William Shotts — [free online](https://linuxcommand.org/tlcl.php)
- **Practice:** [HackerRank Shell](https://www.hackerrank.com/domains/shell)
- **Reference:** [Bash Hackers Wiki](https://wiki.bash-hackers.org)

```bash
# Install shellcheck locally
sudo apt install shellcheck
shellcheck script.sh
```

> **Learning tip:** After each stage, write a small script that uses only what you just learned. The best way to remember Bash is to use it — automate something you do manually every day.
