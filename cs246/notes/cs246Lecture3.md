# CS 246 Lecture 3 January 13th 2015
## Redirection, Pipes, and Grep
### I/O
- There are 3 streams when ever any program, process or command is run:
    1. StdIn (standard input)
        - When the program needs input, it looks to the standard input (stdin)
        - Default is keyboard
        - With input redirection, we change the default to a file (<)
    2. StdOut (standart output)
        - When a program needs to output, it outputs to the standard output (stdout)
        - Defiant is screen
        - With output redirection, we change the default to a file (>)
        - Is a buffer, so you don't get output instantaneously
        - Use >> to append instead of overwritting
    3. StdErr (standard error)
        - Used for sending error messages
        - Default is the screen
        - To redirect stderr, use 2>
        - Advantages of stderr:
            - Does not clutter program output
            - Never buffered, so what ever is being sent is sent immediately
- To use the output of a command as an argument for another command, use backquotes (\`date\`)
    - Double quotes will expand backquotes, single quotes will not
    - New syntax is $([command])
        - ex. $(date)
    - Commands can be nested

### Pipes
- Motivating Example:
    - How many words occur in the first 20 lines of sample.txt?
        - "head -n [filename]" will give you first n lines
        - "head -20 sample.txt > temp.txt"
        - "wc -w temp.txt" will count the number of words in temp.txt
        - Is there a better way?
            - YES "head -20 sample.txt | wc -w"
- A pipe (|) is still input redirection, just without the middle man of file writting.
- Sets the default stdin of the second program to be the stdout of the first program.
- A pipeline is a linux command that uses >= 0 pipes
    - ex. SUppose files words1.txt and words2.txt contain a lost of words, one word per line. Print a duplicate free list of all the words in all files.
    - cat words\*.txt | sort | uniq

### Grep (pattern matching within files)
- Not going to use grep, using egrep instead (same as grep -E)
- Output: prits every line in the given file that contains a string matching the given pattern
    - eg. "egrep [pattern] [filename]"
- egrep *IS CASE SENSITIVE* unless you tell it not to be case sensitive with "-i"
- To use or (|), put it quotes so that bash doesnt interperate it as a pipe
    - ex. egrep "cs246|CS246" sample.txt
        - another way of writing this is egrep "(cs|CS)246 sample.txt

#### Regular Expressions
- egrep cs246 index.html (an exact match for cs246)
- egrep "cs246|CS246" index.html (quotes protect pipe from being interperated by the shell
- egrep "(cs|CS)246" index.html is a neater way of doing eh above
- egrep "(c|C)(s|S)246" index.html is a way of getting all possible combinations
- a|b|c|d has a shortcut with square brackets [abcd]
    - [] means pick one of the elements from within this set
    - [^abcd] means pick any character except for the elements of this set
- ? means 0 or one of the preceeding expression
    - ex "(cs|CS) ?246" : space is optional
    - ex "((cs|CS) )?246" : entire cs|CS with a space is optional
- * 0 or more of the preceeding expression
    - ex "(cs)\*246" : matches just 246 and an infinite numbre of cs infront
- + 1 or more of the preceeding expression
    - ex "(cs)\+246" : matches only >1 times of cs
- . matches any one character
    - ex .\* matches any sequence of characters (\* in globbing patterns)
- ^ means line should shart with the following character
- $ means end with preceeding character
    - "^cs.\*246$"
