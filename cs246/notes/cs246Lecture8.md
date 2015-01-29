# CS 246 Lecture 8 January 29th 2015: Misc Topics
## Strings
- In C, we use arrays of `char *` which are null terminated `(\0)`
    - Dissadvantages:
        - explicitly manage array size
        - easy to screw up by overwriting `\0`
- In C++ we have std::string
    - Advantages:
        - automatically adjusts size of string
        - no null terminator to worry about

- C style string is NOT the same as a C++ style string
- C++ allows for an implicit conversion from a C string to a C++ string when initilizing a new C++ string
- In C, to compare two strings, you use strcmp()
- `s1 == s2` is not valid in C since it compares 2 memory address
    - In C++ this is ok
- `s1 + s2` concatenates the two strings, and returns a new string
- `s.length()` gets the strings length
- In C, for a string s, we can do `s[0], s[1], ...` to access characters in string
    - C++ lets you do this

## Default arguements
- Ex.
    ```
    void print(string fileName="myfile.txt") {
        ifstream f(fileName.c_str());
        string s;
        while (f >> s) {
            cout << s << endl;
        }
    }
    ```
    - Does not compile: type mismatch when initilizing f
        - ifstream initilization needs a c-style string
    - We can convert a std::string to a c-style string with `s.c_str()`
- Suppose 99% of the time, we call this function as `print("myfile.txt")`
- To give fileName a default value, we change the parameters to have the default value
    - Called by `print();` which will default to myfile.txt
- A default arguement cannot be followed by an argument which has no default value
    - EX
    ```
    int testParams(int n=0, string c="Waterloo");
    testParams(); // OK
    testParams(10); // OK
    testParams(10, "Toronto"); // OK
    testParams(,"Montreal"); // NOT OK
    ```

## Overloading
- In C:
    ```
    int negInt(int a) {
        return -a;
    }
    
    bool negBool(bool b) {
        return !b;
    }
    ```
    - Can't have functions with the same name even if the types are different
- In C++, two or more functions can have the same name as long as they differ in the number of arguements or the types of arguements
    - Just differing on the return type is not enough
- `>>` is overloaded because it is an inout operator, bit shifter, and other things

## Declaration before use
- In C, C++ you must declare something before you use it. 
- It is declaration, not definition before use

## Pointers
```
int n = 5;
int *p = &n; // points to the address of n
cout << p; // prints the address of n
count << *p; // prints the value at the address of n (5)
int **pp = &p; // points to a pointer of a pointer to a variable
**pp = 10; // Sets n to be 10
```
- Arrays are NOT pointers
- The name of an array is short hand for the address of the first element of the array
- `a == &a[0]`

