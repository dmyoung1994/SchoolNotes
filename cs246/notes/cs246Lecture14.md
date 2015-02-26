# CS 246 Lecture 14: The Big ThreeL copy ctor, stor, assignment operator (operator =)
- Typically, if a class has a field that stores dynamically allocated memory, most likely do not want a default shallow copy.
    - This means that we should write our own custom copy ctor that does a deep copy
- Deep copy ctor example
```
// classes.constructors/LinkedListDeep.cc
// Implements deep copy by creating a new instance of what the other points to
struct Node {
    ...
    ...
    // The deep Node ctor recursively sets next
    Node (const Node &other) : data(other.data), next(other.next ? new Node(*other.next) : NULL) 
    {}
```
- Places where a copy ctor is called:
    1. Constructing a new object as a copy of an existing object
    2. Passing an object by value
    3. Returning an object by value
- Copy ctor parameter must be a reference, otherwise causes infinite recursion because the ctor has to copy the parameters, over and over and over again.

## Destructor
- Stack allocated:
    - Object is destroyed when it goes out of scope
- Heap allocated:
    - Object is destroyed when we call delete on a pointer to that object
- The destriction of an object is caused by the special method called the destructor (dtor)
- A class can only have one destructor
- For a class C, the destructor is always `~C();`
    - C is the name of the class
    - 0 parameters
    - ~ prefic
    - no return type
- You get a dtor for free
     - calls dtor on any fields that are objects
```
Node *p = new Node(1, ...);
delete p; // destructor for Node Runs (does nothing because we have no objects)
          // space used by *p is reclaimed
// Custom destructor
struct Node {
    ...
    ...
    ~Node() {
        delete next;
    }
}
```
- the `::` in .cc files is used to implement methods for classes definined in .h files
    - its the scope resolution operator

## Assignment Operator (operator=)
- Assign an existing object to be a copy of another existing object
- Ex
```
Student billy(60,70,80);
Student = bobby;
bobby = billy // same as billy.operator=(bobby);

// We use a refernce to use cascading to work
// This is wrong, must guard against self assignment
Node &Node::operator=(cosnt Node &other) {
    delete next; // If you dont do this, you will leak next
    data = other.data
    next = other.next ? new Node(*other.next) : NULL;
    return *this;
}

// This guards against self assignment
Node &Node::operator=(const Node &other) {
    if (this == &other) return *this;
    data = other.data
    delete next;
    next = other.next ? new Node(*other.next) : NULL;
    return *this;
}
```
