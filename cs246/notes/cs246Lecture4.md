# CS 246 Lecture 4: Permissions / Scripts
- Recapping egrep
    - Example 1: Print all words in /usr/share/dict/words that start with an e and contain 5 characters
        - egrep "^e....$" /usr/share/dict/words
    - Example 2: Print all words of even length from /usr/share/dict/words
        - egrep "^(..)+$" /usr/share/dict/words
        - "^([a-zA-Z][a-zA-Z])*$"
    - Example 3: Print all file names in the current directory where the name contains exactly 1 a
        - ls | egrep "^[^a]*a[^a]*$"
# File Permissions
- Access file permission with the -l option on ls
- Permission are made up of 9 bits "_________"

