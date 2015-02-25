# CS 246 Lecture 11: Preprocessor, Conditional Compilation, Separate Compilations
## Conditional Compilation
- In Unix, native programs written in C++ need to have `int main() { ... }` functions
- In Wondows, native programs need to have a `int WinMain() { ... }` functions
    - And you **CANT** have a main function
- To have it all in one file, we can have preprocessor conditionals:
```
#define Unix 1
#define Windows 2
## define OS Unix // Change this to be the system you are depolying on
#if OS == Unix
    int main () { // Appears in compiled code
#elif OS == Windows
    int WinMain() { // Doesnt appear in compiled code
#endif
        // Shared code
        ...
    }
```
- Preprocessor `#if` if satisfied forwards the body to the compiler
- Can be done without modifying codebase, ie setting flags
    - We can supply the compililer arguements to the preprocessor
        - This means we wont need to change the code at all
        - `C++/preprocess/define.cc`
- User the `-D[variable] command to define preprocessor variables
    - Ex. `g++ -DX=15 define.cc` sets the `X` variable in define.cc to be `15`
    - In our code example, we could do `g++ -DOS=Unix file` to set up a unix environemnt
- The value in `#define VAR VALUE` is optional
    - it defualts to the empty string
- `#ifdef VAR` is true is VAR is defined
- `#ifndef VAR` is true if VAR is not defined
- We can wrap code (aka comments) in preprocessor conditionals to only print when something si defined
    - In `debug.cc` if we comile with `-DDEBUG debug.cc` will turn printing on
- Compile time variables default to 1

## Separate Compilation
- Interface (.h, headers)
    - type definitions
    - forward declarations of function, function prototypes, function headers
- Implementation (.cc)
    - actual implimentation of functions
- Rules for compilation:
    1. Never compile a header file
    2. Compile multiple files with globbing patterns `g++ \*.cc`
- Compiling 
- **Seperate Compilation** is the ability to compile separte files of a code base seperatly and link them later to produce an executable
    - Decreases re-compilation time
- Compiling steps: preprocess -> compile -> link -> produce executable
- `g++ -c file` allows you to compile without linking or prodcing an executable
    - produces *.o files
- Object Files (.o files)
    - Binary for the coresponding source code
    - list of functions provided/needed
    - To link
        - `g++ obj1 obj2`
    - Linker ensures that all requesets are fulfilled

## Global Variables
- Makes sense to create a header file for globals
- Problem with this is that every time you redefine a global this way, they aren't redefined globally
    - Linker wont link two definitions of a global
- Want to define globals in .cc files, but we NEVER include a .cc file
    - So in the header file, we add `extern` to the type of the global, and now it is just a declaration, not a definiton
