---
layout: post
title:  "Basics of Shell Scripting"
date:   2025-12-06 11:00:00 -0500
author: Ravi Shankar Gurram
categories: ['Linux']
background: '/images/scripting/scriptbackground.png'
---

## Basics of Shell Scripting

As a Linux administrator, you often need to repeat tasks such as installing tools, creating users, or performing backups. Doing these manually every time can be time‑consuming and error‑prone. This is where shell scripting comes in.

Shell scripting allows you to automate repetitive tasks in Unix/Linux like operating systems. Instead of typing commands one by one, you can write them into a script file. When executed, the script runs the commands in sequence, making it easier to handle operations like user management, file management, and backups etc.

Before writing scripts, it’s important to understand a few core concepts:

- Shell: The shell is a command‑line interpreter that acts as the user interface for Unix/Linux systems. It takes your commands and passes them to the operating system. Common shells include: 
    - Bourne shell (sh), 
    - C shell (csh), 
    - Korn shell (ksh),
    - Bourne Again shell (bash).

- Script: A shell script is simply a file containing a series of commands. Instead of typing commands one by one, you save them in a script and run the file to execute everything in order. This makes automation possible. These commands are written in a scripting language supported by the shell.

- Shebang: The very first line of a shell script usually starts with #! followed by the path to the interpreter. This line tells the system which shell should run the script.
    - `#!/bin/bash`
    - `echo "Hello, World!"`

- Variables: Think of variables like a labeled container that can hold values for your script. In scripting we can use variables to store data. In shell scripting variables are defined without any data type and can hold numbers, strings, etc. You can assign values to variables as shown below.
    - `name="Ravi"`
    - `echo "Hello, $name"`

- Environment Variables: environment variables have system information stored in them, you can use this in scripts to retrieve information such as your home directory, user id etc. Here are some examples:
    - `$HOME`: stores user home directory
        - `echo $HOME`
    - `$USER`: stores username
        - `echo $USER`
    - to view all the environment variables you can use the `set` command.

- Command line arguments: you can pass data to shell scripts using command line arguments when you are running them. In a shell script, arguments are accessed using special variables:
    - `$1` the first argument
    - `$2` the second argument
    - `$3` the third argument, and so on
    - `$@` refers to all arguments
    - `$#` shows the number of arguments passed

command substitution $()

- Conditional Statements (`if`): Conditional statements let you execute different code depending on conditions.
    -  ```
        if [$1 -gt 10]; then
            echo value entered is greater than 10
        else
            echo value entered is less than 10
        fi
        ```
    - ```
      $username = $1
      if id "$username"; then
        echo "User $username already exists."
      else
        sudo useradd "$username"
        echo "User $username created successfully."
      fi
      ```
- The case statement is like a multi‑branch if. It’s useful when you need to handle different values of a variable or argument.
    - ```
        #!/bin/bash
        echo "Choose an option: start, stop, restart"
        action=$1

        case $action in
        start)
            echo "Starting the service..."
            ;;
        stop)
            echo "Stopping the service..."
            ;;
        restart)
            echo "Restarting the service..."
            ;;
        *)
            echo "Invalid option. Use start|stop|restart."
            ;;
        esac
      ```

- Loops allow you to execute a block of code repeatedly. Common types are for, while and until.
    - A For Loop will iterate over a list of items.
        -   ```
            #!/bin/bash
            for i in 1 2 3 4 5
            do
              echo "Number: $i"
            done
            ```
    -  A While Loop will runs as long as the condition is true.
        - ```
            #!/bin/bash
            counter=1
            while [ $counter -le 5 ]
            do
                echo "Counter: $counter"
                ((counter++))
            done
          ```
    - An Until Loop will runs until a condition becomes true (opposite of while).
        - ```
          #!/bin/bash
          counter=1
          until [ $counter -gt 5 ]
          do
            echo "Counter: $counter"
            ((counter++))
          done
          ```
- Functions let you group commands into reusable blocks. They make scripts modular and easier to maintain.
    -   ```
        #!/bin/bash
        greet_user() {
        echo "Hello, $1!"
        }

        function check_age {
        if [ "$1" -ge 18 ]; then
            echo "You are an adult."
        else
            echo "You are a minor."
        fi
        }

        greet_user "Alice"
        check_age 20
        ```