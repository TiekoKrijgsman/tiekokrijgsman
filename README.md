# Command-Line Interface (CLI)

## Links

- [Command Line Interface Guidelines](https://clig.dev)
- [Complete Intro to Linux and the CLI](https://btholt.github.io/complete-intro-to-linux-and-the-cli/)

## Differences between a "Shell" and a "Terminal"

- **Terminal:** Run shells applications, Graphical User Interface (<abbr>GUI</abbr>).
- **Shell:** Command line interpreter (e.g: Bash, Zsh, Fish, etc.).

## Basics commands - [GNU Core Utilities](https://www.gnu.org/software/coreutils/)

`man`, `ls`, `mkdir`, `touch`, `pwd`, `cd`, `mv`, `cat`, `echo`, `rm`, `cp`, `chmod`, `chown`, `find`, `find`, `tr`, `date`, `ps`, `whoami`, `tail`, `head`, `wc`, `ln`, `cut`, `sort`, `read`, `expr`, `uniq`, etc.

- `man`: manual pages (e.g: `man ls`)
- `echo`: print arguments (e.g: `echo "Hello World"`)
- `echo $0`: print the name of the current shell
- `which`: locate/path of a command (e.g: `which ls`)
- `ls -al`: list all files and directories in the current directory, including hidden files and directories
- `ls -d`: list only directories
- `stat`: show file or file system status
- `cat "file1.txt" "file2.txt"`: concatenate files
- `mkdir -p`: create directories recursively with parents (e.g: `mkdir -p "dir1/dir2/dir3"`)
- `touch`: create an empty file (e.g: `touch main.txt`)
- `rm -rf`: remove files and directories recursively and force (e.g: `rm -rf main.txt`)
- `pwd`: print working directory
- `cd`: change directory
- `mv`: move files and directories
- `cp`: copy files and directories
- `tail -nN`: show the last `N` lines of a file (e.g: `tail -n5 main.c`)
- `head -nN`: show the first `N` lines of a file (e.g: `head -n5 main.c`)
- `sort -r`: sort in reverse order
- `sort -n`: sort numerically
- `cut -fCOLUMN -dDELIMITER -cPOSITION`: cut a column from a file (e.g: `cut -f1 -d: /etc/passwd`)
- `grep OPTIONS PATTERN FILE`: search for a pattern in a file (e.g: `grep "hello" main.c`)
    - `-i`: ignore case
    - `-n`: show line number
    - `-v`: invert match (ignore pattern)
    - `-r`: recursive
- `expr`: evaluate expressions (e.g: `expr 5 + 1`)
- `find`: find files and directories (e.g: `find . -size +100M`: find files bigger than 100MB in the current directory)
- `diff`: compare files line by line (e.g: `diff file1.txt file2.txt`, or compare output of commands `diff <(echo "Hello\!") <(echo "Hello, world\!")`)
- `su -`: switch user (e.g: `su - root`)
- `sudo`: execute a command as root (e.g: `sudo ls`)
- `adduser`: add a user (e.g: `adduser user1`)
- `df --human-readable`: report file system disk space usage
- `lshw`: list hardware

## Aliases

Commands can have aliases, for example: `alias ls='colorls'`, so when typing `ls` it will run `colorls` instead.

- `alias`: list aliases
- `unalias`: remove an alias

To opt-out of an alias, we can use `\` before the command, for example: `\ls`, so it will use the original command.

## Processes

- `ps`: list processes (e.g: `ps aux`: list all processes)
- `kill`: kill a process (e.g: `kill 1234`: kill process with PID 1234, `kill -9 1234`: kill process with PID 1234 with SIGKILL signal)
- `&`: run a program in the background (e.g: `sleep 10 &`)
- `jobs`: list background jobs

## Redirects

- `>`: redirect output to a file (e.g: `echo "hello" > main.txt`)
- `>>`: append output at the end of a file (e.g: `echo "world" >> main.txt`)
- `<`: redirect input from a file (e.g: `cat < main.txt`)
- `<<`: redirect input from a string (e.g: `cat << EOF`)
- `2>`: redirect error to a file (e.g: `ls -l /root 2> error.txt`)
- `2>>`: append error at the end of a file (e.g: `ls -l /root 2>> error.txt`)
- `&>`: redirect output and error to a file (e.g: `ls -l /root &> output.txt`)
- `&>>`: append output and error at the end of a file (e.g: `ls -l /root &>> output.txt`)
- `2>&1`: redirect error to output (e.g: `ls -l /root 2>&1`)
- `|`: redirect output to another command (e.g: `ls -l | grep "main"`)

## Permissions

Each file has a set of file mode bits that control the kinds of access that users have to that file. They can be represented either in _symbolic notation_ or _octal notation_.

- `chmod`: change permissions (e.g: `chmod 754 main.txt`)
- `chown`: change owner (e.g: `chown root:root main.txt`: change owner to root and group to root)

To give all permissions (read, write and execute) to user, read and execute permissions to group and read permissions to other, you can use `chmod u=rwx,g=rx,o=r main.txt` or `chmod 754 main.txt`.

To give all permissions to all (user, group and other) recursively to all files and directories in the current directory: `chmod 777 -R .`.

_Note:_ Groups are used to organize users and their names should be singular (e.g: `administrator` instead of `administrators`).

### Symbolic notation

```txt
---     ---     ---
rwx     rwx     rwx
user    group   other
```

#### Read, write and execute permissions

- `r`: read
- `w`: write
- `x`: execute

#### Symbols

- `+`: add permission
- `-`: remove permission
- `=`: set permission

#### User, group and other

- `u`: user (owner of the file or directory)
- `g`: group
- `o`: other
- `a`: all (user, group and other)

### Octal notation

3 digits represent respectively, in order, the permissions for the user, group and other.

- `4`: read permission
- `2`: write permission
- `1`: execute permission
- `0`: no permission

#### Examples

- `7`: `4 + 2 + 1`: read, write and execute permissions
- `5`: `4 + 1`: read and execute permissions
- `4`: `4`: read permission

## Bash Programming

Shebang: The first line of a script that starts with `#!` and tells the shell which interpreter to use to execute the script.

For example: `#!/usr/bin/env bash`

### Variables

```sh
# Declare a variable
my_var="hello"

# Print a variable
echo "$my_var"

# List of arguments
echo "$@"

# Number of arguments
echo "$#"

# Argument n°1 and n°2
echo "$1"
echo "$2"
# etc.

# Exit status of the last command
echo "$?"

# Exit the script
exit 0
```

### Conditionals

```sh
# `-eq`: equal
# `-ne`: not equal
# `-gt`: greater than
# `-ge`: greater than or equal
# `-lt`: less than
# `-le`: less than or equal
# `-z`: empty
# `-n`: not empty
# `!`: not
# `-a`: and
# `-o`: or
if [[ 5 -eq 5 ]]; then
    echo "5 is equal to 5"
elif [[ 5 -ne 5 ]]; then
    echo "5 is not equal to 5"
else
    echo "5 is not equal to 5"
fi
```

### Loops

```sh
# For loop
for i in {1..5}; do
    echo "$i"
done

# While loop
i=1
while [[ $i -le 5 ]]; do
    echo "$i"
    ((i++))
done
```

## How to write `.iso` to a bootable USB device using `dd` command

[How to write/create a Ubuntu .iso to a bootable USB device on Linux using dd command](https://www.cyberciti.biz/faq/creating-a-bootable-ubuntu-usb-stick-on-a-debian-linux/)
