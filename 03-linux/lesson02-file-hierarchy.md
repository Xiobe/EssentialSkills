# The Linux File Hierarchy
On Windows we start the file system with a drive letter `C:\`. On Linux we have kind of the same idea but there are no concepts like drives. The system works differently and trying to make comparisons does not really serve you good.

# The root
When we look at a file system we can compare it to a tree. We have a shared root system and a trunk, then we have branches and finally leaves. The root system and trunk are called ```the root``` and are represented by `/`. Notice that the slash has a direction than in Windows, it is common to make typos, do not worry about them.

# Directories
When we keep our tree analogy the directories are the branches of the tree. They are simply there to organize data. That can be data that either belongs to the user or to the operating system.

# Files
Finally we have our files, which contain the actual data and are grouped into directories most often of the time.

# The hierarchy
Linux has a specific hierarchy but before we dive into that subject we will look at the basic commands to navigate our file system.

## ls
```
user@host ~$ ls
```
The ```ls ``` command allows you to list the contents of a directory. By default ls will not show you all files, do not worry about it we will come back to ```ls``` later. 

## pwd

```
user@host ~$ pwd
```
The ```pwd``` command allows you to print out the current directory.

## cd
```
user@host ~$ cd <destination>
```
The ```cd``` command allows you to change directories. By filling in the destination directory you will switch to that destination. Once you understand the hierarchy you will be able to navigate with ```cd``` to anywhere on the system.

## hierarchy
As stated above we always start with the root which is written as ```/```. To position yourself into the root you do
```
user@server ~$ cd /
```

Now that you are in the root. Notice that your prompt has been changed. It should look like this
```
user@server /$
```

If we want to see the contents of our root we run 
```
user@server /$ ls
```

This will show you the directories that are located in the root. All Linux systems share a similar structure you will find described as the FHS or file hierarchy standard.

### /bin
```/bin``` is a directory in which we find a number of essential command binaries that are available to all users. For example the ```ls``` command we used to list the files lives here. It is a very important directory and thus if these commands are manipulated by an attacker you would have a serious issue. We will see techniques later to detect any manipulation of this directory.
### /boot
```/boot``` is where the bootloader lives together with the kernels and other files to start the system. 
### /dev
```/dev``` is a representation of devices on your system. A hard disk on your system is represented by a file in ```/dev```.
### /etc
```/etc``` is where the configuration of the system lives. You will thus manipulate these files during installation or when reconfiguration of the system is required.
### /home
```/home``` is where the user stores his or her files on the system. Each user gets their own subdirectory in which that user is ruler of the universe.
### /lib
```/lib``` contains essential libraries for the binaries. We will discuss libraries later.
### /media
```/media``` contains directories of external media we want to keep for a longer period. Short term use of external media should be found in /mnt
### /mnt
```/mnt``` contains directories of external media that are temporarily attached to the system.
### /opt
```/opt``` contains optional software packages
### /proc
```/proc``` is a representation of the processes running on the system.
### /root
```/root``` is the home directory of the root user.
### /run
```/run``` contains information since the system last booted.
### /sbin
```/sbin``` contains essential system binaries.
### /srv
```/srv``` is reserved for data such as data and scripts for web servers, ftp servers, and versioning systems.
### /sys
```/sys``` contains information about devices, the drivers, and some kernel features. 
### /tmp
```/tmp``` contains temporary files, a word of caution is in place. If you store data here it will often not be preserved during reboots.
### /usr
```/usr``` contains the majority of the applications on the system
### /var
```/var``` contains files that change during operation like log files and spool files

# Absolute and Relative path
In Linux you will see either a path expressed as an absolute path, meaning you express it from the root (e.g. ```/var/www/```) or an relative path which is starting from where you are. Depending on the situation the use of a relative path can be risky unless you position yourself first at the right location.
