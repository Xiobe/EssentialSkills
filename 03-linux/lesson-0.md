# Getting started with a terminal
Most people get scared by a terminal (on Windows Powershell), they are used clicking and a bit of typing but that is it. There are a couple of good reasons to learn how to use a terminal:
1. It is fast
2. It gives often more possibilities
3. It allows you to do scripting which is programming interations without you typing

I will be using a terminal called bash, which is the default on a lot of systems.

# The terminal prompt
The terminal prompt is where it all starts. It will look something like this
```
<user>@<system>:<current directory>$
```

The <user> is who you are currently logged in as. Where <system> is the name of the current system you are logged on to. The <current directory> is where on the file system you are currently and it ends with a $-sign or a #-sign.
The difference between the $-sign and the #-sign is what you can do. #-sign is to indicate you are root, which is the system administrator.

It should be noted that there is absolutely **no** reason for any user to be logged in as root unless you are a *special snowflake* that can not follow basic security rules. In modern operating system you can elevate your privileges. In Windows you tell Windows you want to become Administrator and in Linux we do this with sudo. If you do not have sudo privileges and you need them you need to be root to configure that (which we will see later)..
