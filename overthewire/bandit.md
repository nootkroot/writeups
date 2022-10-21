*Some screenshots may look different and that's becaue I'm working on this on different devices. Just ignore it lol. Also keep in mind these writeups are just for my own understanding so it may not feel like it's helpful.*

# Level 0 &rarr; Level 1

## Level Goal
>The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

## Solution
Read from the `readme` file using `cat` giving us the password for the next level

![image](https://user-images.githubusercontent.com/65555981/196868347-b4bc3312-39b7-4b08-8f1f-c5603a652790.png)

### password: NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL

# Level 1 &rarr; Level 2

## Level Goal
>The password for the next level is stored in a file called - located in the home directory

## Solution
When `-` is used by itself, Unix systems will intrepret that as stdin/stdout (from what I understood), so we need to specify the file by prepending `./`. The full command would be `cat ./-`.

![image](https://user-images.githubusercontent.com/65555981/196870218-69579d5e-9d0b-4ba7-8b27-3130e20157eb.png)

### password: rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi

# Level 2 &rarr; Level 3

## Level Goal
> The password for the next level is stored in a file called **spaces in this filename** located in the home directory

## Solution
There are two ways to solve this, you can either put the file name in double quotes or you can escape each space with a backslash.

![image](https://user-images.githubusercontent.com/65555981/196984616-74da6321-9c11-425f-a1b6-275b6c15f989.png)

### password: aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG

# Level 3 &rarr; Level 4

## Level Goal
> The password for the next level is stored in a hidden file in the inhere directory.

## Solution
At first, when I go into the `inhere` folder and run `ls`, I notice that don't see any files, though not suprising since we're told the file is hidden. If we look at the man page for `ls`, we see that theres a flag `-a` which doesn't ignore file names starting with a dot. When I ran `ls -a` I saw that theres a file called `.hidden`. Running `cat` on the file gives us the password.

![image](https://user-images.githubusercontent.com/65555981/196988883-5d552491-4f89-477c-9ab9-12979cc9eb40.png)

### password: 2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe

# Level 4 &rarr; Level 5

## Level Goal
> The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.

## Solution
There are two ways I found to do this (other than manually looking.) The first was to run the `strings` command on all the files at once using the wilcard `*`.
This gave me an output that included other human-readable strings but one of them stood out, which just happened to be the password.

![image](https://user-images.githubusercontent.com/65555981/196991921-cfa39c2f-554f-4375-849e-ee834c7c5099.png)

The other way I found of doing this seemed a lot nicer, and that was to look at what type of data was in each file using the `file` command. I almost forgot that I had to add `./` before the dash, even while using the wildcard.

![image](https://user-images.githubusercontent.com/65555981/196992404-08c3fc8e-1c3c-4d6f-a920-e4dcf5181443.png)

Two files stand out here. One is a RSA key, and the other is just some text. Since I're looking for human-readable text, I decided to check that file first, and I would end up being correct.

![image](https://user-images.githubusercontent.com/65555981/196992816-ca151fda-79fe-4cd5-ba6e-264c6049bed0.png)

### password: lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR

# Level 5 &rarr; Level 6

## Level Goal
> The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:
> - human-readable
> - 1033 bytes in size
> - not executable

## Solution
When I went into the `inhere` directory, I noticed that all of the contents are directories. I think about what to do and realize I could use the `find`, although I was unsure what parameters you could specify. Looking at the man page for `find`, I see a size argument and a type argument, and since `find` already looks recusively, I can use these to find the password. I crafted the command `find . -size 1033c -type f`. When ran it output a single file, `./maybehere07/.file2`. When I printed the file, the password was shown.

![image](https://user-images.githubusercontent.com/65555981/197009896-7f840eeb-fb80-4230-b641-c6356438a255.png)

The empty space is just to get the file to the size of 1033 bytes.

### password: P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU

# Level 6 &rarr; Level 7

## Level Goal
> The password for the next level is stored **somewhere on the server** and has all of the following properties:
> - owned by user bandit7
> - owned by group bandit6
> - 33 bytes in size

## Solution
Once again, we can use the `find` command to find the file. Going back to the man page, we see the arguments `-group` and `-user`, and we already know about the `-size` arugment. Another difference is that we're looking through the entire server, not just our home directory, meaning instead of searching `.` we search through the root directory (`/`). I crafted the command `find / -size 33c -group bandit6 -user bandit7` however this spit out a lot of errors. The file was still there, but hard to find. I wanted this to look cleaner so I added `2>/dev/null` which takes all stderr message and throws it away. Doing this gives us a much cleaner output.

![image](https://user-images.githubusercontent.com/65555981/197013126-ecc0f99d-6e83-49a9-9903-114c39bd9d6b.png)

Reading from the file gives us the password

![image](https://user-images.githubusercontent.com/65555981/197013277-8dddcc7e-99b1-4819-bd0b-b23f3a6bb440.png)

### password: z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S

# Level 7 &rarr; Level 8

## Level Goal
> The password for the next level is stored in the file **data.txt** next to the word **millionth**

## Solution
Looking at data.txt, we see that each line has a normal english word and to the right of it is a "password"

![image](https://user-images.githubusercontent.com/65555981/197014206-6f7359ac-2c8a-4dd1-b890-304638085ca3.png)
###### *`head` prints the first 10 lines of a file*

For this challenge we can use the `grep` command. According to the man page, "*`grep` searches the named input FILEs (or standard input if no files are named, or if a single hyphen-minus (-) is given as file name) for lines containing a match to the given PATTERN. By default, `grep` prints the matching lines.*" All we need to do is pipe the content of `data.txt` to grep, and we can do that using `|`. The full command would look like `cat data.txt | grep millionth`. Running this commnad gives us the password.

![image](https://user-images.githubusercontent.com/65555981/197015885-b040d0ef-c785-4e21-bb48-4b97a329c43f.png)

### password: TESKZC0XvTetK0S9xNwm25STk5iWrBvP

# Level 8 &rarr; Level 9

## Level Goal
> The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once

## Solution
The first thing that came to mind when I read "occurs only once" was the command `uniq`. `uniq` will remove all duplicates by default, however with the `-u` flag it will only print unique lines. There's one more problem, in `uniq` only check adjacent lines for duplicates, not through the entire file. That means the file has to be sorted first before running `uniq`. We can use the `sort` command to sort the lines of the file. The full command would be `sort data.txt | uniq -u`.

![image](https://user-images.githubusercontent.com/65555981/197019118-6f7c7f10-e51d-46b0-b152-d454b39e107c.png)

### password: EN632PlfYiZbn3PhVK3XOGSlNInNE00t

# Level 9 &rarr; Level 10

## Level Goal
> The password for the next level is stored in the file **data.txt** in one of the few human-readable strings, preceded by several ‘=’ characters.

## Solution
All I did was grep the file for at least 3 "=" and it gave me the password.

![image](https://user-images.githubusercontent.com/65555981/197020811-06a9de5c-51e0-44ab-89af-bc1dff06bc4c.png)

### password: G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s

# Level 10 &rarr; Level 11

## Level Goal
> The password for the next level is stored in the file **data.txt**, which contains base64 encoded data

## Solution
Base64 is a simply a different way to represent numbers a data. Instead of it being represented in base-10 (decimal) or base-2 (binary), it has 64 unique (digits) it uses to represent data. We can decode this data in linux with the `base64` command with the `-d` flag. Doing this on the contents of `data.txt` gives us the password.

![image](https://user-images.githubusercontent.com/65555981/197021794-60acd543-70cd-4439-92b8-fedc2ac8252d.png)

### password: 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM

# Level 11 &rarr; Level 12

## Level Goal
> The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

## Solution
`data.txt` is encrypted using ROT13. ROT13 simply rotates each letter by 13 positions. In the case that it goes over 26, it loops back around. Decoding ROT13 can be done on linux with the `tr` command. `tr` translates the characters in its first argument with the characters in it's second. The command to decode ROT13 would look like `tr 'A-Za-z' 'N-ZA-Mn-za-m'`. So 'A' would be replaced with 'N' and 'Z' would be replaced with 'M'. Running this command on the contents of `data.txt` gives us the password.

![image](https://user-images.githubusercontent.com/65555981/197025639-eca389b3-862a-4154-9b57-20142e731c09.png)

### password: JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv

# Level 12 &rarr; Level 13

## Level Goal
> The password for the next level is stored in the file **data.txt**, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)

## Solution
Because I can't edit anything in the home directory, I create a temporary folder using `mktemp -d`. Once we have our temporary directory we copy over `data.txt`. I then convert `data.txt` to a normal binary file using `xxd -r` and then uncompress using either `bzip2`, `gzip`, or `tar` until I got the password. Eventually, we end up with the file `data8` and printing it gives us the password.

![image](https://user-images.githubusercontent.com/65555981/197033585-82d25e23-b2df-4086-81af-d38b172b15b5.png)

### password: wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw

# Level 13 &rarr; Level 14

## Level Goal
> The password for the next level is stored in **/etc/bandit_pass/bandit14 and can only be read by user bandit14**. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. **Note: localhost** is a hostname that refers to the machine you are working on

## Solution
You can use the private key to login to `bandit14` instead of using a password. You can specify the private key using the `-i` flag in `ssh`. The full command would look like `ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220`. Once you're in you can grab the password from `/etc/bandit_pass/bandit14`.

![image](https://user-images.githubusercontent.com/65555981/197039529-848670e8-f168-4f82-bb74-ee56e40b542c.png)
![image](https://user-images.githubusercontent.com/65555981/197039570-0f599cb9-02a5-43cb-bc78-47a109c316c0.png)

### password: fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq

# Level 14 &rarr; Level 15

## Level Goal
> The password for the next level can be retrieved by submitting the password of the current level to **port 30000 on localhost**.

## Solution
We can connect to localhost on port 30000 by using a tool called netcat, typed out `nc` in Linux. Using `nc localhost 30000` we are able to submit the current level's password and recieve the next password.

![image](https://user-images.githubusercontent.com/65555981/197250271-e57001d1-5ef4-4bcd-b3a6-21f537059684.png)

### password: jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt

# Level 15 &rarr; Level 16

## Level Goal
> The password for the next level can be retrieved by submitting the password of the current level to **port 30001 on localhost** using SSL encryption.
> 
> **Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…**

## Solution
I used the `openssl s_client` command to connect using SSL. The full command looked like `openssl s_client -connect localhost:30001`. (I didn't have to use the hint as I was still able to input the current password and still get the next password.)

![image](https://user-images.githubusercontent.com/65555981/197252432-b145cd86-c23c-4b5c-8129-c4f2fa7478f7.png)

### password: JQttfApK4SeyHwDlI9SXGR50qclOAil1

# Level 16 &rarr; Level 17

## Level Goal
> The credentials for the next level can be retrieved by submitting the password of the current level to **a port on localhost in the range 31000 to 32000**. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

## Solution
It would take way too long to manually go through each port and check if it's the right server so we would need some sort of tool to scan the entire network quickly and detect whether its using SSL. Thankfully, we don't have to make that ourselves since it's already been made. Nmap is a port scanning tool that will help us solve this challenge effectively. When look at the man page for `nmap`, we see and example command which is the what the typical `nmap` command looks like. Using this, we can figure out what ports are open and if SSL is on them. However, we aren't looking to scan the entire network, only in the range of 31000 ro 32000. In order to specify the port range, we can use the arugment `-p`. The full command would be `nmap -a -T4 -p31000-32000 localhost`. The scan returns this:

![image](https://user-images.githubusercontent.com/65555981/197256086-eb64bc31-302f-410e-919f-4d6322d2a193.png)

We can see there are actually 2 ports using SSL, but one of them is marked as `ssl/echo` meaning when we send data to it, we will just get that data back. The port we should try connect to is probably `31790`. Doing this gives us the private key to connect to the next level.

![image](https://user-images.githubusercontent.com/65555981/197258000-f2701aae-b760-4008-8b01-39dbf92b47cc.png)


### cert:
```
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
```

