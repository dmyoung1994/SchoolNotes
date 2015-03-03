# CS 246 Lecture 15: Copy + swap, methods vs functons, arrays of objects, const methods, static
## Copy and Swap
```
void Node::swap(Node& tmp) {
    int tData = data;
    data = tmp.data;
    tmp.data = tData;
    Node *tNext = next;
    next = tmp.next;
    tmp.next = tnext;
}

Node& Node::operator=(const Node& other) {
    Node tmp = other; // implemented on the stack and assumes copy ctor has been implemented
    swap(tmp); // when swap returns, "this" now contains the copy of fields from other
               // tmp gets poped when the stack clears, so no need for delete
    return *this;
}
```
- Rule of 3:
    - If you implement a custom version of:
        1. copy ctor
        2. dtor
        3. operator
    - then you usually need to custom implement all 3

## Methods vs Functions
- Before we knew about classes, we wrote:
```
Vec v1(1,2);
Vec v2(3,4);
```
- Previously, we implemented operator+ and operator * as FUNCTIONS
- Now we can implement them as methods
```
struct Vec {
    Vec operator+(const Vec& other) {
        Vec v(x+v1.x, y+v1.y);
        return v;
    }
    // Cant write k * vec, must write vec * v
    Vec operator*(const int k) {
        Vec v(k*x, k*y);
        return v;
    }
}
```

- c++ requires the following operators to be implemented as methods:
    - operator=
    - operator[]
        - subscript notation for objects
    - operator->
        - lets you treat objects as pointers
    - operator()
        - lets you treat objects as functions
    - operatorT()
        - implements an implicit conversion to type T

## Arrays of Objects
```
struct Vec {
    int x,y;
    Vec(int x, int y) : x(x), y(y) {}
};

// These arrays do not compile because we have no default (0 arg) ctor to initialize elements
// of the array (objects)
Vec vectors[3]; // does not compile
Vec *moreVectors = new Vec[10]; // does not compile
```
- Fix this, we have a few options:
    1. Provide a 0 arg ctor
    2. 
        - for static allocated array, we use array initialization `Vec vector[3] = {Vec(1,2), Vec(3,4), Vec(5,6)};`
        - create a heap allocated array of ptrs to objects
            - to do this we can do `Vec** pVectors = new Vectors*[10]`
                -  A heap allocated array of vec pointers (pointer to a poiter)
        - We can also do this on the stack
            - `Vec* vectors[10];`

- To delete these arrays of objects, delete the contets of the array, then delete the array itself

## Constant Methods
```
struct Student {
    int assns, mt, final;
    float grade() {
        return 0.4 * assns + 0.2 * mt + 0.4 * final;
    }
}

const Student bobby(60,70,80); // cannot be changed
```
- You can still call methods on constant objects as long as it doesn't change the fields
- To tell the compiler that the method doesnt change const values:
    - `float grade() const {};`
    - It will then check whether or not you try to change const values
- Objects that are constant can only call constant methods
- ex keeping count of how many times `grade()` has bee called
```
struct Student {
    int assns, mt, final;
    int numCallsGrade;
    // mutable int numCallsToGrade // will compile
    ...
    ...
    float grade() const {
        ++numCallsToGrade;
        return ...
    }
}
```
- This wont compile because you are changing things in a const method
    - Perhaps we should be allowed to change numCallsToGrade even if the object is constant

## Static keyword
- Static Fields
    - if a field is declared to be static, the the field is associated with the class, not any specific object
        - A field is stored in the class, and all objects access the same memory
```
struct Student {
    static int numInstances; // declaration
    ...
}
```
- numInstances is not reserved on a per object basis, so we have to define it in a seperate file (.cc)
    - `int Student::numInstances = 0;`
