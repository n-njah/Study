# Bash Shell Scripting Study Guide

## Table of Contents

1. [Introduction to Bash](#1-introduction-to-bash)
2. [Getting Started](#2-getting-started)
3. [Basic Syntax and Concepts](#3-basic-syntax-and-concepts)
4. [Variables and Data Types](#4-variables-and-data-types)
5. [Input/Output Operations](#5-inputoutput-operations)
6. [Operators](#6-operators)
7. [Control Structures](#7-control-structures)
8. [Functions](#8-functions)
9. [Arrays](#9-arrays)
10. [String Manipulation](#10-string-manipulation)
11. [File Operations](#11-file-operations)
12. [Advanced Topics](#12-advanced-topics)
13. [Best Practices](#13-best-practices)
14. [Common Pitfalls](#14-common-pitfalls)
15. [Practice Exercises](#15-practice-exercises)
16. [Quick Reference Commands](#16-quick-reference-commands)

---

## 1. Introduction to Bash

### What is Bash?

Bash (Bourne Again SHell) is a command-line interpreter and scripting language for Unix-based systems. It's the default shell on most Linux distributions and macOS.

### Why Learn Bash?

- Automate repetitive tasks
- System administration and configuration
- Process text and data files
- Schedule jobs and manage workflows
- Interact with the operating system efficiently

---

## 2. Getting Started

### Creating Your First Script

Create a file called `hello.sh`:

```bash
#!/bin/bash

# This is a comment

echo "Hello, World!"
```

### The Shebang (`#!/bin/bash`)

The first line tells the system which interpreter to use. Always include this at the top of your scripts.

### Making Scripts Executable

```bash
chmod +x hello.sh
./hello.sh
```

### Running Scripts

```bash
bash hello.sh       # Method 1
./hello.sh          # Method 2 (requires execute permission)
source hello.sh     # Method 3 (runs in current shell)
```

---

## 3. Basic Syntax and Concepts

### Comments

```bash
# Single line comment

: '
Multi-line comment
Everything here is ignored
'
```

### Whitespace

Bash is sensitive to whitespace in certain contexts:

```bash
# Correct
x=5

# Wrong (spaces around =)
x = 5
```

### Command Substitution

```bash
current_date=$(date)
files_count=`ls | wc -l`   # Old style, avoid
echo "Today is $current_date"
```

### Exit Status

Every command returns an exit code (0 = success, non-zero = failure):

```bash
ls /valid/path
echo $?    # Prints 0 if successful

ls /invalid/path
echo $?    # Prints non-zero if failed
```

---

## 4. Variables and Data Types

### Declaring Variables

```bash
name="John"
age=30
price=19.99        # Bash treats this as a string
readonly PI=3.14159  # Cannot be changed
```

### Variable Naming Rules

- Start with letter or underscore
- Can contain letters, numbers, underscores
- Case-sensitive
- No spaces around `=`

### Using Variables

```bash
echo $name
echo ${name}               # Preferred for clarity
echo "My name is $name"
echo 'My name is $name'    # Single quotes: no expansion
```

### Special Variables

| Variable | Meaning |
|----------|---------|
| `$0`     | Script name |
| `$1-$9`  | Positional parameters (arguments) |
| `$#`     | Number of arguments |
| `$@`     | All arguments (as separate strings) |
| `$*`     | All arguments (as single string) |
| `$?`     | Exit status of last command |
| `$$`     | Process ID of current script |
| `$!`     | Process ID of last background command |

### Environment Variables

```bash
echo $HOME
echo $PATH
echo $USER
export MY_VAR="value"    # Make available to child processes
```

---

## 5. Input/Output Operations

### Reading User Input

```bash
read name
echo "Hello, $name"

read -p "Enter your age: " age
read -s password          # Silent input (for passwords)
read -t 5 response        # Timeout after 5 seconds
```

### Echo Command

```bash
echo "Text"
echo -n "No newline"
echo -e "Line1\nLine2"    # Enable escape sequences
```

### Printf Command (more control than echo)

```bash
printf "Name: %s, Age: %d\n" "Alice" 30
printf "%.2f\n" 3.14159   # 2 decimal places
```

### Redirecting Output

```bash
echo "text" > file.txt        # Overwrite file
echo "more" >> file.txt       # Append to file
command 2> error.log          # Redirect stderr
command &> all.log            # Redirect both stdout and stderr
command > /dev/null 2>&1      # Suppress all output
```

### Reading Files

```bash
# Method 1: while read
while read line; do
    echo $line
done < input.txt

# Method 2: using cat
cat file.txt | while read line; do
    echo $line
done
```

---

## 6. Operators

### Arithmetic Operators

```bash
# Using (( )) for arithmetic (preferred)
((result = 5 + 3))
((x++))
((y = x * 2))

# Using let
let "result = 5 + 3"

# Using expr (old style)
result=$(expr 5 + 3)

# Using $[ ] (deprecated)
result=$[5 + 3]

# Using $(( )) (recommended)
result=$((5 + 3))

echo $((10 / 3))    # Integer division = 3
```

### Arithmetic Operations

| Operator | Meaning |
|----------|---------|
| `+`      | Addition |
| `-`      | Subtraction |
| `*`      | Multiplication |
| `/`      | Division |
| `%`      | Modulus |
| `**`     | Exponentiation |

### Comparison Operators (Numeric)

| Operator | Meaning |
|----------|---------|
| `-eq`    | Equal to |
| `-ne`    | Not equal to |
| `-gt`    | Greater than |
| `-ge`    | Greater than or equal to |
| `-lt`    | Less than |
| `-le`    | Less than or equal to |

### String Comparison

| Operator | Meaning |
|----------|---------|
| `=`      | Equal |
| `!=`     | Not equal |
| `<`      | Less than (alphabetically) |
| `>`      | Greater than (alphabetically) |
| `-z`     | String is null (zero length) |
| `-n`     | String is not null |

### Logical Operators

| Operator | Meaning |
|----------|---------|
| `&&`     | AND |
| `||`     | OR |
| `!`      | NOT |

### File Test Operators

| Operator | Meaning |
|----------|---------|
| `-e file` | File exists |
| `-f file` | Is a regular file |
| `-d file` | Is a directory |
| `-r file` | Is readable |
| `-w file` | Is writable |
| `-x file` | Is executable |
| `-s file` | File size is greater than zero |
| `-L file` | Is a symbolic link |

---

## 7. Control Structures

### If-Else Statements

```bash
if [ condition ]; then
    # code
elif [ condition ]; then
    # code
else
    # code
fi

# Example
if [ $age -ge 18 ]; then
    echo "Adult"
else
    echo "Minor"
fi

# Multiple conditions
if [ $x -gt 5 ] && [ $x -lt 10 ]; then
    echo "Between 5 and 10"
fi

# Using [[ ]] (recommended, more features)
if [[ $name == "John" ]]; then
    echo "Hello John"
fi
```

### Case Statements

```bash
case $variable in
    pattern1)
        # code
        ;;
    pattern2)
        # code
        ;;
    *)
        # default case
        ;;
esac

# Example
case $fruit in
    apple)
        echo "Red fruit"
        ;;
    banana|mango)
        echo "Yellow fruit"
        ;;
    *)
        echo "Unknown fruit"
        ;;
esac
```

### For Loops

```bash
# C-style for loop
for ((i=0; i<5; i++)); do
    echo $i
done

# For-in loop
for item in apple banana cherry; do
    echo $item
done

# Looping through files
for file in *.txt; do
    echo "Processing $file"
done

# Range
for i in {1..5}; do
    echo $i
done

# With step
for i in {0..10..2}; do
    echo $i
done
```

### While Loops

```bash
counter=0
while [ $counter -lt 5 ]; do
    echo $counter
    ((counter++))
done

# Reading file line by line
while read line; do
    echo $line
done < file.txt

# Infinite loop
while true; do
    echo "Press Ctrl+C to stop"
    sleep 1
done
```

### Until Loops

```bash
counter=0
until [ $counter -ge 5 ]; do
    echo $counter
    ((counter++))
done
```

### Loop Control

```bash
# Break - exit loop
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        break
    fi
    echo $i
done

# Continue - skip to next iteration
for i in {1..5}; do
    if [ $i -eq 3 ]; then
        continue
    fi
    echo $i
done
```

---

## 8. Functions

### Defining Functions

```bash
# Method 1
function greet() {
    echo "Hello, $1!"
}

# Method 2
greet() {
    echo "Hello, $1!"
}

# Calling function
greet "Alice"
```

### Function Parameters

```bash
add() {
    local result=$(( $1 + $2 ))
    echo $result
}

sum=$(add 5 3)
echo "Sum: $sum"
```

### Return Values

```bash
is_even() {
    if [ $(($1 % 2)) -eq 0 ]; then
        return 0    # true
    else
        return 1    # false
    fi
}

if is_even 4; then
    echo "Even"
fi
```

### Local Variables

```bash
my_function() {
    local local_var="I'm local"
    global_var="I'm global"
}

my_function
echo $global_var     # Accessible
echo $local_var      # Empty
```

---

## 9. Arrays

### Declaring Arrays

```bash
# Method 1
fruits=("apple" "banana" "cherry")

# Method 2
fruits[0]="apple"
fruits[1]="banana"

# Method 3
declare -a numbers=(1 2 3 4 5)
```

### Accessing Array Elements

```bash
echo ${fruits[0]}    # First element
echo ${fruits[@]}    # All elements
echo ${fruits[*]}    # All elements (different quoting behavior)
echo ${#fruits[@]}   # Array length
echo ${!fruits[@]}   # Array indices
```

### Array Operations

```bash
# Append
fruits+=("date")

# Slice
echo ${fruits[@]:1:2}    # Elements 1-2

# Loop through array
for fruit in "${fruits[@]}"; do
    echo $fruit
done

# With indices
for i in "${!fruits[@]}"; do
    echo "Index $i: ${fruits[$i]}"
done
```

### Associative Arrays (Bash 4+)

```bash
declare -A person
person[name]="John"
person[age]=30
echo ${person[name]}
echo ${!person[@]}    # Keys
echo ${person[@]}     # Values
```

---

## 10. String Manipulation

### String Length

```bash
str="Hello World"
echo ${#str}    # 11
```

### Substring Extraction

```bash
str="Hello World"
echo ${str:0:5}     # Hello
echo ${str:6}       # World
echo ${str: -5}     # World (note the space before -)
```

### String Replacement

```bash
str="Hello World World"
echo ${str/World/Earth}     # Replace first occurrence
echo ${str//World/Earth}    # Replace all occurrences
echo ${str/#Hello/Hi}       # Replace at beginning
echo ${str/%World/Earth}    # Replace at end
```

### String Removal

```bash
filename="archive.tar.gz"
echo ${filename%.gz}      # archive.tar (remove shortest match from end)
echo ${filename%.*}       # archive.tar (remove shortest match from end)
echo ${filename%%.*}      # archive (remove longest match from end)
echo ${filename#*.}       # tar.gz (remove shortest match from beginning)
echo ${filename##*.}      # gz (remove longest match from beginning)
```

### Case Conversion

```bash
str="Hello World"
echo ${str,,}         # hello world (lowercase all)
echo ${str^^}         # HELLO WORLD (uppercase all)
echo ${str,}          # hello World (lowercase first char)
echo ${str^}          # Hello World (uppercase first char)
```

### String Concatenation

```bash
first="Hello"
second="World"
combined="$first $second"
combined="${first}${second}"
```

---

## 11. File Operations

### Checking File Existence

```bash
if [ -f "file.txt" ]; then
    echo "File exists"
fi

if [ -d "directory" ]; then
    echo "Directory exists"
fi
```

### Reading Files

```bash
# Read entire file
content=$(cat file.txt)

# Read line by line
while IFS= read -r line; do
    echo "$line"
done < file.txt
```

### Writing Files

```bash
echo "text" > file.txt      # Overwrite
echo "more" >> file.txt     # Append

# Here document
cat > file.txt << EOF
Line 1
Line 2
Line 3
EOF
```

### File Operations

| Command | Description |
|---------|-------------|
| `cp source.txt dest.txt` | Copy |
| `mv old.txt new.txt` | Move/rename |
| `rm file.txt` | Delete |
| `mkdir directory` | Create directory |
| `rmdir directory` | Remove empty directory |
| `rm -r directory` | Remove directory and contents |

### Finding Files

```bash
find . -name "*.txt"              # Find by name
find . -type f -mtime -7          # Modified in last 7 days
find . -type f -size +1M          # Larger than 1MB
```

### File Permissions

```bash
chmod +x script.sh      # Add execute permission
chmod 755 script.sh     # rwxr-xr-x
chmod u+w file.txt      # Add write for user
```

---

## 12. Advanced Topics

### Command-Line Arguments

```bash
#!/bin/bash
echo "Script name: $0"
echo "First arg: $1"
echo "Second arg: $2"
echo "All args: $@"
echo "Number of args: $#"

# Shift arguments
shift    # $2 becomes $1, $3 becomes $2, etc.
```

### Getopts (Parsing Options)

```bash
while getopts "a:b:c" opt; do
    case $opt in
        a) arg_a=$OPTARG ;;
        b) arg_b=$OPTARG ;;
        c) flag_c=true ;;
        \?) echo "Invalid option" ;;
    esac
done

# Usage: script.sh -a value1 -b value2 -c
```

### Process Substitution

```bash
diff <(ls dir1) <(ls dir2)

while read line; do
    echo $line
done < <(command)
```

### Here Documents

```bash
cat << EOF > output.txt
This is line 1
This is line 2
Variable: $var
EOF

# Prevent variable expansion
cat << 'EOF'
This $var won't be expanded
EOF
```

### Pipelines and Subshells

```bash
# Pipeline
ls | grep ".txt" | wc -l

# Subshell
(cd /tmp && ls)
echo $PWD    # Still in original directory
```

### Error Handling

```bash
# Exit on error
set -e

# Exit on undefined variable
set -u

# Pipeline fails if any command fails
set -o pipefail

# Trap errors
trap 'echo "Error on line $LINENO"' ERR

# Cleanup on exit
trap 'rm -f tempfile' EXIT
```

### Regular Expressions

```bash
if [[ $email =~ ^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$ ]]; then
    echo "Valid email"
fi
```

### Debugging

```bash
bash -x script.sh      # Print commands as executed
set -x                 # Enable debugging in script
set +x                 # Disable debugging

# Debug specific section
set -x
# commands to debug
set +x
```

---

## 13. Best Practices

### Script Structure

```bash
#!/bin/bash
# Script: script_name.sh
# Description: What this script does
# Author: Your Name
# Date: YYYY-MM-DD

set -euo pipefail    # Strict mode

# Constants
readonly SCRIPT_DIR=$(dirname "$0")
readonly LOG_FILE="/var/log/script.log"

# Functions
main() {
    # Main logic here
    :
}

# Execute main function
main "$@"
```

### Quoting Variables

```bash
# Always quote variables to handle spaces
rm "$filename"     # Good
rm $filename       # Bad (breaks with spaces)

# Use arrays for multiple items
files=("file 1.txt" "file 2.txt")
rm "${files[@]}"   # Good
```

### Error Checking

```bash
# Check if command exists
if ! command -v git &> /dev/null; then
    echo "git not found"
    exit 1
fi

# Check file operations
if ! mkdir "$dir"; then
    echo "Failed to create directory"
    exit 1
fi
```

### Use `[[ ]]` Instead of `[ ]`

```bash
# [[ ]] is more powerful and safer
if [[ $var == "value" ]]; then    # Good
if [ $var = "value" ]; then       # Works but less safe
```

### Meaningful Exit Codes

| Code | Meaning |
|------|---------|
| 0    | Success |
| 1    | General error |
| 2    | Misuse of command |
| 126  | Command cannot execute |
| 127  | Command not found |
| 130  | Terminated by Ctrl+C |

```bash
exit 0    # Success
exit 1    # Error
```

---

## 14. Common Pitfalls

### Spacing Issues

```bash
# Wrong
if [$x -eq 5]; then    # Missing spaces
x = 5                  # Spaces around =

# Right
if [ $x -eq 5 ]; then
x=5
```

### Unquoted Variables

```bash
# Problematic
file=my file.txt
rm $file    # Tries to remove "my" and "file.txt"

# Correct
file="my file.txt"
rm "$file"
```

### Not Checking Return Values

```bash
# Bad
cd /some/directory
rm -rf *    # Dangerous if cd failed!

# Good
cd /some/directory || exit 1
rm -rf *
```

### Using `cat` Unnecessarily

```bash
# Inefficient
cat file.txt | grep "pattern"

# Better
grep "pattern" file.txt
```

### Incorrect Comparison Operators

```bash
# String comparison
if [ "$str" = "value" ]; then    # Correct for strings

# Numeric comparison
if [ $num -eq 5 ]; then          # Correct for numbers
```

---

## 15. Practice Exercises

### Exercise 1: File Backup Script
Create a script that backs up files from one directory to another with a timestamp.

### Exercise 2: User Management
Write a script that reads usernames from a file and creates user accounts.

### Exercise 3: Log Analyzer
Parse a log file and count occurrences of ERROR, WARNING, and INFO messages.

### Exercise 4: System Monitor
Create a script that monitors CPU and memory usage and sends alerts if thresholds are exceeded.

### Exercise 5: Batch File Renamer
Write a script that renames all files in a directory according to a pattern.

### Exercise 6: Calculator
Build a command-line calculator that performs basic arithmetic operations.

### Exercise 7: Directory Tree
Create a script that displays a directory tree structure.

### Exercise 8: Password Generator
Generate random passwords of specified length with various character types.

---

## 16. Quick Reference Commands

### File Operations
`ls`, `cd`, `pwd`, `mkdir`, `rm`, `cp`, `mv`, `touch`, `cat`, `less`, `head`, `tail`

### Text Processing
`grep`, `sed`, `awk`, `cut`, `sort`, `uniq`, `wc`, `tr`

### System Info
`ps`, `top`, `df`, `du`, `free`, `uname`, `whoami`, `date`

### Permissions
`chmod`, `chown`, `chgrp`

### Network
`ping`, `curl`, `wget`, `ssh`, `scp`

### Compression
`tar`, `gzip`, `zip`, `unzip`

### Process Management
`kill`, `killall`, `jobs`, `fg`, `bg`, `nohup`

---

*Last Updated: October 2026*
*License: Educational use. Free to modify and distribute.*
