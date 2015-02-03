# CS 246 Lecture 9: References/Dynamic Memory Allocation February 3rd 2015
## Structs
- In C, you always have to user `struct name` when using it
- In C++, you dont need to have `struct`, compiler sees it as a real type
- Need pointers to structs when defining them becuase we know the size of a pointer, but not of a defined structure

## Constants
- A constant definition must be initilized
- Examples:
    ```
    const int maxGrade = 100; // maxGrade cant be changed in this code
    ```
- Constants allow the compiler to optimize your code
- Constants fields cannot be changed
- Constant pointers can be changed, but you cant use it to change what the pointer is pointing to
    - Ex. 
    ```
    int n = 5;
    const int *p = &n;
    p = &m; // this is ok
    *p = 10; // this is not ok
    n = 10; // this is ok
    ```
- `const` applies to the thing on the left, unless there is nothing on the left, then it applies to the thing on the right

## I/O
- In C, we use `scanf("%d", &x);
- In C++, we can do `cin >> x` which calls `>>(cin, x)`
    - How is this possible?
        - Because we pass by refernece

## References
```
int y = 10;
int &z = y;
```
- z is a reference to y (because the & is in the decleration)
- A **reference** is like a constant pointer with automatic dereferencing
- To change the value that z refers to, we do `z = 20`
    - we dont have to derefeence z to change the value because of automatic dereferencing
    - aka z is a nother name for y (alias)
- References have no identites of its own, it mimicks what ever it is referencing
- Things you can/cannot do with references:
    1. You cant leave references unitilized
    2. Must initilize a reference with an lvalue (memory address)
    3. Cannot create a pointer to a reference
    4. Cannot create a reference to a reference
    5. Cannot create a reference to an array
    6. Can create a reference to a pointer
- Incrememnt function with references:
    ```
    void inc(int &n) {
        n += 1; // n is a reference so we dont have to dereference
                // n become a reference to x
    }

    int x = 5;
    inc(x); // Increments x by 1 since we are passing by reference
    cout << x << endl; // prints 6
    ```
- Cascadable function return reference types

## Pass by value vs Pass by pointer vs Pass by reference
```
voing f(int x){...}
int n = ...;
f(n);
```
- Pass by value
- Changes to x are not visible outside of f

```
struct ReallyBig {...};
void f(ReallyBig n){...}
ReallyBig lib = ...;
f(lib);
```
- Pass by value
- If ReallyBig is big enough, this is expensive
- Changes to n are not changes to lib
- In C, we can overcome these problems with pointers
- In C++, we can overcome these problems with references

```
// above code with references
void g(ReallyBig &n) {...} //n is just another name for lib
void h(const ReallyBig &n) {...} // suppressed copy and promises not to change anything
ReallyBig lib = ...;
g(lib);
```
- Advice: Prefer pass by reference to const for anything bigger than an int

## Dynamically allocating memory
- In C, `int \*p = malloc(size computation);`
    - Once you are done with p, you have to free it with `free(p);`
- In C++, to allocate memory we use the keyword `new`
    - To deallocate, use the keyword `delete`
- Biggest advantage to C++ way is that it is type aware
    - Makes code less error prone
```
struct Node {
    int data;
    Node *next;
};

Node *np = new Node;
...
delete np;
```
- To allocate arrays:
```
int size;
cin >> size;
int *arr = new int[size];
...
delete [] arr;
```
