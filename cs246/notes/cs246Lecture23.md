# CS246 Lecture 23: Big Three (revisited), Casting
- Assignment using base pointers causes mixed arguements
    - Asign a Comic to a TextBook
- Recommendation: make the book class abstract!!
```
//Book higherarchy

// AbstractBook // abstract
// |
// +-> NormalBook
// +-> TextBook
// +->Comic

class AbstractBook {
    string title, author;
    int numpages;
    protected:
        abstract book& operator=(const AbstractBook &other) {
            ...
            ...
        }
};

class NormalBook : public AbstractBook {
    public: 
        NormalBook& operator=(const NormalBook& author) {
            AbstractBook::operator=(other);
            return *this;
        }
};

TextBook b1(...);
TextBook b2(...);
AbstractBook *pb1 = &b1;
AbstractBook *pb2 = &b2;
*pb1 = *pb2; // prevents partial assignment because compiler refuses to compile
```

## Casting
- In C,
```
Node n;
int* p = (int *)&n;
```
- Casts should stand out
- No restriction in casts in C
- 4 kinds of casts in C++
1. Static cast:
```
int n = 9;
int n = 2;
cout << m/n << endl; // prints 4
cout << static_cast<double>(m)/n << endl; // prints 4.5
```
    - Can downcast from a superclass type to a sublcass using static\_cast
    - When doing this, be sure what the type of the object is
    - It is an unchecked cast, compiler trusts you
    - Behaviour undefined if the assumption (right types) is not true
2. Reinterperate Cast
```
Vec v;
Student* p = reinterperate_cast<Student*>(&v);
p->getAssns();
```
    - No restrictions on the targer cast
    - Completely dependent on how compiler lays down the objects in memory
        - Completly dependent on compiler
3. Const cast
```
void g(int* p) {...} // written by someone else
void f(const int* q) {
    g(q); // g needs non-const, cant send a const
    g(const_cast<int*>(q));
```
    - The only cast which will cast away "constness" of a variable
4. Dynamic Cast
```
vector<Book *> myBooks;
for (//iteratre over books) {
    Book* bp = *it;
    Comic* cp = dynamic_cast<Comic*>(bp);
    if (cp != NULL) cout << cp->getProtag();
    else cout << "not a comic" << endl;
```
    - Has Restrictions:
        - Types you are trying to cast must actually make sense
        - The class higherarchy must have atleast 1 virtual method
