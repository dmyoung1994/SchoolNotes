# CS 246 Lecture 19: Dtor revisited, Pure Virtual, Observer pattern, Decorator Pattern, Valgrind, Make
## Destructors
- Always make destructor of base class virtual
- Valgrind on Mac leaks memory
    - X11 also leaks memory
- Subclass destructor always automatically calls the base class desctuctor
## Pure Virtual: Computing Student Fees
```
class Student {
    protected:
        int numCourses;
    public:
        virtual int fees();
        // virtual int fees() = 0; pure virtual
};

class Regular : public Student {
    public:
        int fees() {
            return numCourses * 350;
        }
};

class Coop : public Student {
    public:
        int fees() {
            return (numCourses * 350) + 600;
        }
};

int main() {
    Student *students[100];
    ...
    ...
    for(int i=0; i<100; ++i) {
        cout << students[i]->fees() << endl;
    }
}
```
- Issue: never implemented fees for the Student base class
    - We do not want to do this, so we tell the compiler to ignore it with `=0` at the end
- Any class with at least one pure virtual method is called abstract
    - Cannot create objects of an abstract class
        - ie cant do `Student s;`
- A derived class ingerits all members (inclduing pure virtual methods)
    - This makes derived classes abstract by default unless all ingerited pure virtual methods are implimented
- A class is a **concrete** class if it has no private virtual methods
- Abstract class names are in italics for UML diagrams (or cursive)
    - Same for virtual and pure virtual methods
- Static members and methods are underlined in UML

## Observer Pattern
- Called the Publish/Subscribe model
- Publisher/Subject generates data
- Subscribers/Obserbers: want to be notified of new data/react to data
- Rule of thumb:
    - If you cant find a method to make pure virtual, choose the dtor but we still have to implemet it
        - subclass dtor will call superclass detor
- A method that is pure virtual must be implemented in the derived classes otherwise the derived class is abstract
- Somthing changes in ConcreteSubject -> calls notifyObservers() -> Observer will call getState to determine what has changed

## Decorator Pattern
- Example would be a browser window
    - add a scrollbar
    - add a menu
- Do this without destroying the object and creating a a new one (decorating it)

