# Command Line Interface

One of the scenes often depicted in movies involves a hacker, chewing gum nonchalantly, clad in a hoodie, deftly typing away on a keyboard. On a black screen with green text, lines of text scroll rapidly. Accompanied by a mysterious soundtrack, the hacker alternates between smirking and looking serious, eventually striking the return key decisively. As the background music halts abruptly, a progress bar on the screen ticks to 100%, and the hacker leans back in their chair, looking relaxed, giving a 'mission accomplished' gesture.

![Hacking in film](ch2/hacking.png)

Although films tend to exaggerate scenarios and artistic processing, the quick scrolling text could be a log for installing a software package. Undeniably, this operation does indeed look cool.

In the first section, we are going to learn about this cool technology: shell and command line.

- Shell prompt
- Composition of command-line tools: commands, scripts, switches, and options

Command-line tools and Shell are fundamental components of computer operating systems. Similar to a graphical interface, command-line tools, also known as a CLI (Command-Line Interface), provide an interface allowing users to interact with a computer system by typing text commands.

These commands can perform various tasks, such as file operations (copy, move, delete, etc.), system administration (start or stop services, install or uninstall programs, etc.), and even network communications (like SSH). In other words, you can accomplish most tasks you need the computer to do via the command line.

A specific example is launching the "Finder" application either by clicking its icon with a mouse or by typing "open ." in the Terminal (assuming the executable file of the code is set in the environment). Both methods lead to the same result.

![Finder application](ch2/finder.png)

A program closely related to the command-line interface is Shell. Shell is a special command-line tool that provides a scripting language, which users can use to write scripts for automated tasks. In Unix and Unix-like systems (like Linux and macOS), Bash (Bourne Again Shell) is the most common shell, while in Windows, PowerShell is the most common.

Shell can be understood as a medium for user interaction with the system kernel. Users enter commands in the shell, which then forwards these commands to the system kernel for execution and returns the results to the user. Therefore, proficiency in using shell is crucial for efficient use of the computer system.

On a Mac, Terminal is an application that provides users with a text interface to run Shell. You can view Terminal as a window allowing access to Shell. When you type commands in Terminal, you are actually telling Shell to execute these commands.

![Run linting](ch2/lint.png)

In summary, command-line tools and Shell offer a powerful and flexible way to control and manage computer systems. Although they may take some time to learn and get familiar with, once mastered, they can greatly enhance your productivity.

## Executing a Command

Let's introduce some terms used in this book via a command. A command typically comprises three parts: command, argument, and option. Arguments and options can often be omitted, with the command executing default functions. When we need to specify a command to perform a particular task, we can control it through arguments and options.

![Command line interface](ch2/cli.png)

In the above command, we executed the `npm` command, passing `install` and `html2canvas` as arguments, and `—save-dev` as an option. When executed, `npm` uses the `install` argument to install the `html2canvas` package, and the `—save-dev` option installs this package into the development dependencies (i.e., not included

 in the final product package).

You might ask, where do these commands come from? How do I know what commands can be executed?

In Shell, you can execute built-in commands of the operating system, such as file and directory operations: `ls` (list directory contents), `cd` (change directory), `mkdir` (create directory), `rm` (remove file or directory), `mv` (move or rename file or directory), `cp` (copy file or directory), etc. You can also install external applications, like executing the `npm` command after installing Node.js.

Additionally, you can define your own commands, usually in two ways:

1. **Aliases**: You can use the `alias` command to create a new command alias, representing a single command or a series of commands. For example, you can create an alias `ll` in Terminal as follows:

    ```bash
    alias ll='ls -l'
    ```
    
    Then, when you type `ll` in the command line and hit enter, Shell will execute the `ls -l` command, as if it replaced it for you.
    
2. **Shell Scripts**: You can create a Shell script file, and write a series of commands within it. This file is equivalent to a new command. You need to grant execution rights to the file, and then it can be executed like any regular command. For instance, you can create a file named `hello.sh`:

    ```bash
    #!/bin/sh
    
    echo "Hello, world!"
    ```
    
    And then give this file execution permissions:

    ```bash
    chmod +x hello.sh
    ```
    
    Consequently, you can run this script using the `./hello.sh` command.
    

In this book, we will primarily discuss and use the second method: Shell scripts. In a script, you can define many different commands, like downloading a file in the first step, parsing the file in the second step, and triggering another action based on a field in the file in the third step, etc.

```bash
#!/bin/bash

# Define directory and file name
dir="tools"
filename="network.conf"

# Check if file exists
if [ -f "$dir/$filename" ]; then
  echo "The file $filename already exists."
  exit 0
else
  echo "The file $filename does not exist. Downloading..."

  # Download file and save in specified directory
  curl -o "$dir/$filename" https://path-to-your/network.conf

  echo "The file $filename has been downloaded to the $dir directory."
fi
```

This script first checks whether a file named network.conf exists in the tools directory. If it exists, the script will print a message and exit. If not, the script will download the network.conf file from a specified URL and save it to the tools directory.

Now that we've done the groundwork, having learned the basics of Shell and command-line interface, in the upcoming content, we'll further learn how to define build scripts and automate programming tasks by implementing a "quote of the day" application.