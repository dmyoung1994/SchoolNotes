# CS 246 Lecture 6 January 22nd 2015

## C++
- Invented by Bjarne Stroustrup in the 80s
    - Created C with classes
    - C++ (slightly better than c, now is much better)
    - We will be using C++ 03

- First C++ Program
    ```
    // hello.cc
    #include <iostream>
    using namespace std;  // Changes the scope of the program
    int main(){
        count << "Hello World" << endl;
        return 0;
    }
    ```

- C++ main function must be of return type of int
- A function that has a return type t, must always return a value of type t
    - Except for the main function need not return a value 
- In C, we include stdio.h and then we use printf
    - You can do this in C++ as well, but don't use it for this class
- For C++ I/O, we include iostream
    - To output something, use `std::cout << ...data... << std::endl;`
        - To not have to use `std`, use the `std` namespace as in the hello.cc code above

### Compiling Porgrams
- Write in the command prompt `g++ <filename> [commands] <newname>`
    - ex `g++ hello.cc -o prog` will compile hello.cc and produce and executable named `a.out`
- To execute a program after compiled, type `./prog` or `./a.out`

### I/O
- C++ provides us with 3 stream objects
    - Standard input -> cin (reads from stdin)
    - Standart output -> cout (writes to stdout)
    - Standart error -> cerr (writes to srderr)
- Two I/O operators
    - << = "put to operator" (output)
    - >> = "get from" operator (input)
- I/O Example
    - Gets two numbers from stdin, adds them, and writes it to stdout
    ```
    // ioex.cc
    #include <iostream>
    using namespace std;
    int main()
        int x, y;
        cin >> x >> y; // Reads the first integer into x, and the second into y
        count << x + y << endl;
    }
    ```
- A read can fail if:
    1. encounter EOF
    2. types do not match
    - Scary thing about this is that the program continues as if nothing bad happened
- When a read fails, a flag is raised
    - To check this flag, use `cin.fail()`
        - Is true if read fails (EOF or type mismatch)
    - If the read fails because of EOF, `cin.eof()` will also be true
- Read all ints from stdin and echo to stdout, one per line. Terminate if a non-int or EOF is encountered
    ```
     // 2-io/readInts.cc
    #include <iostream>
    using namespace std;
    int main() {
        int x;
        while(true) {
             cin >> x;
             if (cin.fain()) {
                 break;
             }
             cout << x << endl;
        }
    }
    ```
- In C and C++ if you add an int to a double, the int gets implicitly converted to a double
- C++ defines an implicit converstion from cin to void\*.
- Pointers, including void\*, are numeric entities
    - Usefull because numeric entities can be used as conditions
        - `if (cin)` is true if `!cin.fail()` is true
- We can now imporve our readIntes.cc code
    ```
     // 2-io/readInts4.cc
    #include <iostream>
    using namespace std;
    int main() {
        int x;
        while(cin >> x) {
             cout << x << endl;
        }
    }
    ```
