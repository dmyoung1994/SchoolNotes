# CS 246 Lecture 2 Jan 8 2015
## Linux File System
- Two kinds of files:
1. Ordinary Files: programs, data
2. Directories: folders, can contain other directories/files

- The file system has a tree structure
- The path specifies the sequence of directories from root to a particular file
    - /bin -> the bin directory within root
    - /usr/bin -> the bin specific to the user directory
- Current directory: the directory we are currently in 
- "pwd" gives the current working directory
- Absolute paths always start at the root directory
- Relative paths are relative to the present working directory (directory you are currently in)

### Special Directories
1.  . (dot):
    - current directory
    - get to it by using `cd .` 
2. .. (two dots):
    - Previous directory (parent)
    - Get to it using `cd ..`
    - Can get to grandparent and etc using `cd ../../..`
3. ~ (tilde):
    - Home directory
    - Get to it by using `cd ~`
4. ~userID 
    - userid's home direcotry
    - ex. `cd ~blushman/cs246/final.pdf`

## Commands
1. ls [path]:
    - Lists contents of directory
    - Shows all files, except hidden files (begin with dot)
    - Show hidden files with `ls -a`
    - Leaving command empty lists current directory
    - Can do [Wildcard Matching](#wildcard-matching) with paths as well.
2. man [command]:
    - Gives manual for supplied command

## Wildcard Matching
Ex. List all files that end in `.txt`:
- > ls `*.txt`
- `*` is a globbing pattern
    - matches anything
- The shell looks at the current directory and finds all the files that match this pattern 
    - The names of the matched files are substituted in place of the globbing pattern and then the command runs
- `rm` with wildcards are EXTREAMLY DANGEROUS
- Since the shell matches the pattern, other/all commands can also use globbing
- To prevent wildcard matching, use quotes
    - ex. `"*.txt"` or `'*.txt'`

## Wotrking With Files
1. cat [file]:
    - Displays contents of given file

- `ctrl + c` kills the current process
- `ctrl + d` terminates quietly and gently
    - EOF (end of file) character is send to the process
- You can create text files by writing to them with `cat` and `>`
    - ex. `cat > output.txt` will capture what is sent to cat to the output.txt file
    - very crude way of creating files because you cant edit what is captured
- In general:
    - `[command] [arg] > [fileName]` will capture the output of the command into the file fineName
    - If the file with the filename is overwritten
        - To prevent this, use `>>` instead, as it appends text (Output Redirection)
- We can also direct input into a process with `<` (Input Redirection)
    - ex. `cat < sample.txt` Sends the contents of sample.txt to the cat command
    - no difference in output with `cat`

cat < simple.txt | cat sample.txt
--- | ---
sample.txt is an argument to the cat command | shell opens sample.txt and sends the content to cat

2. wc [options] [file]
    - prints lines, words, and chars in a file

- We can do both inout and output redirection at the same time
    - `cat < input.txt > output.txt` copies input.txt to output.txt
    - use the `cp` command if you actually need to copy files
