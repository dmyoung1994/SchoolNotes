# Lecture 16: Static, Singleton Design Pattern, Encapsulation
## Static
- **Static Fields**: if a field is declared as static, it is associated with the class, not with the object
    - Must define it seperately (to reserve memory)
        - must be external `Student::numInstances=0`
- **Static Member Functions**: dont depend on any instance/object of the class
    - Implication of calling these functions is that we dont have access to `this`
    - Often these are called static methods, but they are not because of the reason above
    - Can only access other static member functions or static fields
```
struct Student {
    int assns,mt,final;
    static int numInstances;
    static void printInstances() {
        cout << numInstances << endl;
    }
}

Student bobby(...);
Student billy = bobby;
...
cout << Student::numInstances << endl; // prints 2
Student::printInstances(); // prints 2
billy.printInstances(); // prints 2
```

## Singleton Design Pattern
- We have a class C, and we need to ensure that only one object of this class is ever created
- Example: Finances
    - Wallet class: only one wallet
    - Expences class: many expences
```
// Wallet.h
struct Wallet {
    int money;
    Wallet();
    void addMoney(int)
    static Wallet* instance;
    static Wallet* getInstance();
    static void cleaUp();
}
 
// Wallet.cc
Wallet:Wallet() : money(0) {}
void Wallet::addMoney(int amount) {
    money += amount;
}

Wallet* Wallet::instance = 0; // Set to NULL
static Wallet* Wallet::getInstance() {
    if (instance == NULL) {
        instance = new Wallet;
        atexit(cleanup);
    }
    return instance;
}

// Expense classes in repository
``` 
- The `<cstdlib>` has a function called atexit
    - takes a pointer to a function as a parameter
    - the function must have return type `void` and not accept any parameters
    - Multiple registered functions with atexit execute in LIFO order
```
void Wallet::cleanUp() {
    delete instance;
}
```

## Encapsulation
- Problem with singleton above is that nothing stops client code from creating their own wallets using
    - Constructor is visable
- **Encapsulation**: hiding of implementation details
    - clients must treat the object as a black box
    - clients access object through an exposed interface
- Advice: always hide fields
```
struct Vec {
    Vec(int x, int y): x(x), y(y) {}
    private: 
        int x, y;
    public:
        Vec operatory+(const Vec& other);
        int getX() { return x; } // getter for client to get access to fields
};
```
- Structs have default visability of `public`
- Default public visibility is contrary to the advice of keeping as much private as we can
- `class` keyword defaults to public visability

```
class Vec {
    int x, y; // private by default
    public:
        Vec(int x, int y)...
        Vec operator+...
};
```

