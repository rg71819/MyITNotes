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
- Variables: Think of variables like a labeled container that can hold values for your script. In scripting we can use variables to store data. In shell scripting variables are defined without any data type and can hold numbers, strings, etc.
    - `name="Ravi"`
    - `echo "Hello, $name"`
