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
