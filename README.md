# Bash Scripting — Complete Reference

> This document is your single source of truth for Bash scripting. Every concept, pattern, gotcha, and project you need is here.

---
## Table of Contents

1. [Stage 0 — Shell Introduction](#stage-0--shell-introduction)
2. [Stage 1 — The Basics](#stage-1--the-basics-week-1)
3. [Stage 2 — Control Flow](#stage-2--control-flow-week-2)
4. [Stage 3 — Functions & Arguments](#stage-3--functions--arguments-week-3)
5. [Stage 4 — Strings & Arrays](#stage-4--strings--arrays-week-4)
6. [Stage 5 — Input/Output & Redirection](#stage-5--inputoutput--redirection-week-5)
7. [Stage 6 — Text Processing Tools](#stage-6--text-processing-tools-week-6)
8. [Stage 7 — Error Handling & Debugging](#stage-7--error-handling--debugging-week-7)
9. [Stage 8 — File Permissions & Ownership](#stage-8--file-permissions--ownership)
10. [Stage 9 — User Management](#stage-9--user-management)
11. [Stage 10 — Real-World Scripting](#stage-10--real-world-scripting-week-8)
12. [Stage 11 — Arithmetic & Math](#stage-11--arithmetic--math)
13. [Stage 12 — Process Management & Signals](#stage-12--process-management--signals)
14. [Stage 13 — System Monitoring](#stage-13--system-monitoring)
15. [Stage 14 — Bash Networking (/dev/tcp)](#stage-14--bash-networking-devtcp)
16. [Stage 15 — Regular Expressions](#stage-15--regular-expressions)
17. [Stage 16 — Networking Tools](#stage-16--networking-tools)
18. [Stage 17 — Package Management](#stage-17--package-management)
19. [Stage 18 — File Compression](#stage-18--file-compression)
20. [Stage 19 — Text Editors](#stage-19--text-editors)
21. [Common Gotchas](#common-gotchas)
22. [Troubleshooting Guide](#troubleshooting-guide)
23. [Practice Projects](#practice-projects)
24. [Quick Reference Card](#quick-reference-card)

---

## Stage 0 — Shell Introduction

**What:** The foundation before writing a single script — what a shell is, why CLI exists, and how to set up Bash.  
**Why:** Understanding the environment you're working in makes every concept that follows click faster.  
**How:** Read this once as background knowledge.

### 0.1 What is a Shell?

**What:** A shell is a program that accepts commands from you and passes them to the OS kernel to execute.  
**Why:** It's the bridge between you and the operating system.  
**How:** You type a command → shell interprets it → kernel executes it → result comes back.

```
You  →  Shell  →  Kernel  →  Hardware
```

**CLI vs GUI:**
| | CLI | GUI |
|--|-----|-----|
| Input | Keyboard commands | Mouse clicks |
| Speed | Fast for bulk tasks | Slow for repetition |
| Automation | Scriptable | Not scriptable |
| Servers | Always available | Rarely installed |

> **Rule:** Learn CLI. Every server you ever manage will have a shell. Almost none will have a GUI.

### 0.2 What is Scripting?

**What:** A script is a plain text file of shell commands executed in sequence.  
**Why:** Instead of typing 10 commands every day, write them once in a file and run the file.  
**How:** Save commands in a `.sh` file, make it executable, run it.

```bash
# Without scripting — type every time:
cd /var/log
tar -czf backup.tar.gz *.log
mv backup.tar.gz /backup/
echo "Done"

# With scripting — one command:
./backup_logs.sh
```

### 0.3 Popular Shells

**What:** There are many shells — Bash is the most common for scripting.  
**Why:** Knowing the landscape helps you understand why scripts sometimes behave differently on different systems.  
**How:** Check which shell you're using with `echo $SHELL`.

| Shell | Notes |
|-------|-------|
| `bash` | Bourne Again Shell — default on most Linux. **What this guide teaches.** |
| `sh` | Original Bourne Shell — minimal, POSIX-only. Available everywhere. |
| `zsh` | Extended bash — better autocomplete, themes. Default on macOS. |
| `fish` | Friendly Interactive Shell — beginner-friendly, non-POSIX. |
| `dash` | Lightweight sh. Used as `/bin/sh` on Ubuntu for speed. |
| `ksh` | Korn Shell — found on AIX/Solaris. |
| `tcsh` | C Shell variant — mostly legacy. |
| `cmd` | Windows Command Prompt — completely different syntax. |
| `PowerShell` | Microsoft's modern cross-platform shell — object-based. |

```bash
echo $SHELL          # your current shell
cat /etc/shells      # all installed shells on this system
bash --version       # check bash version
```

### 0.4 Setting Up Bash

**What:** How to run Bash scripts — three different ways.  
**Why:** Each method behaves slightly differently; knowing which to use matters.  
**How:** Pick the right method for the situation.

```bash
bash script.sh        # run with bash explicitly — no chmod needed
./script.sh           # run directly — requires chmod +x first
source script.sh      # run in current shell — variables/functions persist after
. script.sh           # same as source (shorthand)
```

**The shebang line — always line 1:**
```bash
#!/bin/bash             # use bash at fixed path
#!/usr/bin/env bash     # portable — finds bash in PATH (preferred)
#!/bin/sh               # POSIX sh — more portable, fewer features
```

**Make a script executable:**
```bash
chmod +x script.sh
./script.sh
```

> **Rule:** Always use `#!/usr/bin/env bash` as your shebang — it works across Linux, macOS, and other Unix systems.

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

### 1.7 Tab Completion

**What:** Press `Tab` to auto-complete commands, filenames, and paths.  
**Why:** Saves typing, prevents typos, and lets you explore available options.  
**How:** Start typing and press `Tab` once (complete) or twice (show options).

```bash
ls /etc/pa<Tab>        # completes to /etc/passwd
git com<Tab>           # completes to git commit
cat /var/log/<Tab><Tab> # lists all files in /var/log/
```

> **Rule:** Use Tab constantly — it's faster and more accurate than typing full names.

### 1.8 Help Commands

**What:** Ways to get documentation for any command without leaving the terminal.  
**Why:** You don't need to Google everything — the answer is built into the system.  
**How:** Three levels of help, from quick to comprehensive.

```bash
man ls              # full manual page (press q to quit, / to search)
man bash            # the complete bash reference
help cd             # help for bash built-in commands
help if             # help for shell keywords
ls --help           # quick usage summary (most commands support this)
type cd             # tells you if a command is built-in, alias, or external
which python3       # shows full path of an external command
```

**Navigating man pages:**
```bash
# Inside man:
/pattern    # search forward
n           # next match
N           # previous match
q           # quit
```

### 1.9 Bash Alias

**What:** A short custom name for a longer command.  
**Why:** Save keystrokes for commands you type constantly.  
**How:** Define with `alias name='command'`. Add to `~/.bashrc` to make permanent.

```bash
# Define aliases
alias ll='ls -la'
alias gs='git status'
alias ..='cd ..'
alias ...='cd ../..'
alias grep='grep --color=auto'
alias mkdir='mkdir -p'

# View all aliases
alias

# Remove an alias
unalias ll

# Make permanent — add to ~/.bashrc:
echo "alias ll='ls -la'" >> ~/.bashrc
source ~/.bashrc        # reload to apply immediately
```

> **Rule:** Don't alias over existing commands with different behavior — it causes confusion in scripts.

### 1.10 Navigate Between Directories

**What:** `pushd` and `popd` let you bookmark directories and jump back.  
**Why:** Faster than `cd -` when switching between more than two directories.  
**How:** `pushd` saves current dir and moves to new one; `popd` returns to saved dir.

```bash
pushd /var/log        # save current dir, go to /var/log
pushd /etc/nginx      # save /var/log, go to /etc/nginx
dirs                  # show directory stack: /etc/nginx /var/log ~
popd                  # back to /var/log
popd                  # back to original dir

# Practical use
pushd /tmp
# do work in /tmp
popd                  # instantly back to where you were
```

### 1.11 Stop Execution

**What:** Ways to interrupt or stop a running command or script.  
**Why:** Essential for stopping runaway processes, infinite loops, or long-running commands.  
**How:** Keyboard shortcuts and signals.

```bash
Ctrl+C    # SIGINT  — interrupt/stop the current foreground process
Ctrl+Z    # SIGTSTP — suspend (pause) the current process
Ctrl+D    # EOF     — signal end of input (exits shell or read loop)
Ctrl+\    # SIGQUIT — quit with core dump (stronger than Ctrl+C)

# After Ctrl+Z:
fg        # resume the suspended process in foreground
bg        # resume it in background
kill %1   # kill the suspended job
```

### 1.12 printf Formatting

**What:** A more powerful alternative to `echo` for formatted output.  
**Why:** `echo` is inconsistent across systems; `printf` is portable and precise.  
**How:** Same syntax as C's `printf` — format string + arguments.

```bash
printf "Hello, %s!\n" "Dhruv"          # Hello, Dhruv!
printf "Number: %d\n" 42               # Number: 42
printf "Float: %.2f\n" 3.14159         # Float: 3.14
printf "Hex: %x\n" 255                 # Hex: ff
printf "%-10s %5d\n" "item" 42         # left-align string, right-align number

# Format specifiers:
# %s  — string
# %d  — integer
# %f  — float
# %x  — hex
# %-Ns — left-align in N-wide field
# %Nd  — right-align integer in N-wide field
# %.Nf — float with N decimal places

# Practical: aligned table output
printf "%-15s %-10s %s\n" "Name" "Age" "City"
printf "%-15s %-10s %s\n" "Dhruv" "25" "Ahmedabad"
printf "%-15s %-10s %s\n" "Alice" "30" "Mumbai"
```

### 1.13 Wildcards (Glob Patterns)

**What:** Special characters that match multiple filenames at once.  
**Why:** Process groups of files without listing them individually.  
**How:** The shell expands wildcards before passing them to the command.

```bash
*           # match any string (including empty)
?           # match exactly one character
[abc]       # match one character from the set
[a-z]       # match one character in the range
[!abc]      # match one character NOT in the set
{a,b,c}     # brace expansion — match each alternative

# Examples
ls *.sh                  # all .sh files
ls file?.txt             # file1.txt, file2.txt, filea.txt ...
ls [0-9]*.log            # files starting with a digit
ls [!.]* 	             # files not starting with dot
cp *.{jpg,png} /images/  # copy all jpg and png files
mkdir {jan,feb,mar}      # create 3 directories at once
touch file{1..5}.txt     # create file1.txt through file5.txt

# Wildcards in scripts — always quote to prevent unexpected expansion
for f in *.log; do
    echo "Processing: $f"
done
```

> **Rule:** Wildcards are expanded by the shell, not the command. `ls *.sh` — the shell finds the files, then passes the list to `ls`.

### 1.14 Viewing Files — less, more, head, tail

**What:** Tools to read file contents without opening an editor.  
**Why:** Quickly inspect logs, configs, and output — especially large files.  
**How:** Each tool has a different use case.

```bash
# less — best for large files (scrollable, searchable)
less file.txt
# Inside less: arrows to scroll, /pattern to search, q to quit, G for end, g for start

# more — older, simpler pager (forward only)
more file.txt
# Space = next page, q = quit

# head — first N lines (default 10)
head file.txt              # first 10 lines
head -n 20 file.txt        # first 20 lines
head -n -5 file.txt        # all lines except last 5

# tail — last N lines (default 10)
tail file.txt              # last 10 lines
tail -n 20 file.txt        # last 20 lines
tail -f file.txt           # follow — live updates (great for logs)
tail -f /var/log/syslog    # watch log in real time

# Combine
head -n 20 file.txt | tail -n 5   # lines 16–20
```

> **Rule:** Use `tail -f` to monitor live logs. Use `less` for reading large files — never `cat` a 100MB log file.

### 1.15 paste, join, split, nl

**What:** Tools for combining, splitting, and numbering text files.  
**Why:** Common in data processing pipelines — merging columns, splitting large files, adding line numbers.  
**How:** Each takes files or stdin.

```bash
# paste — merge lines from multiple files side by side
paste file1.txt file2.txt          # columns separated by tab
paste -d',' file1.txt file2.txt    # use comma as delimiter
paste -s file.txt                  # serial: all lines of file on one line

# Example:
# file1: a        file2: 1
#        b               2
# paste output: a\t1
#               b\t2

# join — join two files on a common field (like SQL JOIN)
# Both files must be sorted on the join field
join file1.txt file2.txt           # join on first field
join -1 2 -2 1 file1.txt file2.txt # join file1 field 2 with file2 field 1
join -t',' file1.csv file2.csv     # comma-delimited

# split — split a large file into smaller pieces
split -l 100 bigfile.txt part_     # 100 lines per file, named part_aa, part_ab...
split -b 1M bigfile.txt chunk_     # split by size (1MB each)
cat part_* > restored.txt          # reassemble

# nl — number lines
nl file.txt                        # number non-empty lines
nl -ba file.txt                    # number all lines including empty
nl -v 0 file.txt                   # start numbering from 0
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

## Stage 8 — File Permissions & Ownership

**What:** Understanding and managing Linux file permissions, ownership, and special permission bits.  
**Why:** Wrong permissions are one of the most common causes of "Permission denied" errors and security vulnerabilities in scripts.  
**How:** Use `chmod`, `chown`, `chgrp`, `umask`, and read the permission bits with `ls -l`.

### 8.1 Understanding Permission Bits

**What:** Every file has 9 permission bits — 3 for owner, 3 for group, 3 for others.  
**Why:** Controls who can read, write, or execute a file.  
**How:** Read the output of `ls -l`.

```bash
ls -l file.txt
# -rwxr-xr-- 1 dhruv devs 1234 May 14 10:00 file.txt
#  ^^^ ^^^ ^^^
#  |   |   └── others: r-- (read only)
#  |   └────── group:  r-x (read + execute)
#  └────────── owner:  rwx (read + write + execute)

# Permission types
# r = read    (4)
# w = write   (2)
# x = execute (1)
```

### 8.2 chmod — Change Permissions

**What:** Modify the permission bits of a file or directory.  
**Why:** Make scripts executable, protect sensitive files, restrict access.  
**How:** Use symbolic (`u+x`) or octal (`755`) notation.

```bash
# Symbolic notation
chmod +x script.sh          # add execute for everyone
chmod u+x script.sh         # add execute for owner only
chmod go-w file.txt         # remove write from group and others
chmod u=rwx,go=r file.txt   # set exact permissions

# Octal notation (most common in scripts)
chmod 755 script.sh         # rwxr-xr-x  (owner: all, others: read+exec)
chmod 644 file.txt          # rw-r--r--  (owner: read+write, others: read)
chmod 600 secret.key        # rw-------  (owner only)
chmod 700 private_dir/      # rwx------  (owner only, directory)

# Recursive
chmod -R 755 /var/www/html  # apply to directory and all contents

# Common permission sets
# 777 — everyone full access (avoid!)
# 755 — standard for executables and public dirs
# 644 — standard for regular files
# 600 — private files (SSH keys, secrets)
# 700 — private directories
```

### 8.3 chown & chgrp — Change Ownership

**What:** Change which user and group owns a file.  
**Why:** Files must be owned by the right user for permissions to work correctly.  
**How:** `chown user:group file` — requires root for changing to another user.

```bash
chown dhruv file.txt              # change owner
chown dhruv:devs file.txt         # change owner and group
chown :devs file.txt              # change group only
chgrp devs file.txt               # change group only (alternative)

chown -R dhruv:devs /var/www/     # recursive
chown --reference=ref.txt file.txt # copy ownership from another file
```

### 8.4 Special Permission Bits

**What:** Three extra bits beyond rwx — setuid, setgid, and sticky bit.  
**Why:** Used for privilege escalation (setuid), shared directories (setgid), and protecting shared dirs (sticky).  
**How:** Set with `chmod` using 4-digit octal or symbolic notation.

```bash
# Setuid (4) — run file as owner, not caller
chmod u+s /usr/bin/passwd     # passwd runs as root regardless of who calls it
chmod 4755 script.sh          # octal: 4 = setuid

# Setgid (2) — files created in dir inherit group
chmod g+s /shared/dir/        # new files get dir's group
chmod 2755 /shared/dir/       # octal: 2 = setgid

# Sticky bit (1) — only owner can delete their files
chmod +t /tmp                 # standard on /tmp
chmod 1777 /tmp               # octal: 1 = sticky

# View special bits in ls -l
ls -l /usr/bin/passwd
# -rwsr-xr-x  (s in owner execute = setuid set)
ls -ld /tmp
# drwxrwxrwt  (t in others execute = sticky set)
```

### 8.5 umask — Default Permissions

**What:** A mask that subtracts permissions from newly created files/dirs.  
**Why:** Controls the default permissions for every new file your script creates.  
**How:** `umask value` — common values: `022` (files: 644, dirs: 755), `077` (files: 600, dirs: 700).

```bash
umask              # show current umask (e.g. 0022)
umask 022          # set umask: new files = 644, new dirs = 755
umask 077          # strict: new files = 600, new dirs = 700

# In a script — set strict umask at the top for security
umask 077
touch secret.txt   # created as 600 automatically
```

### 8.6 Checking Permissions in Scripts

**What:** Test file permissions before acting on a file.  
**Why:** Fail fast with a clear error instead of a cryptic "Permission denied" later.  
**How:** Use file test operators in `[[ ]]`.

```bash
[[ -r "$file" ]] || { echo "Cannot read: $file"; exit 1; }
[[ -w "$file" ]] || { echo "Cannot write: $file"; exit 1; }
[[ -x "$file" ]] || { echo "Cannot execute: $file"; exit 1; }

# Check if running as root
if [[ $EUID -ne 0 ]]; then
    echo "This script must be run as root"
    exit 1
fi

# Get numeric permissions of a file
stat -c "%a" file.txt          # e.g. 644
stat -c "%U:%G" file.txt       # owner:group  e.g. dhruv:devs

# Find files with dangerous permissions
find /var/www -perm -o+w       # world-writable files (security risk)
find /home -perm 777           # files with full open permissions
find / -perm -4000 2>/dev/null # setuid files (potential privilege escalation)
```

---

## Stage 9 — User Management

**What:** Creating, modifying, and managing Linux user accounts and groups from scripts.  
**Why:** Automating user onboarding, auditing accounts, and managing access is a core sysadmin task.  
**How:** Use `useradd`, `usermod`, `userdel`, `groupadd`, `passwd`, and read `/etc/passwd` and `/etc/group`.

### 9.1 Understanding /etc/passwd and /etc/shadow

**What:** The files that store user account information.  
**Why:** Scripts often need to read, parse, or validate user data from these files.  
**How:** `/etc/passwd` is world-readable; `/etc/shadow` stores hashed passwords (root only).

```bash
# /etc/passwd format:
# username:password:UID:GID:comment:home:shell
# dhruv:x:1000:1000:Dhruv Moradiya:/home/dhruv:/bin/bash
#   x = password is in /etc/shadow

# Read all regular users (UID >= 1000)
awk -F: '$3 >= 1000 {print $1, $3, $6, $7}' /etc/passwd

# Check if a user exists
id username &>/dev/null && echo "exists" || echo "not found"
getent passwd username         # more portable than grep on /etc/passwd

# /etc/group format:
# groupname:password:GID:members
cat /etc/group
getent group groupname
```

### 9.2 Creating and Deleting Users

**What:** Add and remove user accounts.  
**Why:** Automate onboarding/offboarding without manual `adduser` prompts.  
**How:** `useradd` (low-level, scriptable) vs `adduser` (interactive, Debian).

```bash
# Create user
useradd username                          # basic — no home dir on some systems
useradd -m username                       # -m = create home directory
useradd -m -s /bin/bash username          # set shell
useradd -m -s /bin/bash -c "Full Name" username   # add comment/full name
useradd -m -u 1500 username              # specify UID
useradd -m -g devs username              # set primary group
useradd -m -G sudo,docker username       # add to supplementary groups

# Set password
echo "username:password" | chpasswd      # set password non-interactively
passwd username                          # interactive password set

# Force password change on first login
chage -d 0 username

# Delete user
userdel username                         # remove user (keep home dir)
userdel -r username                      # remove user AND home directory
```

### 9.3 Modifying Users

**What:** Change user properties after creation.  
**Why:** Update shell, groups, home directory, or lock/unlock accounts.  
**How:** `usermod` — same flags as `useradd`.

```bash
usermod -s /bin/zsh username             # change shell
usermod -aG docker username              # add to group (-a = append, important!)
usermod -G sudo username                 # set supplementary groups (replaces existing!)
usermod -l newname oldname               # rename user
usermod -d /new/home -m username         # move home directory
usermod -L username                      # lock account (disable login)
usermod -U username                      # unlock account
usermod -e 2026-12-31 username           # set account expiry date
```

### 9.4 Group Management

**What:** Create and manage groups.  
**Why:** Groups control shared access to files and resources.  
**How:** `groupadd`, `groupmod`, `groupdel`, `gpasswd`.

```bash
groupadd devs                            # create group
groupadd -g 2000 devs                    # with specific GID
groupmod -n newname devs                 # rename group
groupdel devs                            # delete group

gpasswd -a username devs                 # add user to group
gpasswd -d username devs                 # remove user from group
gpasswd -M user1,user2 devs             # set group members

# View group membership
groups username                          # groups a user belongs to
id username                              # UID, GID, and all groups
getent group devs                        # members of a group
```

### 9.5 User Management Patterns in Scripts

**What:** Safe, idempotent patterns for managing users in automation scripts.  
**Why:** Scripts run repeatedly — they must not fail or duplicate if the user already exists.  
**How:** Always check before creating.

```bash
# Idempotent user creation
create_user() {
    local username="$1"
    if id "$username" &>/dev/null; then
        echo "User $username already exists — skipping"
        return 0
    fi
    useradd -m -s /bin/bash "$username"
    echo "Created user: $username"
}

# Idempotent group membership
add_to_group() {
    local user="$1" group="$2"
    if id -nG "$user" | grep -qw "$group"; then
        echo "$user already in $group — skipping"
        return 0
    fi
    usermod -aG "$group" "$user"
}

# List all users with their last login
lastlog | grep -v "Never logged in"

# Find users with no password set (security audit)
awk -F: '($2 == "" ) {print $1}' /etc/shadow

# Find users with UID 0 (should only be root)
awk -F: '($3 == 0) {print $1}' /etc/passwd
```

---

## Stage 10 — Real-World Scripting (Week 8)

**What:** Applying everything to practical, production-style scripts.  
**Why:** The gap between "works on my machine" and "runs reliably in production" is error handling, logging, and structure.  
**How:** Use the script template below as your starting point for every real script.

### 10.1 Working with Files

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

### 10.2 Logging

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

### 10.3 Scheduling with cron

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

### 10.4 Scheduling with at

**What:** Run a command once at a specific future time.  
**Why:** `cron` is for recurring jobs; `at` is for one-off scheduled tasks.  
**How:** `at TIME` — type commands, then press `Ctrl+D` to submit.

```bash
# Schedule a one-time job
at 10:30                        # opens prompt — type commands, Ctrl+D to save
at 10:30 tomorrow
at now + 2 hours
at 9am next monday
at midnight

# Pass command directly
echo "bash /scripts/backup.sh" | at 2am
at now + 5 minutes <<< "echo 'reminder' | mail -s 'Hey' user@host"

# Manage at jobs
atq                             # list pending jobs
atrm 3                          # remove job number 3
at -l                           # same as atq

# at reads from a file
at 3am < /scripts/nightly.sh
```

> **Rule:** Use `cron` for recurring schedules. Use `at` for "run this once at a specific time."

### 10.5 systemd Timers

**What:** A modern alternative to cron — schedule jobs using systemd units.  
**Why:** More powerful than cron — supports dependencies, logging via journald, and monotonic timers (run X minutes after boot).  
**How:** Create two files: a `.service` unit (what to run) and a `.timer` unit (when to run it).

```bash
# Example: run a backup script daily at 2am

# 1. Create the service unit: /etc/systemd/system/backup.service
[Unit]
Description=Daily Backup

[Service]
Type=oneshot
ExecStart=/scripts/backup.sh

# 2. Create the timer unit: /etc/systemd/system/backup.timer
[Unit]
Description=Run backup daily at 2am

[Timer]
OnCalendar=*-*-* 02:00:00
Persistent=true          # run missed jobs if system was off

[Install]
WantedBy=timers.target

# 3. Enable and start the timer
sudo systemctl daemon-reload
sudo systemctl enable backup.timer
sudo systemctl start backup.timer

# Managing timers
systemctl list-timers           # list all active timers with next run time
systemctl status backup.timer   # check timer status
journalctl -u backup.service    # view logs for the service

# OnCalendar examples:
# daily                  — every day at midnight
# weekly                 — every Monday at midnight
# *-*-* 02:00:00         — every day at 2am
# Mon *-*-* 09:00:00     — every Monday at 9am
# *-*-* *:00:00          — every hour on the hour
# OnBootSec=10min        — 10 minutes after boot (monotonic)
```

> **Rule:** Use systemd timers for new systems — they have better logging, dependency handling, and reliability than cron. Use cron when systemd is not available or for simplicity.

### 10.6 Production Script Template

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
## Stage 11 — Arithmetic & Math

**What:** Performing numeric calculations inside Bash scripts.  
**Why:** Scripts constantly need counters, percentages, port numbers, file sizes — you need to do math without calling Python.  
**How:** Use `$(( ))` for integer arithmetic, `bc` for decimals.

### 11.1 Integer Arithmetic

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

### 11.2 Arithmetic in Conditions

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

### 11.3 Floating Point with bc

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

### 11.3 expr and let

**What:** Older ways to do arithmetic — `expr` is an external command, `let` is a shell built-in.  
**Why:** You'll encounter them in legacy scripts; knowing them helps you read and maintain old code.  
**How:** Prefer `$(( ))` in new scripts — it's faster and cleaner.

```bash
# expr — external command, spaces required around operators
expr 5 + 3          # 8
expr 10 - 4         # 6
expr 6 \* 7         # 42  (must escape * to avoid glob expansion)
expr 10 / 3         # 3   (integer division)
expr 10 % 3         # 1   (modulo)

# Store result
result=$(expr 5 + 3)
echo $result        # 8

# expr for string operations
expr length "hello"          # 5
expr substr "hello" 2 3      # ell  (start at pos 2, length 3)
expr index "hello" "l"       # 3    (position of first match)

# let — shell built-in, no spaces needed, no $ on variables
let a=5+3           # a=8
let "a = 5 + 3"     # same, with spaces (must quote)
let a++             # increment
let a*=2            # multiply in place

# let vs $(( )) — they are equivalent for integers
let result=10*5     # result=50
result=$(( 10 * 5 )) # same thing — prefer this style
```

> **Rule:** Use `$(( ))` in all new scripts. Use `expr` only when you need POSIX portability (`#!/bin/sh` scripts). Use `let` only when you see it in existing code.

### 11.4 Useful Math Patterns

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

## Stage 12 — Process Management & Signals

**What:** Running commands in the background, managing jobs, and handling OS signals.  
**Why:** Real scripts need to run tasks in parallel, wait for them, kill runaway processes, and respond to Ctrl+C cleanly.  
**How:** Use `&`, `wait`, `jobs`, `kill`, and `trap` for signal handling.

### 12.1 Background Processes

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

### 12.2 Parallel Execution Pattern

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

### 12.3 Job Control

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

### 12.4 Signals

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

### 12.5 Process Information

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

### 12.6 nohup and disown

**What:** Run processes that survive after you close the terminal.  
**Why:** Background jobs tied to your shell die when you log out. `nohup` and `disown` detach them.  
**How:** Use `nohup` before starting, or `disown` after.

```bash
# nohup — run command immune to hangup signal (terminal close)
nohup ./long_script.sh &              # runs in background, output → nohup.out
nohup ./long_script.sh > out.log 2>&1 &  # redirect output explicitly

# disown — detach an already-running background job from the shell
./long_script.sh &
disown                # detach last background job
disown %1             # detach job 1
disown -a             # detach all background jobs

# Check it's still running after disown
jobs                  # won't show disowned jobs
ps aux | grep long_script.sh   # but it's still in the process list

# Difference:
# nohup  — set before running, redirects output, immune to SIGHUP
# disown — applied after running, removes from job table
```

> **Rule:** Use `nohup cmd &` when starting long tasks remotely over SSH. Use `disown` when you forgot to use `nohup` and need to detach a running job.

### 12.7 Subshells

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
## Stage 13 — System Monitoring

**What:** Collecting and reporting system metrics — CPU, memory, disk, network, and processes — from Bash scripts.  
**Why:** Monitoring scripts are the backbone of ops work: health checks, alerting, capacity planning, and dashboards.  
**How:** Use standard Linux tools (`top`, `free`, `df`, `vmstat`, `iostat`, `ss`) and parse their output.

### 13.1 CPU Usage

**What:** Measure how much CPU is being used.  
**Why:** Detect runaway processes, capacity issues, or performance degradation.  
**How:** Parse `top`, `/proc/stat`, or `mpstat`.

```bash
# Instant CPU usage (idle %)
cpu_idle=$(top -bn1 | grep "Cpu(s)" | awk '{print $8}' | tr -d '%id,')
cpu_used=$(echo "scale=1; 100 - $cpu_idle" | bc)
echo "CPU used: ${cpu_used}%"

# Using /proc/stat (more accurate, no external tools)
get_cpu_usage() {
    local cpu1 cpu2
    cpu1=($(grep '^cpu ' /proc/stat))
    sleep 1
    cpu2=($(grep '^cpu ' /proc/stat))
    local idle1=${cpu1[4]} idle2=${cpu2[4]}
    local total1=0 total2=0
    for v in "${cpu1[@]:1}"; do (( total1 += v )); done
    for v in "${cpu2[@]:1}"; do (( total2 += v )); done
    local diff_idle=$(( idle2 - idle1 ))
    local diff_total=$(( total2 - total1 ))
    echo "scale=1; 100 * ($diff_total - $diff_idle) / $diff_total" | bc
}

# Load average (1, 5, 15 minute)
cat /proc/loadavg                        # e.g. 0.52 0.41 0.38 2/312 12345
uptime                                   # human-readable with load avg
```

### 13.2 Memory Usage

**What:** Check RAM and swap usage.  
**Why:** Memory exhaustion causes OOM kills and system instability.  
**How:** Parse `free` command output.

```bash
# Memory in MB
mem_total=$(free -m | awk '/^Mem:/{print $2}')
mem_used=$(free -m  | awk '/^Mem:/{print $3}')
mem_free=$(free -m  | awk '/^Mem:/{print $4}')
mem_pct=$(echo "scale=1; $mem_used * 100 / $mem_total" | bc)

echo "Memory: ${mem_used}MB / ${mem_total}MB (${mem_pct}%)"

# Swap usage
swap_total=$(free -m | awk '/^Swap:/{print $2}')
swap_used=$(free -m  | awk '/^Swap:/{print $3}')
echo "Swap: ${swap_used}MB / ${swap_total}MB"

# Detailed memory breakdown
cat /proc/meminfo | grep -E "MemTotal|MemFree|MemAvailable|Cached|SwapTotal|SwapFree"
```

### 13.3 Disk Usage

**What:** Monitor disk space and inode usage.  
**Why:** Full disks crash applications, corrupt databases, and fill logs.  
**How:** Use `df` for space, `du` for directory sizes.

```bash
# Disk usage of all mounted filesystems
df -h                                    # human-readable
df -h /                                  # just root partition

# Get usage percentage as a number
disk_pct=$(df / | awk 'NR==2{print $5}' | tr -d '%')
echo "Disk: ${disk_pct}%"

# Alert if over threshold
THRESHOLD=80
if (( disk_pct > THRESHOLD )); then
    echo "WARNING: Disk usage at ${disk_pct}%" >&2
fi

# Find largest directories
du -sh /var/log/*  | sort -rh | head -10

# Find files larger than 100MB
find / -type f -size +100M -exec ls -lh {} \; 2>/dev/null

# Inode usage (can fill up even when disk space is free)
df -i /
```

### 13.4 Process Monitoring

**What:** List, filter, and monitor running processes.  
**Why:** Find resource hogs, detect zombie processes, monitor specific services.  
**How:** Use `ps`, `top`, `pgrep`, `pidstat`.

```bash
# All processes
ps aux                                   # BSD style
ps -ef                                   # POSIX style

# Top CPU consumers
ps aux --sort=-%cpu | head -10

# Top memory consumers
ps aux --sort=-%mem | head -10

# Find a specific process
pgrep -a nginx                           # PID + full command
ps aux | grep "[n]ginx"                  # grep trick to exclude grep itself

# Process tree
pstree -p                                # show parent-child relationships

# Monitor a process continuously
watch -n 2 'ps aux | grep nginx'

# Check if process is running and get its PID
get_pid() {
    pgrep -x "$1" || { echo "Process '$1' not running"; return 1; }
}
```

### 13.5 Network Monitoring

**What:** Check open ports, active connections, and network interface stats.  
**Why:** Verify services are listening, detect unexpected connections, monitor bandwidth.  
**How:** Use `ss`, `netstat`, `ip`, `ifconfig`.

```bash
# Open ports and listening services
ss -tlnp                                 # TCP listening ports with process names
ss -ulnp                                 # UDP listening ports
netstat -tlnp                            # alternative (older systems)

# Active connections
ss -tnp                                  # all TCP connections
ss -tnp | grep ESTABLISHED               # only established

# Network interface stats
ip -s link                               # bytes sent/received per interface
cat /proc/net/dev                        # raw interface stats

# Check if a specific port is in use
ss -tlnp | grep ":8080"

# Bandwidth usage (requires ifstat or nethogs)
cat /proc/net/dev | awk 'NR>2{print $1, "RX:", $2, "TX:", $10}'
```

### 13.5 iostat and vmstat

**What:** Tools for monitoring disk I/O and virtual memory/CPU statistics over time.  
**Why:** `top` and `free` give a snapshot; `iostat` and `vmstat` show trends and I/O bottlenecks.  
**How:** Run once for a snapshot, or with an interval for continuous monitoring.

```bash
# iostat — CPU and disk I/O statistics
iostat                    # one-time snapshot
iostat 2                  # update every 2 seconds
iostat -x 2               # extended stats (await, util% per disk)
iostat -d /dev/sda 2 5    # disk sda only, every 2s, 5 times

# Key iostat columns:
# tps      — transfers per second
# kB_read/s, kB_wrtn/s — read/write throughput
# await    — average wait time for I/O (ms) — high = bottleneck
# %util    — % time disk was busy — near 100% = saturated

# vmstat — virtual memory, CPU, I/O, processes
vmstat                    # one-time snapshot
vmstat 2                  # update every 2 seconds
vmstat 2 5                # every 2s, 5 times
vmstat -s                 # summary of memory stats

# Key vmstat columns:
# r   — processes waiting to run (high = CPU bottleneck)
# b   — processes in uninterruptible sleep (high = I/O bottleneck)
# si/so — swap in/out (non-zero = memory pressure)
# us/sy/id/wa — CPU: user, system, idle, wait

# Quick health check in a script
check_io() {
    echo "=== Disk I/O ==="
    iostat -x 1 1 | awk 'NR>3 && $1!="" {printf "%-10s await=%-6s util=%s%%\n", $1, $10, $14}'
    echo "=== VM Stats ==="
    vmstat 1 1 | awk 'NR==3 {printf "run=%s block=%s swapIn=%s swapOut=%s cpuWait=%s%%\n", $1,$2,$7,$8,$16}'
}
```

### 13.7 System Monitoring Script

**What:** A complete monitoring script that ties everything together.  
**Why:** A single script you can run manually or from cron to get a full system snapshot.  
**How:** Combine all the above into one formatted report.

```bash
#!/bin/bash
set -euo pipefail

# Progress bar helper
bar() {
    local pct=$1 width=20
    local filled=$(( pct * width / 100 ))
    local empty=$(( width - filled ))
    printf "[%s%s] %3d%%" \
        "$(printf '#%.0s' $(seq 1 $filled 2>/dev/null))" \
        "$(printf '.%.0s' $(seq 1 $empty  2>/dev/null))" \
        "$pct"
}

echo "==============================="
echo "  System Monitor — $(date '+%Y-%m-%d %H:%M:%S')"
echo "==============================="
echo "Host   : $(hostname)"
echo "Uptime : $(uptime -p)"
echo ""

# CPU
cpu_idle=$(top -bn1 | grep "Cpu(s)" | awk '{print $8}' | tr -d '%id,')
cpu_used=$(echo "scale=0; 100 - $cpu_idle" | bc)
echo "CPU    : $(bar "$cpu_used")"

# Memory
mem_total=$(free -m | awk '/^Mem:/{print $2}')
mem_used=$(free -m  | awk '/^Mem:/{print $3}')
mem_pct=$(echo "scale=0; $mem_used * 100 / $mem_total" | bc)
echo "Memory : $(bar "$mem_pct") (${mem_used}/${mem_total} MB)"

# Disk
disk_pct=$(df / | awk 'NR==2{print $5}' | tr -d '%')
echo "Disk   : $(bar "$disk_pct")"

echo ""
echo "--- Top 3 Processes (CPU) ---"
ps aux --sort=-%cpu | awk 'NR>1 && NR<=4 {printf "%-20s %5s%%\n", $11, $3}'

echo ""
echo "--- Listening Ports ---"
ss -tlnp | awk 'NR>1{print $4}' | sort
```

---

## Stage 14 — Bash Networking (/dev/tcp)

**What:** Making network connections directly from Bash using the built-in `/dev/tcp` and `/dev/udp` pseudo-devices — no `curl`, `nc`, or any external tool required.  
**Why:** On minimal systems (containers, embedded, rescue environments) you may not have `curl` or `nc`. `/dev/tcp` is always available in Bash. It's also great for quick port checks and lightweight HTTP requests in scripts.  
**How:** Open a connection by redirecting to/from `/dev/tcp/host/port`. Bash handles the TCP socket transparently.

### 14.1 How /dev/tcp Works

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

### 14.2 Port Checking

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

### 14.3 Simple HTTP Request

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

### 14.4 Checking Multiple Services

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

### 14.5 Simple TCP Chat / Listener

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

### 14.6 UDP with /dev/udp

**What:** Send UDP datagrams from Bash.  
**Why:** Some monitoring systems (e.g., StatsD, syslog) use UDP.  
**How:** Same syntax as `/dev/tcp` but use `/dev/udp`.

```bash
# Send a UDP packet
echo "myapp.requests:1|c" > /dev/udp/localhost/8125   # StatsD metric

# Send a syslog UDP message
echo "<14>$(date) $(hostname) myscript: deployment complete" > /dev/udp/localhost/514
```

### 14.7 Limitations & When NOT to Use /dev/tcp

| Situation | Use instead |
|-----------|-------------|
| HTTPS / TLS connections | `curl`, `openssl s_client` |
| Large file downloads | `curl`, `wget` |
| Complex HTTP (headers, auth, JSON) | `curl` |
| Production-grade networking | `curl`, `nc`, proper tools |
| UDP is unreliable for your use case | TCP with `/dev/tcp` |

> **Rule:** Use `/dev/tcp` for simple port checks and lightweight plaintext TCP. For anything involving TLS, authentication, or complex protocols, use `curl`.

---

## Stage 15 — Regular Expressions

**What:** Patterns for matching text — used with `grep`, `sed`, `awk`, and `[[ =~ ]]`.  
**Why:** Regex is the backbone of text processing in Bash — log parsing, validation, extraction all depend on it.  
**How:** Learn basic syntax first, then extended, then apply with tools.

### 15.1 Basic Regex (BRE)

**What:** The default regex flavor used by `grep` and `sed`.  
**Why:** Available everywhere — no flags needed.  
**How:** Special characters need backslash escaping in BRE.

```bash
# Anchors
^pattern      # match at start of line
pattern$      # match at end of line
^$            # empty line

# Character classes
.             # any single character (except newline)
[abc]         # one character: a, b, or c
[a-z]         # one character in range a–z
[^abc]        # one character NOT a, b, or c
[[:alpha:]]   # any letter (POSIX class)
[[:digit:]]   # any digit
[[:space:]]   # any whitespace
[[:alnum:]]   # any letter or digit

# Quantifiers (BRE — no escaping needed for * but need \+ \?)
*             # 0 or more of previous
\+            # 1 or more (BRE needs backslash)
\?            # 0 or 1   (BRE needs backslash)
\{n\}         # exactly n times
\{n,m\}       # between n and m times

# Grouping and alternation
\(abc\)       # group (BRE needs backslash)
abc\|def      # alternation: abc OR def (BRE)
```

```bash
# Examples with grep (BRE by default)
grep '^error' file.txt          # lines starting with "error"
grep 'error$' file.txt          # lines ending with "error"
grep '^$' file.txt              # empty lines
grep '[0-9]\+' file.txt         # lines with one or more digits
grep 'colou\?r' file.txt        # matches "color" or "colour"
```

### 15.2 Extended Regex (ERE)

**What:** A cleaner regex syntax — no backslashes needed for `+`, `?`, `()`, `|`.  
**Why:** Much more readable than BRE for complex patterns.  
**How:** Use `grep -E`, `sed -E`, or `awk` (which uses ERE natively).

```bash
# ERE — same as BRE but cleaner syntax
+             # 1 or more (no backslash needed)
?             # 0 or 1
{n,m}         # n to m times
(abc)         # group
abc|def       # alternation: abc OR def

# Examples with grep -E
grep -E '^[0-9]+' file.txt           # lines starting with digits
grep -E '(error|warn|crit)' log.txt  # match any of three words
grep -E '^[A-Z][a-z]+$' file.txt     # capitalized single word
grep -E '\b[0-9]{3}-[0-9]{4}\b'      # phone number pattern
grep -E '^.{10,}$' file.txt          # lines with 10+ characters
```

### 15.3 Regex with grep

**What:** Using regex patterns to search files and streams.  
**Why:** The most common use of regex in Bash — filtering logs, finding patterns.  
**How:** `grep 'pattern' file` — BRE by default, `-E` for ERE, `-P` for Perl regex.

```bash
grep 'pattern' file.txt          # basic match (BRE)
grep -E 'pattern' file.txt       # extended regex
grep -P '\d{3}' file.txt         # Perl-compatible regex (PCRE)
grep -i 'error' file.txt         # case-insensitive
grep -v 'debug' file.txt         # invert — lines NOT matching
grep -n 'error' file.txt         # show line numbers
grep -c 'error' file.txt         # count matching lines
grep -o '[0-9]+' file.txt        # print only the matched part
grep -w 'error' file.txt         # whole word match only

# Practical patterns
grep -E '^[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' file.txt  # IP address
grep -E '^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$' file.txt # email
grep -E 'ERROR|WARN|FATAL' app.log                                     # log levels
```

### 15.4 Regex with sed

**What:** Using regex in `sed` for find-and-replace operations.  
**Why:** Transform text in files or streams using patterns, not just literal strings.  
**How:** `sed 's/pattern/replacement/flags'` — use `-E` for ERE.

```bash
# Basic substitution with regex
sed 's/[0-9]\+/NUM/g' file.txt        # replace all numbers with NUM (BRE)
sed -E 's/[0-9]+/NUM/g' file.txt      # same with ERE (cleaner)
sed -E 's/(error|warn)/ALERT/gi' file.txt  # replace error or warn

# Capture groups — reference with \1, \2
sed -E 's/([0-9]{4})-([0-9]{2})-([0-9]{2})/\3\/\2\/\1/' file.txt
# Converts 2026-05-14 → 14/05/2026

# Delete lines matching pattern
sed '/^#/d' file.txt                  # delete comment lines
sed '/^[[:space:]]*$/d' file.txt      # delete blank lines

# Extract with regex
sed -n '/^error/p' file.txt           # print only lines starting with error
```

### 15.5 Regex with awk

**What:** Using regex in `awk` for pattern matching and field processing.  
**Why:** `awk` uses ERE natively and can combine regex with column operations.  
**How:** `/pattern/ { action }` — runs action on lines matching pattern.

```bash
# Match lines
awk '/error/ {print}' file.txt              # lines containing "error"
awk '/^[0-9]/ {print $1}' file.txt         # lines starting with digit, print col 1
awk '!/^#/ {print}' file.txt               # lines NOT starting with #

# Regex on specific fields
awk '$1 ~ /^[0-9]+$/ {print}' file.txt     # col 1 matches digits
awk '$2 !~ /error/ {print}' file.txt       # col 2 does NOT match error

# Capture and transform
awk '{gsub(/[0-9]+/, "NUM"); print}' file.txt   # replace all numbers
awk '{sub(/^[ \t]+/, ""); print}' file.txt      # strip leading whitespace
```

### 15.6 Regex in Bash `[[ =~ ]]`

**What:** Test if a string matches a regex directly in an `if` statement.  
**Why:** Validate input, parse strings — without calling `grep` or `sed`.  
**How:** `[[ string =~ pattern ]]` — uses ERE. Captured groups go into `BASH_REMATCH`.

```bash
# Basic match test
if [[ "hello123" =~ ^[a-z]+[0-9]+$ ]]; then
    echo "matches"
fi

# Validate input
read -p "Enter IP: " ip
if [[ $ip =~ ^([0-9]{1,3}\.){3}[0-9]{1,3}$ ]]; then
    echo "Valid IP"
else
    echo "Invalid IP"
fi

# Capture groups with BASH_REMATCH
date_str="2026-05-14"
if [[ $date_str =~ ^([0-9]{4})-([0-9]{2})-([0-9]{2})$ ]]; then
    echo "Year:  ${BASH_REMATCH[1]}"   # 2026
    echo "Month: ${BASH_REMATCH[2]}"   # 05
    echo "Day:   ${BASH_REMATCH[3]}"   # 14
fi
```

> **Rule:** Don't quote the regex pattern in `[[ =~ ]]` — quoting forces literal string matching, not regex.

---

## Stage 16 — Networking Tools

**What:** Command-line tools for network connectivity, file transfer, and remote access.  
**Why:** Every DevOps/sysadmin task involves networks — checking connectivity, transferring files, logging into remote servers.  
**How:** Each tool has a specific job; combine them in scripts for automation.

### 16.1 ping and curl

**What:** `ping` tests reachability; `curl` transfers data over HTTP/HTTPS and other protocols.  
**Why:** The two most common tools for checking if a host or service is alive.  
**How:** Use `ping` for ICMP checks, `curl` for HTTP checks.

```bash
# ping — test network reachability
ping google.com              # ping until Ctrl+C
ping -c 4 google.com         # send exactly 4 packets
ping -c 1 -W 2 192.168.1.1   # 1 packet, 2s timeout
ping -i 0.5 google.com       # ping every 0.5 seconds

# Check if host is reachable in a script
if ping -c 1 -W 2 "$host" &>/dev/null; then
    echo "$host is UP"
else
    echo "$host is DOWN"
fi

# curl — transfer data (HTTP, HTTPS, FTP, and more)
curl https://example.com                    # fetch a URL (print to stdout)
curl -o file.html https://example.com       # save to file
curl -O https://example.com/file.zip        # save with original filename
curl -s https://example.com                 # silent (no progress bar)
curl -I https://example.com                 # headers only
curl -L https://example.com                 # follow redirects
curl -w "%{http_code}" -o /dev/null -s URL  # get HTTP status code only

# POST request
curl -X POST -H "Content-Type: application/json" \
     -d '{"key":"value"}' https://api.example.com/endpoint

# With authentication
curl -u user:password https://example.com
curl -H "Authorization: Bearer $TOKEN" https://api.example.com
```

### 16.2 wget

**What:** Download files from the web.  
**Why:** Simpler than `curl` for downloading — especially recursive downloads.  
**How:** `wget URL` — saves file to current directory.

```bash
wget https://example.com/file.zip           # download file
wget -O output.zip https://example.com/f    # save with custom name
wget -q https://example.com/file.zip        # quiet (no output)
wget -c https://example.com/bigfile.zip     # resume interrupted download
wget -r -np https://example.com/docs/       # recursive download
wget --limit-rate=1m https://example.com/f  # limit download speed

# Download multiple files from a list
wget -i urls.txt                            # each URL on a separate line

# Check if URL is reachable
wget -q --spider https://example.com && echo "UP" || echo "DOWN"
```

### 16.3 ssh and scp

**What:** `ssh` logs into remote servers; `scp` copies files over SSH.  
**Why:** The standard way to manage remote Linux servers securely.  
**How:** Both use SSH keys or passwords for authentication.

```bash
# ssh — secure shell remote login
ssh user@hostname                    # connect to remote server
ssh user@192.168.1.10                # connect by IP
ssh -p 2222 user@hostname            # custom port
ssh -i ~/.ssh/mykey.pem user@host    # use specific key file
ssh -L 8080:localhost:80 user@host   # local port forwarding

# Run a command remotely without interactive session
ssh user@host "ls /var/log"
ssh user@host "df -h && free -m"

# SSH key setup (do this once)
ssh-keygen -t ed25519 -C "your@email.com"   # generate key pair
ssh-copy-id user@hostname                    # copy public key to server
# After this, ssh user@hostname works without password

# scp — secure copy over SSH
scp file.txt user@host:/remote/path/         # local → remote
scp user@host:/remote/file.txt ./            # remote → local
scp -r ./dir user@host:/remote/path/         # copy directory recursively
scp -P 2222 file.txt user@host:/path/        # custom port
scp user@host1:/file user@host2:/path/       # remote → remote
```

### 16.4 rsync

**What:** Efficiently sync files and directories — only transfers what changed.  
**Why:** Much faster than `scp` for repeated transfers; perfect for backups.  
**How:** `rsync [options] source destination` — works locally or over SSH.

```bash
# Local sync
rsync -av /source/dir/ /dest/dir/           # archive + verbose
rsync -av --delete /source/ /dest/          # mirror (delete files not in source)
rsync -av --dry-run /source/ /dest/         # preview without making changes

# Over SSH
rsync -avz user@host:/remote/dir/ /local/   # -z = compress during transfer
rsync -avz /local/ user@host:/remote/dir/   # push local to remote
rsync -avz -e "ssh -p 2222" /src/ user@host:/dst/  # custom SSH port

# Useful flags
# -a  archive (preserves permissions, timestamps, symlinks)
# -v  verbose
# -z  compress
# -P  show progress + resume partial transfers
# --exclude='*.log'  skip files matching pattern
# --delete  remove files in dest that don't exist in source

rsync -avzP --exclude='node_modules' --exclude='.git' \
      /project/ user@host:/deploy/project/
```

### 16.5 ifconfig and ip

**What:** View and configure network interfaces.  
**Why:** Check your IP address, interface status, and network configuration.  
**How:** `ip` is the modern replacement for `ifconfig` — use `ip` on new systems.

```bash
# ifconfig — older tool (may need net-tools package)
ifconfig                    # show all interfaces
ifconfig eth0               # show specific interface
ifconfig eth0 up            # bring interface up
ifconfig eth0 down          # bring interface down

# ip — modern replacement (iproute2 package)
ip addr                     # show all IP addresses (short: ip a)
ip addr show eth0           # show specific interface
ip link                     # show link layer info
ip route                    # show routing table (short: ip r)
ip route show default       # show default gateway

# Common tasks
ip addr show | grep "inet "          # show all IPv4 addresses
hostname -I                          # quick way to get your IP(s)
ip addr show eth0 | awk '/inet /{print $2}'  # get IP of eth0

# Network stats
ss -s                       # socket summary
ss -tlnp                    # TCP listening ports
netstat -tlnp               # same (older systems)
cat /proc/net/dev           # raw interface stats
```

---

## Stage 17 — Package Management

**What:** Installing, updating, and removing software packages from the command line.  
**Why:** Every script that sets up a server or environment needs to install packages — knowing the right tool for each distro is essential.  
**How:** Different Linux distros use different package managers; the concepts are the same.

### 17.1 apt — Debian/Ubuntu

**What:** The package manager for Debian-based systems (Ubuntu, Mint, Raspberry Pi OS).  
**Why:** Most servers and cloud VMs run Ubuntu — `apt` is what you'll use most.  
**How:** `apt` is the modern frontend; `apt-get` is the older scriptable version.

```bash
# Update package list (always do this first)
sudo apt update

# Upgrade installed packages
sudo apt upgrade -y             # upgrade all
sudo apt full-upgrade -y        # upgrade + handle dependency changes

# Install packages
sudo apt install nginx          # install a package
sudo apt install -y nginx curl  # install multiple, auto-confirm

# Remove packages
sudo apt remove nginx           # remove but keep config files
sudo apt purge nginx            # remove + delete config files
sudo apt autoremove             # remove unused dependencies

# Search and info
apt search nginx                # search for packages
apt show nginx                  # show package details
apt list --installed            # list all installed packages
apt list --installed | grep nginx  # check if specific package is installed

# In scripts — check before installing
if ! command -v nginx &>/dev/null; then
    sudo apt install -y nginx
fi
```

### 17.2 yum and dnf — RHEL/CentOS/Fedora

**What:** Package managers for Red Hat-based systems.  
**Why:** Enterprise servers (RHEL, CentOS, Amazon Linux) use `yum` or `dnf`.  
**How:** `dnf` is the modern replacement for `yum` — same syntax, better dependency resolution.

```bash
# dnf (Fedora, RHEL 8+, CentOS Stream)
sudo dnf update -y              # update all packages
sudo dnf install nginx -y       # install
sudo dnf remove nginx -y        # remove
sudo dnf search nginx           # search
sudo dnf info nginx             # package details
sudo dnf list installed         # list installed packages

# yum (RHEL 7, CentOS 7, Amazon Linux 2)
sudo yum update -y
sudo yum install nginx -y
sudo yum remove nginx -y
sudo yum search nginx
sudo yum info nginx

# Both support the same core commands — dnf is preferred on newer systems
```

### 17.3 brew — macOS

**What:** Homebrew — the unofficial package manager for macOS (and Linux).  
**Why:** macOS has no built-in package manager; Homebrew fills that gap.  
**How:** Install Homebrew first, then use `brew` like `apt`.

```bash
# Install Homebrew (one-time setup)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Common commands
brew update                     # update Homebrew itself
brew upgrade                    # upgrade all installed packages
brew install wget               # install a package
brew uninstall wget             # remove a package
brew search nginx               # search
brew info nginx                 # package details
brew list                       # list installed packages
brew doctor                     # diagnose issues

# Casks — install GUI apps
brew install --cask visual-studio-code
```

### 17.4 Scripting with Package Managers

**What:** Patterns for using package managers safely in scripts.  
**Why:** Scripts that install software must handle different distros and avoid interactive prompts.  
**How:** Detect the distro, use the right manager, always use non-interactive flags.

```bash
# Detect package manager and install
install_package() {
    local pkg="$1"
    if command -v apt &>/dev/null; then
        sudo apt install -y "$pkg"
    elif command -v dnf &>/dev/null; then
        sudo dnf install -y "$pkg"
    elif command -v yum &>/dev/null; then
        sudo yum install -y "$pkg"
    elif command -v brew &>/dev/null; then
        brew install "$pkg"
    else
        echo "No supported package manager found" >&2
        return 1
    fi
}

# Idempotent install — only install if not already present
ensure_installed() {
    local cmd="$1" pkg="${2:-$1}"
    command -v "$cmd" &>/dev/null || install_package "$pkg"
}

ensure_installed curl
ensure_installed git
ensure_installed jq
```

---

## Stage 18 — File Compression

**What:** Tools to compress and archive files — reducing size for storage and transfer.  
**Why:** Logs, backups, and deployments all involve compressed files. Knowing which format to use and how to work with each is essential.  
**How:** `tar` for archiving, `gzip`/`bzip2`/`xz` for compression, `zip` for cross-platform compatibility.

### 18.1 tar — Archive and Compress

**What:** Combines multiple files into one archive, optionally compressed.  
**Why:** The standard tool for backups and distributing files on Linux.  
**How:** `tar [flags] archive.tar.gz files/`

```bash
# Create archives
tar -czf archive.tar.gz dir/        # create gzip-compressed archive
tar -cjf archive.tar.bz2 dir/       # create bzip2-compressed archive
tar -cJf archive.tar.xz dir/        # create xz-compressed archive
tar -cf archive.tar dir/            # create uncompressed archive

# Extract archives
tar -xzf archive.tar.gz             # extract gzip archive
tar -xjf archive.tar.bz2            # extract bzip2 archive
tar -xJf archive.tar.xz             # extract xz archive
tar -xf archive.tar.gz -C /dest/    # extract to specific directory

# List contents without extracting
tar -tzf archive.tar.gz

# Common flags:
# -c  create   -x  extract   -t  list
# -z  gzip     -j  bzip2     -J  xz
# -f  filename -v  verbose   -C  change to directory
```

### 18.2 gzip and gunzip

**What:** Compress/decompress individual files using the gzip format (`.gz`).  
**Why:** The most common compression format on Linux — fast and widely supported.  
**How:** `gzip` replaces the original file with a `.gz` version; `gunzip` reverses it.

```bash
gzip file.txt               # compress → file.txt.gz (original deleted)
gzip -k file.txt            # compress and keep original (-k = keep)
gzip -9 file.txt            # maximum compression
gzip -d file.txt.gz         # decompress (same as gunzip)
gunzip file.txt.gz          # decompress → file.txt

# Compress multiple files
gzip *.log                  # compress all .log files in place

# View compressed file without extracting
zcat file.txt.gz            # like cat for .gz files
zless file.txt.gz           # like less for .gz files
zgrep "error" file.txt.gz   # grep inside .gz file
```

### 18.3 zip and unzip

**What:** Cross-platform archive format — compatible with Windows, macOS, and Linux.  
**Why:** Use `zip` when sharing files with non-Linux users or systems.  
**How:** `zip` creates `.zip` archives; `unzip` extracts them.

```bash
# Create zip archives
zip archive.zip file1 file2         # zip specific files
zip -r archive.zip dir/             # zip directory recursively
zip -9 archive.zip file.txt         # maximum compression
zip -e archive.zip file.txt         # encrypt with password

# Extract zip archives
unzip archive.zip                   # extract to current directory
unzip archive.zip -d /dest/         # extract to specific directory
unzip -l archive.zip                # list contents without extracting
unzip -o archive.zip                # overwrite existing files without asking

# Update existing zip
zip -u archive.zip newfile.txt      # add/update file in existing zip
zip -d archive.zip oldfile.txt      # delete file from zip
```

### 18.4 bzip2 and xz

**What:** Higher-compression alternatives to gzip.  
**Why:** `bzip2` and `xz` produce smaller files than gzip — useful for large archives where size matters more than speed.  
**How:** Same pattern as gzip — compress in place, use flags to keep original.

```bash
# bzip2 — better compression than gzip, slower
bzip2 file.txt              # compress → file.txt.bz2
bzip2 -k file.txt           # keep original
bzip2 -d file.txt.bz2       # decompress
bunzip2 file.txt.bz2        # same as bzip2 -d

bzcat file.txt.bz2          # view without extracting
bzgrep "error" file.txt.bz2 # grep inside .bz2

# xz — best compression ratio, slowest
xz file.txt                 # compress → file.txt.xz
xz -k file.txt              # keep original
xz -d file.txt.xz           # decompress
unxz file.txt.xz            # same as xz -d
xz -9 file.txt              # maximum compression

xzcat file.txt.xz           # view without extracting

# Compression comparison (same file):
# gzip  — fastest,  largest output
# bzip2 — medium speed, medium output
# xz    — slowest,  smallest output
```

> **Rule:** Use `gzip` for speed (logs, temp files). Use `xz` when size matters (distribution packages, long-term archives). Use `zip` when sharing with Windows users.

---

## Stage 19 — Text Editors

**What:** Terminal-based text editors for editing files directly on the server.  
**Why:** When you SSH into a remote server, there's no GUI — you need to edit files in the terminal.  
**How:** `nano` for quick edits (beginner-friendly), `vim` for power users.

### 19.1 nano — Simple Editor

**What:** A straightforward terminal editor — what you see is what you get.  
**Why:** No modes, no learning curve — open a file, edit, save, done.  
**How:** `nano filename` — keyboard shortcuts shown at the bottom of the screen.

```bash
nano file.txt           # open file (creates it if it doesn't exist)
nano +20 file.txt       # open at line 20
nano -l file.txt        # show line numbers
nano -v file.txt        # view-only (read-only mode)
```

**Essential nano shortcuts:**
| Shortcut | Action |
|----------|--------|
| `Ctrl+O` | Save (Write Out) |
| `Ctrl+X` | Exit |
| `Ctrl+K` | Cut current line |
| `Ctrl+U` | Paste (Uncut) |
| `Ctrl+W` | Search |
| `Ctrl+\` | Search and replace |
| `Ctrl+G` | Help |
| `Ctrl+_` | Go to line number |
| `Alt+U`  | Undo |

> **Rule:** Use `nano` when you just need to quickly edit a config file on a server. It's always available and requires zero learning.

### 19.2 vim — Power Editor

**What:** A highly configurable, modal text editor — the most common editor on Linux servers.  
**Why:** `vim` is on virtually every Linux system. Knowing the basics gets you out of trouble when `nano` isn't available.  
**How:** `vim` has modes — you must understand modes to use it.

**The three essential modes:**
| Mode | How to enter | What it does |
|------|-------------|--------------|
| Normal | `Esc` | Navigate, delete, copy — default mode |
| Insert | `i` | Type text |
| Command | `:` | Save, quit, search, replace |

```bash
vim file.txt            # open file
vim +20 file.txt        # open at line 20
vim -R file.txt         # read-only mode
```

**Survival commands — the minimum you need:**
```bash
# Opening and quitting
:q          # quit (fails if unsaved changes)
:q!         # force quit without saving
:w          # save
:wq         # save and quit
:x          # save and quit (same as :wq)
ZZ          # save and quit (normal mode shortcut)

# Entering insert mode
i           # insert before cursor
a           # insert after cursor
o           # open new line below and insert
O           # open new line above and insert

# Navigation (normal mode)
h j k l     # left, down, up, right
w           # jump forward one word
b           # jump backward one word
0           # start of line
$           # end of line
gg          # top of file
G           # bottom of file
:20         # go to line 20

# Editing (normal mode)
dd          # delete (cut) current line
yy          # yank (copy) current line
p           # paste after cursor
u           # undo
Ctrl+r      # redo
x           # delete character under cursor

# Search
/pattern    # search forward
n           # next match
N           # previous match
:%s/old/new/g   # replace all occurrences in file
```

> **Rule:** When you accidentally open vim and can't exit — press `Esc` then type `:q!` and press `Enter`. That always works.

### 19.3 vi vs vim vs emacs

**What:** Understanding the editor landscape.  
**Why:** You'll encounter references to all of these.  
**How:** Know which to use when.

| Editor | Notes |
|--------|-------|
| `vi` | Original visual editor — on every Unix system. Minimal feature set. |
| `vim` | "Vi IMproved" — superset of vi with syntax highlighting, plugins, undo history. |
| `nvim` | Neovim — modern rewrite of vim. Increasingly popular. |
| `emacs` | Completely different paradigm — extensible via Lisp. Steep learning curve. |
| `nano` | Simplest — no modes, shortcuts shown on screen. |

```bash
# Check what's available
which vi vim nano emacs
# On most systems, vi is actually vim in disguise:
vi --version | head -1
```

---

## Common Gotchas
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
