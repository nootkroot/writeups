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
