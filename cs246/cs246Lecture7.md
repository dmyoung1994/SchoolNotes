# CS 246 Lecture 7 January 27th 2014: All About Streams
## cin / cout
- What happens when a read fails while utilizing cascaded reads?
    - The failed read fails, as well as all subsequent reads
        - This happens unless something is done about it

- We are going to refactor the readInts to ignore non-ints and terminate if eof is encountered
    ```
    // Typical includes and namespace excluded
    int main() {
        int i;
        while (true) {
            if (cin >> i) {
                cout << i << endl;
            } else {
                if (cin.eof()) {
                    // Read encountered an EOF
                    break;
                } else {
                    // Read failed due to a bad read
                    cin.clear(); // Acknowledges that we have seen the failed read and tells cin to clear the flag
                    cin.ignore(); // discardes the failed read, and moves forward for more input
                }
            }
        }
        return 0;
    }
    ```

## Reading Strings
- C++ has a string type in the standard package
- To use it, `#include<string>` and `using namespace std;`
- Ex:
    ```
    #include<string>
    #include<iostream>
    using namespace std;
    int main() {
        string s;
        cin >> s;
        cout << s;
    }
    ```
- cin will ignore leading whitespace, starts reading from the first non-whitespace character, and keeps reading until whitespace is encountered again
- If you want to include whitespace, use `getline(cin,s)` where `s` is the string variable you want to save the line to
    - Reads from current position unit a newline
        - Might be useful for a2
- In C, you can specify a format specifier to print and interger in hex

## I/O Manipulators in C++
- Ex:
    ```
    int x = 95;
    cout << x; // prints in decimal
    cout << hex << x; // prints in hex where hex is an I/O manipulator
    ```
- Sending hex to cout mutates cout so that now all subsequent couts will print in hex
- In order to use them, `#include<iomanip>`
- Other manipulators
    - boolalpha
    - showpoint
    - setpercision(N)

## Streams
- cin is a variable of type istream
- cout is a variable of type ostream
- The stream abstraction is applicable to other sources of data
- To read and write to files, `#include<fstream>`
    - ifstream for reading input from files
    - ofstream for outputting to files
- Ex
    ```
    #include<fstream>
    #include<iostream>
    #include<string>
    using namespace std;
    int main() {
        string s;
        ifstream f("myfile.txt");
        while (f >> s) {
            cout << s << endl;
        }
    ```
- The file is closed automatically when f goes out of the scope, because it was stack allocated
- Anything you can do with cin (istream) you can do with f(ifstream)
- The same stream abstraction can be used to read/write to strings
    - Header: `#include<sstream>`
    - 2 types:
        - istringstream
        - ostringstream
    - Ex
    ```
    // buildString.cc
    int low = ...;
    int high = ...;
    ostringstream ss;
    ss << "Enter a number between ";
    ss << lo << " and " << hi << endl;
    string s = ss.str(); // stores the created string in s;
    cout << s << endl; // Prints the string;
    ```
- istringstream can be used, apart from many other things, to conver a string into an int
- Reading into a string only ever fails at EOF
