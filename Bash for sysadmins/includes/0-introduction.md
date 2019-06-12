# Introduction

Imagine that you just started a new job as a system administrator (sysadmin) at Northwind, a small chain of cat boutiques for cuddly felines. You know Windows Server like the back of your hand, but in your new position, you also need to manage Linux servers. It's time to skill up.

A vital tool you must learn is [Bash](https://en.wikipedia.org/wiki/Bash_(Unix_shell)). The name stands for "Bourne Again SHell." 

A shell is a program that commands the operating system to perform actions. You type commands into a console and execute them directly, or use scripts to execute batches of commands without having to type the commands repeatedly. That's different from the commonplace graphical interface. Shells such as [PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/overview?view=powershell-6) and Bash give system administrators the power and precision they need for fine-tuned control of the computers for which they are responsible. 

If you do any serious sysadmin work with Linux, you must get to know Bash. While there are other Linux shells, including [csh](https://en.wikipedia.org/wiki/C_shell), [Korn shell](https://en.wikipedia.org/wiki/KornShell), and [zsh](https://en.wikipedia.org/wiki/Z_shell), Bash has become Linux's default shell. That's because Bash is compatible with Unix's first serious shell, the [Bourne shell](https://en.wikipedia.org/wiki/Bourne_shell), also know as sh. Bash incorporates the best features of its predecessors. It includes its own built-in commands and can invoke external programs.

One reason for its success is its simplicity. Bash, like the rest of Linux, is based on the Unix design philosophy. As Peter Salus summarized in his book, [A Quarter Century of Unix](https://www.amazon.com/Quarter-Century-UNIX-Peter-Salus/dp/0201547775/ref=sr_1_1), the three fundamental ideas are:
- Programs do one thing and do it well
- Programs work together
- Programs use text streams as the universal interface

That last part is key to understanding how Bash works. In Unix and Linux, everything is a file. That means you can use the same commands without worrying about whether the I/O stream — the stream of characters — is connected to a keyboard, a disk file, a socket, a pipe, or another I/O abstraction.

Whatever you think of it personally, Bash is the command shell of choice for Linux users. As [PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/overview?view=powershell-6) is to Windows Server, so Bash is to Linux. It behooves you to gain at least a modicum of expertise using the shell.

Bash has many features. Since we're just getting started, we focus on a few commands and how to use them together.

First, though, we need to do a little system configuration.