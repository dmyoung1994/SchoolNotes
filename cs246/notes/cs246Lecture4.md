# CS 246 Lecture 4: Permissions / Scripts
- Recapping egrep
    - Example 1: Print all words in /usr/share/dict/words that start with an e and contain 5 characters
        - egrep "^e....$" /usr/share/dict/words
    - Example 2: Print all words of even length from /usr/share/dict/words
        - egrep "^(..)+$" /usr/share/dict/words
        - "^([a-zA-Z][a-zA-Z])\*$"
    - Example 3: Print all file names in the current directory where the name contains exactly 1 a
        - ls | egrep "^[^a]\*a[^a]\*$"

## File Permissions
- Access file permission with the -l option on ls
- Permissions are made up of 9 bits

    | file permissions | pointers | user | group |
    | --- | --- | ---| --- |
    | -rwxr-xr-- | 1 | dmyoung | staff |

    - a file can only belong to one group
    - a user can belong to multiple groups (cmd groups)
    - permissions are 3 groups of 3 bits   
        - each group must have the form "rwx"
            - "r" = read
            - "w" = write
            - "x" = execute
            - "-" = not set
        - first 3 bits are user bits (u)
            - the permission of the user that created the file
        - next 3 bits are group bits (g)
            - the permission that the people of the group have
        - last 3 bits are other bits (o)
            - the permission everyone else has

### Permission Table
| | Ordinary files | Directory |
| --- | --- | --- |
|r|see the credentials, cat, text editor|see contents, ls, gloobing, tab completion|
|w|modify the contents|add/rename files|
|x|try to run the file as a program|navigate into the directory (cd)|

- The owner of a file is the only one with the permission to change file permissions
    - You can't give someone else the permission to change the original file permissions (ownership of a file can't be transfered)
- If you are the owner of a file, you can use the `chmod [mode] [filename]` command
    - Mode:
        - Ownership Class
            - "u" for user
            - "g" for group
            - "o" for other
            - "a" for all
        - Operator
            - "+" add
            - "-" revoke
            - "=" set exactly
        - Permissions
            - "r" for read
            - "w" for write
            - "x" for execute
        - Can also user numbers
            - binary representation of the numbers is paired with the bits of the permissions
        - Ex:
            1. Give other read permission
                - chmod o+r
            2. Revoke execute from group
                - chmod g-x
            3. Give everyone just read and write access
                - chmod a=rw

## Shell Variables
- We can do things like "x=1"
    - no spaces between decliration of variable and assigning it
- Values of variables are ALWAYS strings
- To retrieve a value, use the $
    - Ex. "echo $x" will print x
- Variables on the left side of "=" dont use $, but add "$" on the right side
- $PATH variable containing paths to directories
    - The shell looks at these paths to find a program

## Scripts
- Scripts are files that contain a sequence of linux commands, executed as a program
- Example script named "basic"
```
// basic

\#!/bin/bash  // shebang line, tells shell this is a bash script
date
whoami
pwd
```
- To execute this script this file, type
    1. give full path `./basic` (prefered)
    2. modify $PATH
- When you try running this program, there are no execute permission on this text file by default
    - to change the permissions for yourself, use `chmod u+x basic`

### Command Line arguments to a script
- Arguments can be accessed using the variable names $1, $2, ...
    - $# gives you the number of arguments
- Usage: ./isItAWord hello
```
# isItAWord
#!/bin/bash
egrep "$1$" /usr/share/dict/words
```

- Every process returns a status code:
    - 0 for succes
    - non-0 for failure
    - $? is how to retrieve the status code
```
# goodPassword
#!/bin/bash
# egrep returns 0 if word is found atleast once, otherwise non-zero
egrep "$1$" /usr/shoare/dict/words > /dev/null # /dev/null is a black hole to dump data
if [ $? -eq 0 ]; then
    echo Not a good password
else 
    echo Perhaps a good password
fi
```
