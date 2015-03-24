# CS 246 Lecture 21: STL, Visitor Design Pattern
## Vector Iterators
```
// iteratre over a vector
for (vector<int>::iterator i=v.begin(); i != v.end(); ++i) {
    cout << *i << endl;
}

// reverse iterator
for (vector<int>::reverse_interator i = v.rbegin(); i != v.rend(); ++i) {
    cout << *i << endl;
}
```
- rend points to one position past the begining of the vector
- `v.pop_back();` allows us to retrieve the last entered entry in v
- `v.erase(v.begin());` erases the first element of v, and updates iterators to point to the right place

## Maps
- Stack allocated
- Key value pair with different indecies
- Templated class parameterized on two types, key and value
    - ex `map<string, int>`
```
#include <map>
map<string, int> m;

// Setters
m["abc"] = 5;
m["def"] = 10;

// Getters
cout << m["abc"] << endl;
if (m.count("abc")) cout << "found" << endl;
```
- Map iterators also exist
- Maps are sorted by keys
```
for (map<string, int>::iterator i = m.begin(); i != m.end(); ++i) {
    cout << i->first << i->second << end; // first gives key and second gives value
```

## Visitor Design Pattern
- **Dynamic Dispatch**: The method that is called depends on the run time type of the object which it was called
- Limited because can only determine the runtime type of one object
- Example: Ememies and weapons
    - Option 1:
        - could impliment `virtual void Enemy::strike(weapon&);`
    - Option 2:
        - could impliment `virtual void Weapon::string(enemy&)
        - same problem though
    - Benefit 1:
        - Double Dispatch
```
class Enemy {
    public:
        virtual void strikeWith(weapon& w) = 0;
        ...
        ...
};

class Turtle : public Enemy {
    public:
        void strikeWith(weapon& w) {
            w.useOn(*this);
        }
};

class Bullet : public Enemy {
    public:
        void strikeWith(weapon& w) {
            w.useOn(*this); // type is known at compile time
        }
};

class Weapon {
    public:
        virtual void useOn(Turtle& turtle) = 0;
        virtual void useOn(Bullet& bullet) = 0;
        ...
};

class Stick : public Weapon {
    public:
        void useOn(Turtle& turtle) {
            // strike turtle with stick
        }

        void useOn(Bullet& bullet) {
            // strike bullet with stick
        }
};

class Rock : public Weapon {
    public: 
        void useOn(Tutrle& turtle) {
            // strike turtle with rock
        }
        // same shit for rock
};

int main() {
    Enemy* e = new Bullet;
    Weapon* w = new Rock;
    e->strikeWith(*w);
```
- Accepting visitors
```
class Book {
    public:
        virtual void accept(BookVisitor& v) {
            v.visit(*this); // this is a book
        }
};

class TextBook : public Book {
    public:
        void accept(BookVisitor& v) { v.visit(*this); } // this is a TextBook
    }
};

// same shit with comic book

class BookVisitor {
    virtual void visit(Book& b) = 0;
    virtual void visit(TextBook& tb) = 0;
    virtual void visit(ComicBook& cb) = 0;
}

class MyCatalogue : public BookVisitor {
    map<string, int> catalogue;
    public:
        void visit(Book& b) {
            catalogue[b.getAuthor()]++;
            //catalogue[b.getAuthor()] = catalogue[b.getAuthor] + 1;
        }

        void visit(TextBook& tb) {
            catalogue[tb.getTopic()]++;
        }

        void visit(ComicBook& cb) {
            catalogue[tb.getProtagonist()]++;
        }
}
```
