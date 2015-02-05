# CS 246 Lecture 10: Operator Overloading + More Review Slides + The Preprocessor
## Operator Overloading
- `<<` and `>>` are overloaded to be I/O and bitshifting
- `+` is overloaded to be integer addition and string concatenation
- Idea is that we can give our own meaning to C++ operators for the types we create
- Ex 1 +,\* overloading
    ```
    struct Vec {
        int x;
        int y;
    };

    // Lets overload the + operator for vectors
    Vec operator+(const Vec &v1, const Vec &v2) {
        Vec v;
        v.x = v1.x + v2.x;
        v.y = v1.y + v2.y;
        return v;
    }

    Vec operator*(const int &k, const Vec &v1) {
        Vec v;
        v.x = k * v1.x;
        v.y = k * v1.y;
        return v;
    }

    Vec x = {3,4};
    Vec y = {1,2};
    Vec z = add(x,y); // Dont do this
    Vec z = x + y;
    Vec w = 3 * y;
    Vec j = y * 3; // Compile time error
    ```
- Ex 2
    ```
    struct Grade {
        int theGrade;
    };

    ostream &operator<<(ostream &out, cosnt Grade &g) {
        return out << g.theGrade << "%";
    }

    ostream &operator>>(istream &in, Grade &g) {
        in >> g.theGrade;
        if (g.theGrade < 0) g.theGrade = 0;
        if (g.theGrade > 100) gmtheGrade = 100;
        return in;
    }

    cout << g1; // want to print a grade
    cout << g1 << g2 << g3; // while still maintaint cascading

    ```

## The Preprocessor
- source code -> preprocessor -> changed source code -> compilation -> executable
- `#include <iosream>` is not C or C++, it is a preprocessor directive
    - It's really a copy and paste of the header file into your code
    - Look for iostream in the C++ standard library (`/usr/include/c++`)
- In C++, we have a new naming convention for C headers `<stdio.h> -> <cstdio>`
    - It's just a c++ implementation of stdio.h
- Preprocessor directive: `#define`
    - ex `#define VAR VALUE`
        - `VALUE` is treaded as a string
    - This creates a preprocessor variable
    - Any time the preprcessor sees a string VAR, it will replace it with the string value
    - It's really just a search and replace
