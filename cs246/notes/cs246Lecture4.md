# CS 246 Lecture 4: Permissions / Scripts
- Recapping egrep
    - Example 1: Print all words in /usr/share/dict/words that start with an e and contain 5 characters
        - egrep "^e....$" /usr/share/dict/words
    - Example 2: Print all words of even length from /usr/share/dict/words
        - egrep "^(..)+$" /usr/share/dict/words
        - "^([a-zA-Z][a-zA-Z])\*$"
    - Example 3: Print all file names in the current directory where the name contains exactly 1 a
        - ls | egrep "^[^a]\*a[^a]\*$"

# File Permissions
- Access file permission with the -l option on ls
- Permission are made up of 9 bits

    | file permissions | pointers | user | group |
    | --- | --- | ---|
    |-rwxr-xr--|1|dmyoung|staff|

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
    -
