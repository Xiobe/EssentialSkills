# User Groups
As mentioned before system administrators group their users to make life a bit easier. The user has a group ID indicating to which group the user belongs.

# Where are the groups stored?
The groups are stored in a file called ```/etc/groups``` or ```/etc/group``` depending on what system you are. This file is readable for every user logged in on the system. The content of the file is based on the same principles as ```/etc/passwd``` and looks like this:
```
root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
adm:x:4:syslog
```

Again each field has its own meaning so let's break it down

## group name
The group name is the name given to a group
## group password
When the group password field contains an x, it means that there is an entry set for the group in the file ```/etc/gshadow```. This file can contain group passwords.
## group ID
The ID is the identification number for the group, again Linux compares numbers not names.
## group members
The last field is the mapping between the user and the group.

# Primary group and secondary groups.
A common tactic is to make all users member of one group and then assign them to distinct groups. That common shared group is called the primary group and basically can get access to the users their own files. 

The secondary group is to grant access to shared resources on the system. For example if you are member of the accounting you will get access to the accounting files through the accounting group and will not have access to the shared files of the marketing group.
