*Some screenshots may look different and that's becaue I'm working on this on different devices. Just ignore it lol.*

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
At first, when we go into the `inhere` folder and run `ls`, we don't see any files, though not suprising since we're told the file is hidden. If we look at the man page for `ls`, we see that theres a flag `-a` which doesn't ignore file names starting with a dot. When we run `ls -a` we see there is a file called `.hidden`. Running `cat` on the file gives us our password.

![image](https://user-images.githubusercontent.com/65555981/196988883-5d552491-4f89-477c-9ab9-12979cc9eb40.png)

### password: 2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe

# Level 4 &rarr; Level 5
