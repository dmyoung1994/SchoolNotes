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
    if [ $# -ne 1]; then
        echo "Usage: $0 [password]"
        exit 1
    fi
    ```
