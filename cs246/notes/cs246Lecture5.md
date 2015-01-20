# CS246 Lecture 5 January 20th 2015
- Arguements within a script: `$1 $2, ]...`
- Each prog/command returns a status code ($?)
    - $? == 0 if successful
    - non-zero otherwise
- $# gives the number of arguements to script
- $0 contains the name of the file
    - ex:
    ```
    #!/bin/bash

    usage () {
        echo "Usage: $0 [password]"
    }

    if [ $# -ne 1]; then
        usage
    fi
    egrep ...
    ```
- Use elif to add other if conditions
    - ex:
    ```
    if [ conditions ]; then
        ...
    elif [ conditions ]; then
        ...
    else 
        ...
    fi
    ```

- All of the different forms of comparisons are availible in the linux commands handout

## Loops
- While loops
    - eg Print the numbers from 1 to $1 with a while loop
    ```
    # While loop
    #!/bin/bash
    x=1
    while [ $x -le $1 ]; do
        echo $x
        x=$((x+1))
    ```
    - Moral of the story, encase arithmetic expressions with $(()). Otherwise, does string contatenation

- For loops
    - eg rename all cpp files to cc
    - to rename files, use `mv [source] [destination]`
    ```
    #!/bin/bash
    # the for loop splits the globbin pattern on whitespace and assigns it to $name
    for name in *.cpp; do
        mv ${name} ${name%cpp}cc
    done
    ```
    - eg how many times does $1 occur in $2 
    ```
    #!/bin/bash
    count=0
    for word in `cat $2`; do
        if [ "$1" -eq word ]; then 
            count=$((count+1))
        fi
    done
    echo $count
    ```
    - eg Payday is the last fraday of a month. What date is this this months payday?
    ```
    cal | awk '{print $6 }' | egrep "[0-9]" | tail -1
    ```

## Assignment info
- A2 - A4:
    - Due date 1 will have no programming
        - Writing test suites
        - Black box tests
            - Positive/Negative
            - Boundary cases
            - Corner cases
        - Test case specs are given in a1q5
        - Marmoset public:
            - checks if the outputs are exactly the same
        - Marmoset release:
            - how good is the coverage?
            - check edge cases
            - release tokens regenerate every 24h
        - Marmoset secret:
            - random tests that you only see if they passed after the due date
    - Due date 2 has the programming
        - testing does *not* prove correctness
        - testing proves incorrect
        - White Box Testing
            - write tests the execute all functions
            - cover all code paths
        - Regression Testing
