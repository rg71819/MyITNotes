---
layout: post
title:  "Deploying Bash Scripts in Linux"
date:   2025-12-06 11:00:00 -0500
author: Ravi Shankar Gurram
categories: ['Linux']
background: ''
---

# Deploying Bash Scripts

## Learning Objectives

By the end of this lesson, you will be able to:
- Write and execute basic shell scripts
- Use variables, arguments, and exit statuses
- Apply logic statements (if, case) and loops (for, while)
- Create and use functions
- Perform command substitution and basic math

---

## 1. The Basics of Shell Scripting

### Running Multiple Commands

Use a semicolon (`;`) to chain commands on one line:

```bash
$ date ; who
```

### Redirecting Output

| Operator | Purpose |
|---|---|
| `>` | Redirect output to file (overwrite) |
| `>>` | Redirect output to file (append) |
| `<` | Redirect input from file |
| `2>` | Redirect error output |
| `&>` | Redirect both output and errors |

```bash
$ date > today.txt          # overwrite
$ date >> today.txt         # append
```

### Piping Data

The pipe (`|`) sends output of one command as input to another:

```bash
$ ls | sort
```

### Shell Script Format

Every shell script needs:
1. **Shebang line** as the first line: `#!/bin/bash`
2. **Comments** start with `#`
3. **Commands** to execute

```bash
#!/bin/bash
# This script displays the date and who's logged in
date
who
```

### Running a Shell Script

Two requirements:
1. Make it executable: `chmod u+x script.sh`
2. Execute it: `./script.sh` (the `./` tells the shell to look in current directory)

---

## 2. Advanced Shell Scripting

### Displaying Messages

Use `echo` to print text:

```bash
$ echo "This is a test"
```

### Variables

**Environment variables** (set by the system, all uppercase):

| Variable | Purpose |
|---|---|
| `$USER` | Current username |
| `$UID` | User ID number |
| `$HOME` | Home directory path |
| `$HOSTNAME` | System hostname |
| `$PATH` | Command search path |

**User variables** (you create them):
- No spaces around `=`
- Reference with `$`

```bash
days=10
guest=Katie
echo $guest checked in $days days ago
```

### Command Line Arguments

Access arguments passed to your script:

| Variable | Meaning |
|---|---|
| `$0` | Script name |
| `$1`, `$2`, ... `$9` | First, second, ... ninth argument |
| `$#` | Number of arguments |
| `$@` | All arguments |
| `$$` | Process ID of script |

```bash
#!/bin/bash
echo $1 checked in $2 days ago
# Run: ./script.sh Barbara 4
# Output: Barbara checked in 4 days ago
```

### Exit Status

Every command returns an exit code:
- `0` = success
- Non-zero = error
- View with `$?`

```bash
$ exit 120
$ echo $?
120
```

---

## 3. Writing Script Programs

### Command Substitution

Capture command output into a variable. Two formats:

```bash
var1=`date`           # backticks (older style)
var2=$(who)           # $() syntax (preferred)
```

### Performing Math

Use `bc` (calculator) or `$[ ]`:

```bash
var1=$(echo "scale=4; 3.44 / 5" | bc)    # decimal math
result=$[ 5 + 3 ]                        # integer math
```

### Logic Statements

**The `if` statement:**

```bash
if [ $1 -eq $2 ]
then
   echo "Both values are equal!"
fi

if [ $1 -gt $2 ]
then
   echo "First is greater"
fi
```

**Comparison operators (numeric):**

| Operator | Meaning |
|---|---|
| `-eq` | Equal |
| `-ne` | Not equal |
| `-gt` | Greater than |
| `-lt` | Less than |
| `-ge` | Greater than or equal |
| `-le` | Less than or equal |

**File test operators:**

| Operator | Meaning |
|---|---|
| `-d` | Is a directory |
| `-f` | Is a file |
| `-r` | Is readable |
| `-w` | Is writable |
| `-x` | Is executable |
| `-e` | Exists |

**The `case` statement** (cleaner than multiple ifs):

```bash
case $USER in
rich | barbara)
   echo "Welcome, $USER";;
testing)
   echo "Special testing account";;
*)
   echo "Sorry, you're not allowed here";;
esac
```

### Loops

**The `for` loop** (iterates through a list):

```bash
for file in $(ls | sort) ; do
   if [ -d $file ]
   then
      echo "$file is a directory"
   fi
   if [ -f $file ]
   then
      echo "$file is a file"
   fi
done
```

**The `while` loop** (continues while condition is true):

```bash
number=$1
factorial=1
while [ $number -gt 0 ] ; do
   factorial=$[ $factorial * $number ]
   number=$[ $number - 1 ]
done
echo "The factorial is $factorial"
```

### Text Manipulation

- **Globbing**: wildcards like `*`, `?`, `[abc]` for matching filenames
- **Parameter expansion**: `${variable}` syntax for variable substitution
- **`read`**: gets input from user → `read varname`
- **Regular expressions**: pattern matching used with tools like `grep`, `sed`, `awk`

---

## 4. Creating Functions

Two ways to define a function:

```bash
# Method 1
function name {
   commands
}

# Method 2
name() {
   commands
}
```

Using a function:

```bash
factor() {
   number=$1
   factorial=1
   while [ $number -gt 0 ] ; do
      factorial=$[ $factorial * $number ]
      number=$[ $number - 1 ]
   done
   echo $factorial
}

result=$(factor 5)
echo "The factorial of 5 is $result"
```

---

## Key Commands Summary

| Command | Purpose |
|---|---|
| `chmod u+x file.sh` | Make script executable |
| `./script.sh` | Run script in current directory |
| `echo` | Display message |
| `read` | Get user input |
| `exit N` | Exit with status N |
| `bc` | Calculator for decimal math |

---

## Exam-Ready Vocabulary

- **Shebang** (`#!/bin/bash`) — tells the system which interpreter to use
- **Variable** — named storage for values
- **Environment variable** — system-wide variable like `$USER`, `$HOME`
- **Command-line argument** — input passed to a script (`$1`, `$2`)
- **Exit status** — return code (0 = success)
- **Command substitution** — capturing command output (`$(command)`)
- **Pipe** (`|`) — sending output to another command
- **Redirect** (`>`, `>>`) — sending output to a file
- **Globbing** — using wildcards to match filenames
- **Parameter expansion** — variable substitution syntax

---

## Practice Questions

**1.** What is the purpose of `#!/bin/bash` at the top of a script?

**2.** Which command makes a script executable for the user?
- A) `chmod 644 script.sh`
- B) `chmod u+x script.sh`
- C) `chmod r script.sh`
- D) `chown script.sh`

**3.** What does `$?` return?

**4.** Write the if statement to check if `$1` is greater than 80.

**5.** What's the difference between `>` and `>>`?

**6.** Which is correct for command substitution?
- A) `var=$date`
- B) `var=(date)`
- C) `var=$(date)`
- D) `var={date}`

**7.** In the script `./test.sh apple banana cherry`, what is `$2`?

**8.** Which exit status indicates a successful command?

**9.** What's the difference between `for` and `while` loops?

**10.** What symbol starts a comment in a bash script?

---

## Answer Key

1. The shebang line; tells the system to use `/bin/bash` to interpret the script
2. **B** — `chmod u+x script.sh`
3. The exit status of the last command (0 = success)
4. `if [ $1 -gt 80 ] ; then ... fi`
5. `>` overwrites the file; `>>` appends to it
6. **C** — `var=$(date)`
7. `banana`
8. `0` (zero)
9. `for` iterates through a known list; `while` continues as long as a condition is true
10. `#`

---

## Quick-Reference Card

```bash
#!/bin/bash                    # shebang
# comment                      # comment

var=value                      # set variable
$var or ${var}                 # use variable
$1 $2 $@ $#                    # arguments
$?                             # last exit status
$(command)                     # command substitution

if [ condition ] ; then        # if
   commands
fi

case $var in                   # case
   pattern) commands ;;
   *) default ;;
esac

for x in list ; do             # for loop
   commands
done

while [ condition ] ; do       # while loop
   commands
done

function name {                # function
   commands
}

chmod u+x script.sh            # make executable
./script.sh arg1 arg2          # run with arguments
```
