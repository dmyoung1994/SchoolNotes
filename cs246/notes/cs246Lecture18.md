# CS 246 Lecture 18
- All members are ingerited from the base class
- What about private members?
    - no, private really does mean private
    - even though the derived object reserves space for the fields that were inherited
```
class Book {
    string title, author;
    int numPages;
    public:
        Book(string title, string author, int numPages) : title(title), author(author), numPages(numPages) {}
}

class Textbook : public Book {
    string topic;
}

Textbook::Textbook(string title, string author, int numPages, string topic) : title(title), author(author), numPages(numPages), topic(topic) {} 
```
- Four steps to object creation:
    1. Space is allocated
    2. superclass part is created
    3. filed initilization/MIL
    4. ctor body

- 2 reasons the Texbook ctor fails:
    1. inherited fields are private, cant set them from subclass
    2. Step 2 wants to call the 0-param ctor for book
        - we dont have one
        - fix would be to provide one
            - stupid so we should prevent the call to the 0-param ctor by calling a non 0-param ctor

- Here is the fix
```
Textbook::Textbook(string title, string author, int numPages, string topoc) : Book(title, author, numpages), topic(topic) {}
```
- To hijack the new step 2, we place the superclass stor in the first position of the MIL

## More viability
- Private really means private!!
- protected is accessable from all subclasses, but nothing else
    - Main cannot access author
```
Textbook::setAuthor(string author) {
    author = auth;
}

int main() {
    //b is a textbook
    b.author = ___; // cant do this, no access
    b.setAuthor(___); // can do this
```

- Advice: keep fields private
    - to give access to derived classes, write protected accessor methods
- UML: pretected is `#`, public is `+`, private is `-`

## Virtual
- Book is heavy is numPages > 200
- Textbook is heavy if numPages > 40
- Comic is heavy if numPages > 30
- We can implement this with method overriding
    - Replaces behaviour of a superclass method
    - example of this is under `inheritance/example2`
```
Book b("___", "___", 50);
b.isHeavy(); // false, Book::isHeavy runs
Comic c ("___", "___", 40, "Batman");
c.isHeavy(); // True, Comic::isHeavy is run
Book b2 = Comic("___", "___", 40, "Batman"); // legal because a comic is a book
b2.isHeavy(); // False, Book::isHeavy is run
```
- Placing a derived object in the space available for a base object can cause the object to be sliced

## Pointers to objects
```
Comic cb(..., 40, ...);
Comic* pcb = &cb;
pcb->isHeavy(); // true runs Comic::isHeavy

Book* pb = cb; // No slicing
pb->isHeavy(); // False runs Book::isHeavy
```
- Compiler looks at the declared type of the variable, doesn't care what the object is going to be at runtime
    - This is default
- We would like to refer to different Books using a base pointer (Book\*)
- We want the most overriden method to run
    - To do this, use the keyword virtual
- Virtual delays the decision of what method to run until runtime based on its runtime type
- The concept of relying on the runtime type of an object is called **dynamic dispatch**
    - uses extra memory ebcause the type has to be checked at runtime
- Every method in Java is a virtual call
```
Book* collection[100]; // each element points to a book, textbook, or comic book
for (int i=0;i<100: ++i) {
    cout << collection[i]->isHeavy(); // runs class specific method if virtual
}
```
- The above is called polymorphism: the ability to accomidate multiple types within an abstraction
