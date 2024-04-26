# Solutions of OverTheWire Bandit



## Level 0 
### To ssh into a server

	ssh username@host -p port_no
 
## Level 0 → Level 1
### The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

	cat readme

## Level 1 → Level 2 
### The password for the next level is stored in a file called - located in the home directory

	cat ./-

## Level 2 → Level 3
### The password for the next level is stored in a file called spaces in this filename located in the home directory

	cat spaces\in\this\filename
 
## Level 3 → Level 4
### The password for the next level is stored in a hidden file in the inhere directory.

	cd inhere
	cat .hidden
 
## Level 4 → Level 5
### The password for the next level is stored in the only human-readable file in the inhere directory.

	cd inhere
 	ls -la 
  	cat ./-file00 ... cat ./-file07 
   
## Level 5 → Level 6
### The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:  
-human-readable

-1033 bytes in size

-not executable
   
	find . -type f -size 1033c ! -executable
 	cat ./maybehere07/.file2
 
## Level 6 → Level 7
### The password for the next level is stored somewhere on the server and has all of the following properties:
-owned by user bandit7

-owned by group bandit6

-33 bytes in size

	find / -type f -user bandit7 -group bandit6 -size 33c
 	cat /var/lib/dpkg/info/bandit7.password

## Level 7 → Level 8
### The password for the next level is stored in the file data.txt next to the word millionth

	strings data.txt |grep  "millionth"

## Level 8 → Level 9
### The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

	sort data.txt | uniq -c

## Level 9 → Level 10
### The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

	strings data.txt | grep "="

## Level 10 → Level 11
### The password for the next level is stored in the file data.txt, which contains base64 encoded data

	base64 -d data.txt

## Level 11 → Level 12
### The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

	cat data.txt 
 	copy encrypdata
	Search rot13 decoder
 	paste encrypdata

## Level 12 → Level 13
### The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!)

	mkdir /tmp/username
 	cp data.txt -t /tmp/username
  	cd /tmp/username
   	xxd -r data.txt > reverse (hexa to binary)    	
	mv reverse reverse.gz (to change the extension)
	gzip -d reverse.gz (the extracted file now has bzip2 extension)
	mv reverse reverse.bz2 (to change the extension)
	bzip2 -d reverse.bz2 (the extracted file now has gzip extension)
	mv reverse reverse.gz (to change the extension)
	gzip -d reverse.gz 
 	mv reverse reverse.tar
  	tar -xf reverse.tar 
   	mv data5.bin data5.tar
	tar -xf data5.tar
	mv data6.bin data6.bz2
	bzip2 -d data6.bz2
	mv data6 data6.tar
	tar -xf data6.tar
 	mv data8.bin data8.gz
  	gzip -d data8.gz
   	cat data8

## Level 13 → Level 14
### The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on

	ssh -i sshkey.private bandit14@localhost -p 2220

## Level 14 → Level 15
### The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.
	
	nc localhost 30000
 	paste password

## Level 15 → Level 16
### The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.
Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…

	ncat --ssl localhost 30001
 	paste password

## Level 16 → Level 17
### The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

	nmap localhost -p 31000-32000
 	ncat --ssl localhost OPENPORTS
  	paste password
   	
## Level 17 → Level 18
### There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

	diff password.new password.old

## Level 18 → Level 19
### The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

	ssh -t  bandit18@bandit.labs.overthewire.org -p 2220 '/bin/sh'
	cat readme

 ## Level 19 → Level 20
 ### To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

 	./bandit20-do cat /etc/bandit_pass/bandit20

## Level 20 → Level 21
### There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

NOTE: Try connecting to your own network daemon to see if it works as you think

	nc -l -p 4444 
 	./suconnect 4444
  	paste password for the both tabs

## Level 21 → Level 22
### A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

	cd /etc/cron.d
 	ls -la
  	cat cronjob_bandit22
   	cat /usr/bin/cronjob_bandit22.sh
	cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv

## Level 22 → Level 23
### A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

	cd /etc/cron.d
 	ls -la
  	cat cronjob_bandit23
   	myname=bandit23
	$(echo I am user $myname | md5sum | cut -d ' ' -f 1)
	cat /tmp/8ca319486bfbbc3663ea0fbe81326349

## Level 23 → Level 24
### A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

	cd /etc/cron.d
	ls -la
	cat cronjob_bandit24
	cd /tmp
	mkdir shell
	nano bash.sh
		!/bin/bash
		cat /etc/bandit_pass/bandit24 > /tmp/sh/pass.txt
  	touch pass.txt
	chmod 777 bash.sh
	chmod 777 pass.txt
	cp bash.sh /var/spool/bandit24/foo
	Wait 1 minute 
	cat pass.txt

## Level 24 → Level 25
### A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing. You do not need to create new connections each time

	cd /tmp
	mkdir test 
	cd test
	nano pass.sh
		#!/bin/bash
		b24_pass=PASSWORD
		for i in {1111..9999};do
        	echo "$b24_pass $i"
		done | nc localhost 30002
	chmod 777 pass.sh
	./pass.sh

## Level 25 → Level 26
### Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.
	
	ssh -i bandit26.sshkey bandit26@♠localhost -p 2220
 	minimize the terminal and re-enter the code until you see "more" keyword
  	When you see "more" keyword press v
   	press ESC on Vim tab
	:set shell=/bin/bash
	press ESC
	:shell
	cat /etc/bandit_pass/bandit26

## Level 26 → Level 27
### Good job getting a shell! Now hurry and grab the password for bandit27!

 	ls
  	./bandit27-do cat  /etc/bandit_pass/bandit27

## Level 27 → Level 28
### There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo via the port 2220. The password for the user bandit28-git is the same as for the user bandit28.

Clone the repository and find the password for the next level.

	cd /tmp
	mkdir test1
	git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
	paste B27PASSWORD
	cd repo
	cat README

## Level 28 → Level 29
### There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo via the port 2220. The password for the user bandit28-git is the same as for the user bandit28.

Clone the repository and find the password for the next level.
	
	cd /tmp
	mkdir test1
	git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo
	paste B28PASSWORD
	cd repo
	cat README.md
	git log
	git checkout COMMITID
	cat README.md

## Level 29 → Level 30
### There is a git repository at ssh://bandit29-git@localhost/home/bandit29-git/repo via the port 2220. The password for the user bandit29-git is the same as for the user bandit29.

Clone the repository and find the password for the next level.
 
	cd /tmp
	mkdir test1
	git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
	paste B29PASSWORD
	cd repo
	cat README.md
	git branch -r
	git checkout dev
	cat README.md

## Level 30 → Level 31 
### There is a git repository at ssh://bandit30-git@localhost/home/bandit30-git/repo via the port 2220. The password for the user bandit30-git is the same as for the user bandit30.

Clone the repository and find the password for the next level.
	
	cd /tmp
	mkdir test1
	git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo
	paste B30PASSWORD
	cd repo
	cat README.md
	git tag
	git show secret

## Level 31 → Level 32
### There is a git repository at ssh://bandit31-git@localhost/home/bandit31-git/repo via the port 2220. The password for the user bandit31-git is the same as for the user bandit31.

Clone the repository and find the password for the next level.

	cd /tmp
	mkdir test1
	git clone ssh://bandit31-git@localhost:2220/home/bandit31-git/repo
	paste B31PASSWORD
	cd repo
	cat README.md
	rm .gitignore
	echo "May I come in" > key.txt
	git add key.txt
	git commit -m "new commit"
	git push

## Level 32 → Level 33
### After all this git stuff its time for another escape. Good luck!

	$0
	export SHELL = /bin/bash
	$SHELL
	cat /etc/bandit_pass/bandit33







 
