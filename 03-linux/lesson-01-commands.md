# Working with commands
As mentioned in the previous lesson, when you log into a Linux system the program that is started is called a shell. It allows you to interact with the system via the command line. In this lesson we will see a number of basic commands to get a feel of working with the command line.
# whoami
```
user@host ~$ whoami
```
When you type ```whoami``` at your prompt you will be returned with the name of your login.

# date
```
user@host ~$ date
```
When you type ```date``` at your prompt you will be returned with the current date. The output has
* day of the week
* month
* number of the day for that month
* time stamp
* time zone
* year
# cal
```
user@host ~$ cal
```
```cal``` shows you a table view representation of the calendar for the current month.
# history
```
user@host ~$ history
```
When you do arrow up on your keyboard, you will get the previous command and if you repeat that you will get the one before. This is the case because your commands are kept in a history. This is a buffer that will overwrite itself when the maximum number of entries is obtained. 

If you want to have a look at what got typed in you use the ```history``` command. Notice that if your command line contains a **password** this will thus be logged too and from a security standpoint this is a very bad situation. When somebody would obtain the login and password for an account all they need to do is type in ```history``` and they could potentially obtain credentials if the credentials are on the command line.

From a defensive point of view it is important to set the buffer of the commandline large enough so that if a system got hacked and the attacker did not whipe the history we maximize our chances the attackers commands are in the command line.
