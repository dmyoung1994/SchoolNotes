# CS 246 Lecture 22: Compilation Dependencies, The Big Three (revisited)
## Visitor pattern
- Leads to classes being defined before their dependencies
    - Cyclic dependency between the includes 
- Should forward declare classes instead of including them because we only need to tell the compiler that the class exists
- When to include vs. when to forward declare? 
    - Fewer the includes, less time the compiler spends on code
    - No need to add compilation dependencies if one doesnt exist
```
// a.h
class A {
    ...
};

// b.h
#include "a.h"
class B : public A {
    ...
};

// To Find the size of B, we need to know the size of A

// c.h
#include "a.h"
class C {
    A myA;
};

// To know the size of C, we need to know the size of A

// d.h
class A;
class D { 
    A* pmyA;
};

// Pointers are fixed size, so we don't need to know the size of A

// e.h 
class A;
class E {
   A foo(A a);
};

// e.cc would need to use include, but here we dont need to know the size of A, we just need to know that the type exists
```

- WONDOWS WOO
```
// window.h
class Xwindow {
    Display *d;
    GC gc;
    Window w;
    ...
    public:
        drawRectangle(...);
        drawString(...);
};

// client.cc
#include "window.h"
Xwindow *w = ...;
w->drawRectangle(...);
```
- Changes to private window.h requires recompilation of client code
    - BAD
- To fix this, we shoudl create a separate implementation class for all private members
- Place a pointer to that class in Xwindow
```
class XWindowImpl {
    Display *d;
    GC gc;
    Window w;
};

// window.h
class XWindowImpl;
class XWindow {
    XWindowImpl* pImpl;
    public:
        ...
}
```
- Client no longer needs to recompile

## Bridge Pattern
- Generalization of the pImpl idion to accomodate different implementatiosn (using ingeritance)

## THe big 3 revisited
```
class Book {
    string title, author;
    int numpages;
    public:
        ...
        ...
};

class TextBook : public Book {
    string topic;
    public:   
        ...
};

TextBook b("Algorithms", "CLRS", 500, "CS");
Texbook c = b;

// the default copy ctor for TextBook executes

Textbook::TextBook(const TextBook &other) : Book(other), topic(other.topic){}

TextBook& TextBook::operator=(const TextBook& other) {
    Book::operator=(other);
    topic = other.topic;
    return *this;
}
```
- Even if dynamic memory is involved, just remember to update "superclass part" first

```
TextBook b1("A", "Nomair", 200, "CS");
TextBook b2("B", "Kirsten", 100, "PHYSICS");

Book *pb1 = &b1;
Book *pb2 = &b2;
*pb1 = *pb2; // both are Books
```
- The compiler looks at the declared type of the ptr => Book::operator= executes
    - Fix this with virtual
- Default assignment operator is not virtual
- To make it virtual, we do
