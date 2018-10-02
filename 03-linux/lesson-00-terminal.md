# Getting started with a terminal
Most people get scared by a terminal (on Windows Powershell), they are used clicking and a bit of typing but that is it. There are a couple of good reasons to learn how to use a terminal:
1. It is fast
2. It gives often more possibilities
3. It allows you to do scripting which is programming interations without you typing

I will be using a shell called bash, which is the default on a lot of systems.

# A terminal, a shell and a console walk into a bar ...

## Shell
When you log into a Linux system you are presented with an application to control your session. This application is called the shell. I will be using the bash shell in this tutorial unless otherwise mentioned. 

The output of the shell to the screen looks like this: 
```
<user>@<system>:<current directory>$
```

The <user> is who you are currently logged in as. Where <system> is the name of the current system you are logged on to. The <current directory> is where on the file system you are currently and it ends with a $-sign or a #-sign.
The difference between the $-sign and the #-sign is what you can do. #-sign is to indicate you are root, which is the system administrator.
  
## Terminal
The terminal is a wrapper around a shell. In a GUI you will see a nice little black box to represent to you the shell. The shell is the actuall process it addresses, not the little window. That little window is called the terminal.

## Console
A console is a left over word from when we connected over serial ports to with a screen and a keyboard physically to systems. Modern Linux systems provide you with a virtualized console but to get started you should not worry about consoles. Just know the difference between the shell and the terminal to start with.

# Warning
It should be noted that there is absolutely **no** reason for any user to be logged in as root unless you are a *special snowflake* that can not follow basic security rules. In modern operating system you can elevate your privileges. In Windows you tell Windows you want to become Administrator and in Linux we do this with sudo. If you do not have sudo privileges and you need them you need to be root to configure that (which we will see later).
