# Linux Users
Linux is a multi-user system, which means that multiple users can be logged into the system at the same time.

# Where are the users stored?
The users on a Linux system are stored in a file called ```/etc/passwd```. The content of that file is readable to anybody on the system. The information in the file is split up by using a colon. It looks thus like this:
```
root:x:0:0:root:/root:/bin/bash
```
Each field has a significance. Let's break it down.
## The user name
The user name is the first field, in our example above we have the user name root.
## Password
When there is a password for the user there will be an x. The password is kept as a hash (a one-way mathematical function) in a different file.
## The user ID
The user ID is a number the user gets when the user is created. The user is thus identified by the user ID and not the user name.
## The group ID
To make management a bit easier user administration is done by grouping the users into groups. The group ID is the identification number of such a group.
## The comment text
A comment text, can be anything but you can leave it blank 
## The home directory
The home directory of that user
## The shell
The shell that user will be presented at login on a terminal. The shell allows the user to interact with the system.

# Naming users
When you want to login to a system you need often just a username and a password. People often want life to be easy and give either:
* a combination of their name (jdoe or doej)
* or an email address (jdoe@company.com or doej@company.com or john.doe@company.com)
* or an employee number

As you probably have figured out, guessing a login is not the hardest thing, especially with company employee listings being public often published by the company or via social media websites like LinkedIn.
The only thing that you need to do is guess or figure out the user password.

To avoid this some organizations actually give randomized usernames to their employees, for example Ks692*. One could argue it is security through obscurity but the chance of randomly guessing that login from the first guess is rather low.
If you see that login being used in a situation that doesn't match the user's context (for example the user is in the country and the login attempt is from abroad) you know you have an issue.

# User types
There are 3 types of users:
1. The root user
2. The regular user
3. The system user

## The root
```root``` is the system administrator and thus has the capability to manage/manipulate the system. It is very dangerous to do everything as root since the system will execute everything you say to do.
It is also a bad practice to allow log in as root. To avoid this we will later see that we can escalate our privileges and just have a temporary root-like situation for system administration. 

As you saw in our extract from ```/etc/passwd``` the root user has as user ID 0 and as user ID 0. We can change the name of the root user on a system but it is only the user ID 0 that is the real root.
Renaming the root user to remotely log in with it remains a bad idea because the attacker will figure out eventually that you renamed the user by observing the ID of a user.

## The regular user
The regular user is a user of the system. Often regular users will have a home directory in ```/home/``` followed by the username. In that home directory the user can create directories and files.

## The system user
System users are users that are not real human users but are users created to run services on the network. For example the ```nobody``` user is a user that looks like this in ```/etc/passwd```.
```
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
```

As you can see the nobody user above has as shell ```/usr/sbin/nologin``` which blocks the user from logging in. The nobody user also has a home directory called ```/nonexistant``` which does not exist in the file hierarchy and thus has no home directory.
