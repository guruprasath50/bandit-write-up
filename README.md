## Over the Wire: Bandit

- If you know a command, but don’t know how to use it, try the manual (man page) by entering “man <command>” (without the quotes). 
- If there is no man page, the command might be a shell built-in. In that case use the “help <X>” command.
___
# Level 0
*To ssh onto a server*
~~~
ssh {user}@{Host} -p {port_no}
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
ls -ld .?* 
~~~  
Will only list hidden files .  
Explain :
~~~
 -l     use a long listing format
 -d, --directory
              list  directory entries instead of contents, and do not derefer‐
              ence symbolic links

.?* will only state hidden files 
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
~~~
file [option] [filename]
file -b filename
file *
file directoryname/*
~~~
- -b, –brief : This is used to display just file type in brief mode.
- \* option : Command displays the all files’s file type.
- directoryname/* option : This is used to display all files filetypes in particular directory.
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

*Using find command to filter and get a specific file*  
  
We can use 'find' command to get a precise file by filtering.  
Eg: in the command used up, we searched uptill the 2nd sub-diectory (-maxdepth 2) and we searched for files (-type f) and not directories (-type d) and we found the file by the exact size of the file by (-size 1033).  
For more info about the 'find' command:  
https://linuxize.com/post/how-to-find-files-in-linux-using-the-command-line/

*NOTE:* To print the contents of the whole directory tree i.e. all the sub folders and files in that sub folder, we can use
~~~
ls -R
~~~ 
*NOTE:* While printing size -s prints there are 2 sizes, one initially (first col) and the other later (after the date). The size mentioned in the first col is the block size which is a multiple of 4kb, to store the file with size <=4kb and the one mentioned later is the actual size of the file.  
Another thing is when we use,  
~~~
ls -l -s
~~~
We get the size of the files in bytes eg: 1039. But when we use,  
~~~
ls -l -h  
~~~
We get sizes in human readable format eg: 1.1K
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
*'Rot13' Encryption / Decryption*  
Rot13 is an encryption/decryption algorithm that shifts each letter of the english alphabet by 13 places (leaving numbers the same).  
Eg: A <-> N | B <-> O | C <-> P  
This is a symmetric algo. as HELLO <-> URYYB and vice-versa.
*The 'trace' Command*  
The tr command in UNIX is a command line utility for translating or deleting characters. It supports a range of transformations including uppercase to lowercase, squeezing repeating characters, deleting specific characters and basic find and replace. It can be used with UNIX pipes to support more complex translation.  
For additional info: https://shapeshed.com/unix-tr/
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
The '-i' means identity that is used to take input of the private key from the file (The file can be in .txt format also).  
For good info about SSH:  
https://help.ubuntu.com/community/SSH/OpenSSH/Keys
For info about Internet and Networking:  
https://help.ubuntu.com/community/InternetAndNetworking
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
# Level 20 -> 21
~~~
bandit20@bandit:~$ ./suconnect
Usage: ./suconnect <portnumber>
This program will connect to the given port on localhost using TCP. If it receives the correct password from the other side, the next password is transmitted back.

bandit20@bandit:~$ ./suconnect 8000

bandit20@bandit:~$ nc -lvp 8000
listening on [any] 8000 ...
connect to [127.0.0.1] from localhost [127.0.0.1] 48578
GbKksEFF4yrVs6il55v6gwY5aVje5f0j
gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr
~~~
*Running Parellel Tasks*  
./suconnect is a setuid binary that runs the program on the port mentioned, and we need to open a parellel window along with it (tmux helps, but i did'nt use it). On the other window, we listen to the server running, which when I send the current password returns the next.
___
# Level 21 -> 22
~~~
bandit21@bandit:~$ cd /etc/cron.d/
bandit21@bandit:/etc/cron.d$ ls
atop  cronjob_bandit22  cronjob_bandit23  cronjob_bandit24
bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI
~~~
*cron processes*  
Cron’s format is a little unorthodox so let’s take a look. there are 5 spaces for specifying what time to run the script or program. If the space isn’t needed a star is used as a placeholder. The first position is used to denote , the minute using a value between 0-59. The second one is hour, using the 24 hour clock, meaning values between 0-23. The third place is used for day of the month,  using values ranging from 1-31. The fourth space is month, specified by numbers between 1-12. The fifth position is used for day of the week, depending on the distribution 0 or 7 could be Sunday, I recommend checking on any distribution that you’re not sure about. The next part of a cron job is the username that executes the job. The final part of the cron job command is the script or program that is to be executed.

~~~
*     *    *    *     *        command to be executed
–     –    –    –    –
|     |     |     |    |
|     |     |     |    +—– day of week (0 – 6) (Sunday=0)
|     |     |     +——- month (1 – 12)
|     |     +——— day of        month (1 – 31)
|     +———– hour (0 – 23)
+————- min (0 – 59)
~~~
NOTE: Here the process is repeating every minute, and that is reboot '/usr/bin/cronjob_bandit22.sh' shell file which changes the permission and copies the passowrd in the /tmp/ file.
___
# Level 22 -> 23
~~~
bandit22@bandit:/etc/cron.d$ ls
atop  cronjob_bandit22  cronjob_bandit23  cronjob_bandit24
bandit22@bandit:/etc/cron.d$ cat cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
bandit22@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
bandit22@bandit:/etc/cron.d$ echo I am user bandit23 | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349
bandit22@bandit:/etc/cron.d$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n
~~~
*Hashing function and delimeter*
Here the output of 'I am user $myname' was hashed using md5 and the first worf of the file was taken (' ' was the delimeter/seperation).  
But we using whoami we used to get bandit22, and we have the password for that. So we just tried out luck to understand the bash file and do the same of 'bandit23' and luckily it worked!
___
# Level 23 -> 24
~~~
bandit23@bandit:~$ cat /etc/cron.d/cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
bandit23@bandit:~$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash
myname=$(whoami)
cd /var/spool/$myname
echo "Executing and deleting all scripts in /var/spool/$myname:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        timeout -s 9 60 ./$i
        rm -f ./$i
    fi
done

bandit23@bandit:~$ mkdir -p /tmp/gxuvimr
bandit23@bandit:~$ cd /tmp/gxuvimr
bandit23@bandit:/tmp/gxuvimr$ touch pass.sh
bandit23@bandit:/tmp/gxuvimr$ chmod 777 pass.sh
bandit23@bandit:/tmp/gxuvimr$ vim pass.sh


#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/gxuvimr/pass


bandit23@bandit:/tmp/gxuvimr$ cat pass
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ
~~~
*Writing the script*  
Here we basically wrote a script, as each file was excecuting before getting deleted. We copied a script so that the password gets copied to our folder in /tmp/.
___
# Level 24 -> 25
~~~
#!/bin/bash
pass=UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ

for i in {0000..9999}
do
	echo $i >> out.txt
	echo $pass $i | nc localhost 30002 >> out.txt &
	pid=$!
	sleep 1
	kill $pid
	echo "" >> out.txt
done
__
Correct!
The password of user bandit25 is uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG
~~~
*BruteForcing and finding PID of a process*  
Basically created a bash script to take each number and check for the password i.e. a for loop. '$!' gives the PID of the process and is killing it after excecution (after a sleep of 1 sec). The meaning of 'kill $pid' is a replacement of 'Ctrl + C' in the bash script.
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
bandit10  |	 truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk
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
bandit21  |  gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr
bandit22  |	 Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI
bandit23  |  jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n
bandit24  |  UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ
bandit25  |  uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG
bandit26  |  5czgV9L3Xx8JPOyRbXh6lQbmIOWvPT6Z
bandit27  |  3ba3118a22e93127a4ed485be72ef5ea
bandit28  |  0ef186ac70e04ea33b4c1853d2526fa2
bandit29  |  bbc96594b4e001778eee9975372716b2
bandit30  |  5b90576bedb2cc04c86a9e924ce42faf
bandit31  |  47e603bb428404d265f59c42920d81e5
bandit32  |  56a9bf19c63d650ce78e6ec0354ee45e
bandit33  |  c9c3199ddf4121b10cf581a98d51caee
bandit34  |  CHALLENGE COMPLETED
