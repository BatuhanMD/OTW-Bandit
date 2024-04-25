# OTW-Bandit

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

 	
 
