
![w:160](https://docs.microsoft.com/en-us/learn/achievements/student-evangelism/bash-introduction-badge.svg)
# Introduction to Bash

Embrace the power of the command line! 

> This workshop is based on this learning module which you should totally visit to learn even more! [Introduction to Bash - Learn](https://aka.ms/bashintro5)

# Ask me questions

- [Tweet me](https://twitter.com/madebygps)
- [Connect with me on LinkedIn](https://linkedin.com/in/madebygps)
- [Follow the tok!](https://tiktok.com/@madebygps)
- [My code and stuff on GitHub](https://github.com/madebygps)
- [My notes on my blog](https://madebygps.com)

# Agenda

- Theory
    - What is a Command Line Interface
    - What is a Shell
    - What is a Terminal
    - What is a Shell script
    - What is Bash
- Bash Fundamentals:
    - syntax
    - get help with commands
    - Wildcards
    - Fundamental commands
    - Redirection and Pipelines
- Practice exercise
    - Terminate a process
    - Filter output with grep 
    - A simple bash script

# What is a Command Line Interface?

A text-based program that performs actions via commands (e.g., Azure CLI)

- Azure CLI
- Docker CLI

# What is a Shell?

A shell is a program that runs commands (from CLIs or built-in) and outputs results.

It is one of the most important parts of a Unix system. There are many different Unix shells, but all derive features from the Bourne Shell `/bin/sh`

# What is a Terminal?

 A GUI program used to run a shell 

- iTerm2
- Guake
- Windows terminal

# What is a Shell Script?

A text file that contains a sequence of shell commands.

# What is Bash?

Linux uses an enhanced version of the Bourne Shell called Bash. Bash stands for Bourne Again Shell and it’s become the Defacto standard for managing Linux machines. 


# Bash fundamentals

The full syntax for a Bash command is

```bash
command [options] [arguments]
```

- The first item is the command
- Commands are often followed by arguments
- Most commands have options (often called flags) that modify how they work.

```bash
ls -a /etc
```

- `ls` is the list contents command
- `-a` is an option (flag) to include hidden files/directories
- `/etc` is an argument to the ls command that tells which directory to list.

Flags can be combined to be more concise.

```bash
# Combined
ls -al /etc
# Long form
ls -a -l /etc
```

## Get help

You are not expected to memorize commands, arguments, or flags. Bash has documentation built into the operating system. To access it, we type `man` (short for manual page) followed by the command. 

```bash
man mkdir
```

We can also use the `--help` flag and it’ll provides us with a description of the command’s syntax and options more concisely than `man`.

```bash
mkdir --help
```

## Wildcards (metacharacters)

Wildcards are symbols that represent one or more characters in Bash commands.

- `*` represents zero or a sequence of characters.
    - List files that end with .png = `ls *.png`
    - List files that end with .jpeg or .jpg
        
        ```bash
        ls *.jp*g
        ```
        
- `?` represents a single character
    - List files that start with 000 and end with .jpg = `ls 000?.jpg`
- `[]` brackets are used to denote groups of characters.
    
    ```bash
    # List all files in the current directory that contain a period immediately followed by a lowercase J or P
    ls *.[jp]*
    ```
    
- Files names and commands are case sensitive.
    
    ```bash
    # List all files in the current directory that contain a period immediately followed by a lowercase J or P
    ls *.[jpJP]*
    ```
    
- Expressions in square brackets can represent a range of characters
    
    ```bash
    # List all files in the current directory that begin with a lowercase letter
    ls [a-z]*
    
    # List all files in the current directory that being with a lower or uppercase
    
    ls [a-zA-Z]*
    ```
    

# Commands to explore for the basics

- `ls`
    - Lists the contents of the current directory or specified directory.
    - Some popular arguments
        - `-a` Include hidden files and directories (names begin with a period)
        - `-l` Include more info about each file.
- `pwd`
    - print working directory.
- `cat`
    - Concatenate file(s) to standard output. Fancy way of saying display the contents of a file to the screen.
- `man`
    - Get documentation on a command.
- `cd`
    - Stands for change directory.
    - Get help with `man builtins`
    - Move up with `..`
    - Go home with `~`
- `mkdir`
    - Create a directory.
    - `-p` to create a subdirectory 
- `rmdir`
    - Delete an empty directory.
- `rm`
    - Delete a file or all files with `*`
    - Make it a habit to use `-i` to be prompted before every removal.
- `cp`
    - Copy files and directories.
    - This command will replace, so use `-i` to be prompted before overwriting.
    - Use `r` to copy subdirectories. Stands for recursive.
- `ps`
    - Get a snapshot of all currently running processes.
- `sudo`
    - Some commands can only be run by the root user (aka superuser). You don’t want to login as the root user because
        - You have no record of system-altering commands or who is performing them.
        - You don’t have access to your normal shell environment.
    - Most distros use a package `sudo` that allows to run commands as root when logged in as themselves.
    - To run commands that require admin privilege without logging in as the root user, preface the command with `sudo`
    
## Redirection
    
I/O redirection allows us to redirect input and output of commands to and from files, as well as connect multiple commands together into pipelines.

- `<` for redirecting input to a source other than the keyboard
- `>` for redirecting output to destination other than the screen
- `>>` for doing the same, but appending rather than overwriting
- `|` for piping output from one command to the input of another

### Redirecting Standard Output

Also know as `stout` The program's results. Known as Standard Output or `stdout`. To redirect stdout to another file instead of the screen, we use the `>` redirection operator followed by the name of the file.

For example, we could tell the shell to send the output of the ls command to the file `ls-output.txt` instead of the screen:   

- `ls -l /usr/bin > ls-output.txt`

We can see that the `ls` output was not sent to the screen, but to the `ls-output.txt` file.

Keep in mind that using the redirection operator will overwrite the destination file. To append, we use the `>>` redirection operator. 

### Redirecting Standard Error

Status and error messages. Known as Standard Error or `stderr`. To redirect `stderr` we must refer to its file descriptor. The shell references `stdout` `stdin` and `stderr` internally as file descriptors 0, 1, and 2, respectively. We can redirect stderr with this notation:

- `ls -l /bin/usr 2> ls-error.txt`
    
### Redirecting Standard Output and Standard Error to one file

There are two ways to accomplish these, first let's use the traditional method, which works with old versions of the shell:

- `ls -l /bin/usr > ls-output.txt 2>&1`

First we are redirecting `stdout` to the file `ls-output.txt` and then we redirect file descriptor 2 `stderr` to file descriptor 1 stdout using the `2>&1` notation. 

Keep in mind that the order of the redirections matter, the redirection of `stderr` must always occur after redirecting `stdout`.

Recent versions of bash provide a second, more streamlined method for performing this combined redirection:

- `ls -l /bin/usr &> ls-output.txt`

### Redirecting Standard Input

Using the `<` redirection operator, we can change the source of stdin from the keyboard to a file.

- `cat < sample.txt`

## Pipelines

It is possible to put several commands together into a pipeline. The commands used this way are referred to as filters. Filters take input, change it somehow and then output it.
Using the pipe operator |, the `stout` of one command can be piped into the `stdin` of another. less is an example of this:

- `ls -l /usr/bin | less`
- `ps -ef | grep daemon`
- `cat file.txt | fmt | pr | lpr`

- Use the `tee` command to read `stdin` and copies it to both `stdout` and to one or more files. `ls /usr/bin | tee ls.txt | grep zip`

# Terminate a misbehaving process

1. Create a program named `bad.py` that will misbehave.
    
    We’ll use `vi` to create a python program. It’s the universal text editor of Linux. Others include `vim` `nano` `emacs` 
    
    ```bash
    # Create the program with vi
    vi bad.py
    
    # Insert the following
    i = 0
    while i == 0:
       pass
    ```
    
2. Start the program `python3 bad.py &`. The `&` tells bash to run the command in the background and not wait for it to finish.

3. Let’s list all the process that contain “python”
    
    ```bash
    ps -ef | grep python
    ```
    
    - `ps` lists the running processes in the current shell
    - `-e` shows all processes
    - `-f` lists in full-format listing.

    We can see `bad.py` is taking up a lot of CPU time.

4. Let’s use the `kill` command to stop the running process. Let's first list the signal types.
    
    ```bash
    # Display a list of signal types
    kill -l    
    ```
    - If we wanted a process to restart, we would use `SIGHUP`
    - In this case we want to stop the process without restarting, so we use `SIGKILL` `9`
    
5. We have to decide which signal we are going to send to `kill` The default is `TERM`(allows the process to safely shut down)
    ```bash
    kill -9 PROCESS_ID
    ```
## Use Bash and grep to filter CLI output

Let’s use Linux’s universal pattern-matching program `grep` to filter Azure CLI output.

```bash
az vm list-sizes --location westus --output table

az vm list-sizes -l eastus2 -o table | awk '{print $3}' | grep DS.*_v2$
```

# The `grep` command

`grep` stands for global regular expression print and is the most widely used text manipulation command. 

The grep command searches text files for text matching a specified regular expression and outputs any line containing a match to standard output.

Two of the most important options for `grep` are:

- `-i` for case-insensitive matches.
- `-v` inverts the search and prints lines that don’t match.

Regular expressions are an entire topic but the three important things to remember are:

- `.*` matches any number of characters, including none.
- `.+` matches any one or more characters.
- `.`  matches exactly one arbitrary character

Let’s play around with grep. We’ll list the contents of a few directories to make text files.

```bash
# /bin directory that holds binaries
ls /bin > list.txt

# /sbin director that holds binaries with superuser privileages requiered
ls /sbin > list-sbin.txt

# /usr/bin directory that holds general system-wide binaries.
ls /usr/bin > list-usr.txt

# /usr/sbin directory that holds general system-wide binaries with superuser privileges.
ls /usr/sbin > list-usr-sbin.txt
```
## Some more grep exercises

```bash
grep bzip list*.txt
grep -l bzip list*.txt
grep -L bzip list*.txt
grep -h '.zip' list*.txt
grep -h '^zip' list*.txt
grep -h 'zip$' list*.txt
grep -h '[bg]zip' list*.txt
grep -h '[^bg]zip' list*.txt
grep -h '^[A-Z]' list*.txt
grep -h '^[A-Za-z0-9]' list*.txt
grep -Eh '^(bz|gz|zip)' list*.txt
grep -Eh '^bz|gz|zip' list*.txt
```

## Simple bash script

1. Write a script named `create_files.sh` with `vim create_files` with this contents:

    ```sh
    #!/bin/bash 
    # Say hello and create some files

    echo 'Creating files for you!'
    for f in {a..z} {0..5}; do
        echo hello > "$f.txt"
    done
    ```
2. Make the script executable with `chmod`
    - `chmod 755` for scripts that everyone can execute.
    - `chmod 700` for scripts that only the owner can execute.

    In our case: `chmod 755 create_files`

3. Run the script `./create_files`
4. If we wanted to run this command without the `./` we would need to add it to a directory in our `$PATH`
5. `echo $PATH`
6. An ideal location for personal scripts is `/usr/local/bin`
7. Move file there `sudo cp create_files /usr/local/bin`
8. Now you can run `create_files`
