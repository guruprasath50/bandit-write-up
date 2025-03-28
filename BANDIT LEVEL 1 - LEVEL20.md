## Over the Wire: Bandit

- If you know a command, but don’t know how to use it, try the manual (man page) by entering “man <command>” (without the quotes). 
- If there is no man page, the command might be a shell built-in. In that case use the “help <X>” command.
___
# Level 0
*To ssh onto a server*
~~~
ssh bandit0@bandit.labs.overtehwire.org -p 2220
~~~
*SSH (Secure Socket Shell)*
Secure Shell (SSH) is a cryptographic protocol that provides communications security over a computer network, connecting an SSH client application with an SSH server. It is typically used to access shell accounts on remote servers. Shell accounts are typically available on Linux systems (but not only) and provide a user interface to the operating system’s services for the purpose of system management.
___
# Level 0 -> 1
~~~
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme
boJ9jbbUNNfktd78OOpsqOltutMc3MY1
~~~
*cat*  
Used to read .txt files

*To exit from SSH session*
1. closing the shell session, e.g. with exit followed by Enter , or Ctrl - d usually allows you to exit the ssh session normally,
2. in the case where you have a bad connection and the shell is unresponsive, hit the Enter key, then type ~. and ssh should immediately close and return you to your command prompt.
___
# Level 1 -> 2
~~~
bandit1@bandit:~$ ls
'-'
bandit1@bandit:~$ cat ./-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
~~~
As '-' is a special keyword, that might be used in a lot of places, it might create a misunderstanding while opening or reading the file.  
So instead of cat '-'', we will use it as './-' whereever we have to refer the '-' named file.
So use 'cat ./-'
___
# Level 2 -> 3
~~~
bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ cat 'spaces in this filename'
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
~~~

*Handling files whose names contain spaces*  
To handle files containing spaces we generally include them in single quotes,  
like 'filename with spaces',  
or use backslash,  
eg: filename\ with\ spaces
___
# Level 3 -> 4
~~~
bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ cd inhere/
bandit3@bandit:~/inhere$ ls
bandit3@bandit:~/inhere$ ls -a
.  ..  .hidden
bandit3@bandit:~/inhere$ cat .hidden
pIwrPrtPN36QITSp3EQaw936yaFoFgAB
~~~

*Handling  hidden files on the terminal*  
'ls -a' shows all files, hidded as well as not-hidden files.  
NOTE: If we only want to see the hidden files: 
The command :  
~~~

~~~
___
# Level 4 -> 5
~~~
bandit4@bandit:~$ ls
inhere
bandit4@bandit:~$ file inhere/*
inhere/-file00: data
inhere/-file01: data
inhere/-file02: data
inhere/-file03: data
inhere/-file04: data
inhere/-file05: data
inhere/-file06: data
inhere/-file07: ASCII text
inhere/-file08: data
inhere/-file09: data
bandit4@bandit:~$ cat inhere/-file07
koReBOKuIDDepwhWk7jZC0RTdopnAYKh
~~~
**Human readable text format**  
Human readable text is a text that is written in ASCII format or a format readble to humans and not data or any binary format.  

'file' command is used to determine the type of a file. .file type may be of human-readable(e.g. ‘ASCII text’) or MIME type(e.g. ‘text/plain; charset=us-ascii’). This command tests each argument in an attempt to categorize it.
___
# Level 5 -> 6
~~~
bandit5@bandit:~$ cd inhere
bandit5@bandit:~/inhere$ ls
maybehere00  maybehere02  maybehere04  maybehere06  maybehere08  maybehere10  maybehere12  maybehere14  maybehere16  maybehere18
maybehere01  maybehere03  maybehere05  maybehere07  maybehere09  maybehere11  maybehere13  maybehere15  maybehere17  maybehere19
bandit5@bandit:~/inhere$ find -maxdepth 2 -type f -size 1033c
./maybehere07/.file2
bandit5@bandit:~/inhere$ cat ./maybehere07/file2
cat: ./maybehere07/file2: No such file or directory
bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
DXjZPULLxYr17uwoI01bNLQbtFemEgo7
~~~
___
# Level 6 -> 7
~~~
bandit6@bandit:/$ pwd
/
bandit6@bandit:/$ find -type f -size 33c -user bandit7 -group bandit6 2>/dev/null
./var/lib/dpkg/info/bandit7.password
bandit6@bandit:/$ cat ./var/lib/dpkg/info/bandit7.password
HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
~~~
*Traversing out of 'home' directory & '2>/dev/null'*  
As the file was somewhere in the server, we had to go out of the main directory and traverse right to the root to find the file.  
Also when we ran 
~~~
find -type f -size 33c -user bandit7 -group bandit6
~~~
Alot of files appeared with alot of errors like permission denied etc. Therefore, we added '2>/dev/null'.  
Essentially by typing ‘2>/dev/null’ you are saying send the errors to /dev/null.   
- ‘2>’ is the redirection operator for standard error (stderr for short). 
- For the sake of simplicity think of /dev/null as a shredder, it just deletes what is sent to it.
___
# Level 7 -> 8
~~~
bandit7@bandit:~$ ls
data.txt
bandit7@bandit:~$ nano data.txt
^W
millionth
~~~
*Searching a word in the file using bash*  
'nano' is a text editor generally present in linux systems. Opened the file throught the editor, and then searched for the word 'millionth'.
Could have also sorted the whole file using,  
~~~
sort data.txt
~~~
But the number of words were too many, so the list went out of the terminal range.  
*NOTE:* For text based strings, 'grep' helps alot. The grep filter searches a file for a particular pattern of characters, and displays all lines that contain that pattern. The pattern that is searched in the file is referred to as the regular expression (grep stands for globally search for regular expression and print out).  
~~~
grep [options] pattern [files]
~~~
*Options Description*
~~~
-c : This prints only a count of the lines that match a pattern
-h : Display the matched lines, but do not display the filenames.
-i : Ignores, case for matching
-l : Displays list of a filenames only.
-n : Display the matched lines and their line numbers.
-v : This prints out all the lines that do not matches the pattern
-e exp : Specifies expression with this option. Can use multiple times.
-f file : Takes patterns from file, one per line.
-E : Treats pattern as an extended regular expression (ERE)
-w : Match whole word
-o : Print only the matched parts of a matching line,
 with each such part on a separate output line.
~~~
___
# Level 8->9
~~~
bandit8@bandit:~$ sort data.txt | uniq -u
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
~~~
*Performing multiple bash commands together & finding unique element in the file*  
The command to remove all duplicates is,  
~~~
uniq data.txt
~~~
But to print the string that is present only once (unique in the file) is uniq -u. But this command requires the file to be sorted alphabetically, so we need to do that first. But as we did not have permissions to save the sorted file as another file, we had to run the command simultaneously. Therefore,   
~~~
sort data.txt | uniq -u
~~~
___
# Level 9 -> 10
~~~
bandit9@bandit:~$ strings data.txt
~~~
*Extracting human readable text from File*  
To get human-readable text from the file (ASCII type), we use the strings function, and as told the password starts with a number of '=' signs.  
___
# Level 10 -> 11
~~~
bandit10@bandit:~$ ls
data.txt
bandit10@bandit:~$ cat data.txt
VGhlIHBhc3N3b3JkIGlzIElGdWt3S0dzRlc4TU9xM0lSRnFyeEUxaHhUTkViVVBSCg==
bandit10@bandit:~$ base64 -d data.txt
The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
~~~
*Understanding Base64 / Encoding Algorithms*  
'Base64' also known as 'Base64 Content-Transfer-Encoding'  
Base64 is an encoding and decoding technique used to convert binary data to an American Standard for Information Interchange (ASCII) text format, and vice versa.   
It is used to transfer data over a medium that only supports ASCII formats, such as email messages on Multipurpose Internet Mail Extension (MIME) and Extensible Markup Language (XML) data.  
Like binary data, Base64 encoded resultant data is not human readable. It helps to convert text from one format to another, just like Binary, but Base64 conversions are way smaller than Binary which makes them easier to transfer/send.
___
# Level 11 -> 12
~~~
bandit11@bandit:~$ ls
data.txt
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf 5Gr8L4qetPEsPk8htqjhRK8XSP6x2RHh
bandit11@bandit:~$ echo | cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
The password is 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu
~~~

___
# Level 12 -> 13
~~~
bandit12@bandit:/tmp/gxuvimr$ xxd -r data.txt > data
__

bandit12@bandit:/tmp/gxuvimr$ ls
data  data.txt

bandit12@bandit:/tmp/gxuvimr$ file data
data: gzip compressed data, was "data2.bin", last modified: Tue Oct 16 12:00:23 2018, max compression, from Unix
bandit12@bandit:/tmp/gxuvimr$ mv data data.gz
bandit12@bandit:/tmp/gxuvimr$ gzip -d data.gz

bandit12@bandit:/tmp/gxuvimr$
bandit12@bandit:/tmp/gxuvimr$ file data
data: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/gxuvimr$ mv data data.bz2
bandit12@bandit:/tmp/gxuvimr$ bzip2 -d data.bz2

bandit12@bandit:/tmp/gxuvimr$ file data
data: gzip compressed data, was "data4.bin", last modified: Tue Oct 16 12:00:23 2018, max compression, from Unix
bandit12@bandit:/tmp/gxuvimr$ mv data data.gz
bandit12@bandit:/tmp/gxuvimr$ gzip -d data.gz

bandit12@bandit:/tmp/gxuvimr$ file data
data: POSIX tar archive (GNU)
bandit12@bandit:/tmp/gxuvimr$ mv data data.tar
bandit12@bandit:/tmp/gxuvimr$ tar -xvf data.tar
data5.bin
bandit12@bandit:/tmp/gxuvimr$ ls
data5.bin  data.tar  data.txt

bandit12@bandit:/tmp/gxuvimr$ file data5.bin
data5.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/gxuvimr$ mv data5.bin data5.tar
bandit12@bandit:/tmp/gxuvimr$ tar -xvf data5.tar
data6.bin

bandit12@bandit:/tmp/gxuvimr$ file data6.bin
data6.bin: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/gxuvimr$ mv data6.bin data6.bz2
bandit12@bandit:/tmp/gxuvimr$ bzip2 -d data6.bz2

bandit12@bandit:/tmp/gxuvimr$ file data6
data6: POSIX tar archive (GNU)
bandit12@bandit:/tmp/gxuvimr$ mv data6 data6.tar
bandit12@bandit:/tmp/gxuvimr$ tar -xvf data6.tar
data8.bin

bandit12@bandit:/tmp/gxuvimr$ file data8.bin
data8.bin: gzip compressed data, was "data9.bin", last modified: Tue Oct 16 12:00:23 2018, max compression, from Unix
bandit12@bandit:/tmp/gxuvimr$ mv data8.bin data8.gz
bandit12@bandit:/tmp/gxuvimr$ gzip -d data8.gz

bandit12@bandit:/tmp/gxuvimr$ file data8
data8: ASCII text
bandit12@bandit:/tmp/gxuvimr$ cat data8
The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL
bandit12@bandit:/tmp/gxuvimr$
~~~
*/tmp/ and uncompression of files of various types and understanding Hexdump*  
The /tmp directory contains mostly files that are required temporarily, it is used by different programs to create lock files and for temporary storage of data.   

- Basically it is used to store temporary files, so we first created a directory there. and then copied our 'data.txt' there to work with, which contained a 'Hexdump'.  

A Hexdump is a utility that displays the contents of binary files in hexadecimal, decimal, octal, or ASCII. It’s a utility for inspection and can be used for data recovery, reverse engineering, and programming. Basically machine level code, not stored in Binary, but in HexaDecimal format. 

- So we converted the hexdump into binary to perform various operations on it. The 'xxd' command generally converts from binary to hexadecimal, but the -r helps it do the reverse. Using-
~~~
xxd -r data.txt > data
~~~
- Now once we had the data, we checked it's type by the 'file' command, to find out that they were in various compression formats like .gzip .bzip2 and .tar. So then we renamed the data files to data.{format} by 'mv' command. And then decompressed it accordingly using their commands.
~~~
gzip -d data.gz
bzip2 -d data.bz2
tar -xvf data.tar
~~~
We did this until the type of the file was ASCII. Then we 'cat' the file to get the password.
___
# Level 13 -> 14
~~~
bandit13@bandit:~$ ssh -i sshkey.private bandit14@localhost
__
bandit14@bandit:/etc/bandit_pass$ cat bandit14
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
~~~
*SSH into a server using RSA/Private key*  
We can SSH into a server using the private key / the RSA key by:
~~~
ssh -i {privatekey_file} {username}@{server} -p {port_no}
~~~
___
# Level 14 -> 15
~~~
bandit14@bandit:/etc/ssh$ telnet localhost 30000
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
Correct!
BfMYroe26WYalil77FoDi9qh59eK5xNr
~~~
*Using Telnet to communicate to various servers through particular ports*  
Telnet is a command that helps to communicate to various servers through various ports. It's more of a text communication line. Request-Response kind of convo.
___
# Level 15 -> 16
~~~
bandit15@bandit:~$ openssl s_client -quiet -connect localhost:30001
depth=0 CN = localhost
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = localhost
verify return:1
BfMYroe26WYalil77FoDi9qh59eK5xNr
Correct!
cluFn7wTiGryunymYOu4RcffSxQluehd
~~~
*Using the OpenSSL Client to connect and interact with SSL Certified Servers*   
Secure Sockets Layer (SSL) is a standard security technology for establishing an encrypted link between a server and a client—typically a web server (website) and a browser, or a mail server and a mail client (e.g., Outlook).  
SSL allows sensitive information such as credit card numbers, social security numbers, and login credentials to be transmitted securely. Normally, data sent between browsers and web servers is sent in plain text—leaving you vulnerable to eavesdropping. If an attacker is able to intercept all data being sent between a browser and a web server, they can see and use that information.   
We used the OpenSSL client to interact with SSL cerified servers. We would have gotten the output even if we hadn't mentioned -quiet, we added that to avoid the info not of our interest (info about the connection) such as session id, ssl certificate etc.
___
# Level 16 -> 17
~~~
bandit16@bandit:~$ nmap -p 31000-32000 localhost

Starting Nmap 7.40 ( https://nmap.org ) at 2020-04-15 21:26 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00031s latency).
Not shown: 999 closed ports
PORT      STATE    SERVICE
31518/tcp filtered unknown
31790/tcp open     unknown

bandit16@bandit:~$ openssl s_client -quiet -connect localhost:31790
depth=0 CN = localhost
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = localhost
verify return:1
cluFn7wTiGryunymYOu4RcffSxQluehd
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----
~~~
*Finding open ports and their services*
We scanned all the ports from 31000 to 32000 to see which ports are up and running. The result of a scan on a port is usually generalized into one of three categories:
- Open or Accepted: The host sent a reply indicating that a service is listening on the port.
- Closed or Denied or Not Listening: The host sent a reply indicating that connections will be denied to the port.
- Filtered, Dropped or Blocked: There was no reply from the host.
~~~
nmap -p 31000-32000 localhost
~~~
Luckily we got only 2 ports running out of which one was open and the other filtered. So only one would give back a response, and we got a hint from the question that it speaks SSL, so we gave the current password to get a private key in return for the next level.
___
# Level 17 -> 18
~~~
Bagga@DESKTOP-Q5EOV5U ~
$ ls -l bandit7.txt
-rw-r--r--+ 1 Bagga Bagga 1676 Apr 16 01:43 bandit7.txt

Bagga@DESKTOP-Q5EOV5U ~
$ chmod 600 bandit7.txt

Bagga@DESKTOP-Q5EOV5U ~
$ ls -l bandit7.txt
-rw-------+ 1 Bagga Bagga 1676 Apr 16 01:43 bandit7.txt

Bagga@DESKTOP-Q5EOV5U ~
$ ssh -i bandit7.txt bandit17@bandit.labs.overthewire.org -p 2220

bandit17@bandit:~$ diff passwords.new passwords.old
42c42
< kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
---
> hlbSBPAWJmL6WFDb06gpTx1pPButblOA
~~~
*Permissions and 'common difference' between files*  
So first I saved the private key in a .txt folder and then tried to ssh through it, but I got an error saying *WARNING: UNPROTECTED PRIVATE KEY FILE!* as the permissions of the file were 644 ie rw-r-r.  
In Unix-like operating systems, the chmod command sets the permissions of files or directories.  
Each digit is a combination of the numbers 4, 2, 1, and 0:  
- 4 stands for "read" [r],
- 2 stands for "write" [w],
- 1 stands for "execute" [x], and
- 0 stands for "no permission."  

NOTE: the format for permissions of a file is "-rwxrwxrwx" and for a directory is "drwxrwxrwx".  
But for a .txt to be a private key it must have 600 permissions meaning only the user can read-write the .txt file and not the group and others.
The permission format is    
-[directory/not] ---[user] ---[group] ---[others]  
& the --- stands for [r][w][x]  
___
# Level 18 -> 19
~~~
Bagga@DESKTOP-Q5EOV5U ~
$ ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x
~~~
*Remote commands while SSH*  
By writing a command after ssh, we can remotely perform a particular task while SSHing into a server. Another alternative way is to-   
~~~
Bagga@DESKTOP-Q5EOV5U ~
$ ssh bandit18@bandit.labs.overthewire.org -p 2220 /bin/bash --norc
ls
readme
cat readme
IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x
~~~
.bashrc is a part of our  “profile” on the remote server that tells the operating system things about our particular profile, such as home directory, preferred shell and text editor, and in our case runs a script that logs us off when we try to ssh in.  
So .bashrc is a startup script that is run whenever a user enters the server or does any particular action.  
*'--norc' means*   
Don’t read the ~/.bashrc initialization file in an interactive shell. This is on by default if the shell is invoked as sh.
And this is present in the /bin/bash folder.
___
# Level 19 -> 20
~~~
bandit19@bandit:~$ ./bandit20-do 
Run a command as another user.
  Example: ./bandit20-do id
bandit19@bandit:~$ ./bandit20-do whoami
bandit20
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
GbKksEFF4yrVs6il55v6gwY5aVje5f0j
~~~
*Understanding UIDs, GIDs, and EUIs*  
Setuid binary's are basically excecutables that basically alloww you to operate as another user or give temporary escalated permissions to users. By giving escallated permissions, a user is given temporary access to excecute a file that it can otherwise not excecute it. To understand if the file can excecuted bu the escalated user, the permissions contain 's' instead of 'x'.
~~~
bandit19@bandit:~$ ls -l ./bandit20-do 
-rwsr-x--- 1 bandit20 bandit19 7296 Oct 16  2018 ./bandit20-do
~~~
They are separated into a few different categories, let’s go over a few.  Uid’s are user identification numbers and are unique to each users. Uids of normal users start at 1000 and are theoretically unlimited, user number 0 is root, 1-99 are reserved for other predefined accounts, and 100-999 are reserved for other system account and groups. gid is a group id.
~~~
bandit19@bandit:~$ ./bandit20-do id
uid=11019(bandit19) gid=11019(bandit19) euid=11020(bandit20) groups=11019(bandit19)
band
~~~

___
# Usernames and Passwords

| Username  | Password |
| ------------- | ------------- |
bandit0	  |  bandit0
bandit1	  |  boJ9jbbUNNfktd78OOpsqOltutMc3MY1
bandit2	  |  CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
bandit3	  |  UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
bandit4	  |  pIwrPrtPN36QITSp3EQaw936yaFoFgAB
bandit5	  |  koReBOKuIDDepwhWk7jZC0RTdopnAYKh
bandit6	  |  DXjZPULLxYr17uwoI01bNLQbtFemEgo7
bandit7	  |  HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
bandit8	  |  cvX2JJa4CFALtqS87jk27qwqGhBM9plV
bandit9	  |  UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
bandit10  |  truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk
bandit11  |  IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
bandit12  |	 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu
bandit13  |	 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL
bandit14  |	 4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
bandit15  |	 BfMYroe26WYalil77FoDi9qh59eK5xNr
bandit16  |	 cluFn7wTiGryunymYOu4RcffSxQluehd
bandit17  |	 xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn
bandit18  |	 kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
bandit19  |	 IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x
bandit20  |	 GbKksEFF4yrVs6il55v6gwY5aVje5f0j
