# CS 246 Lecture 20: Factory Method Pattern, Template Method Pattern, C++ Templates
## Factory Method Pattern
        Enemy               level
        /  \                /   \
    Turtle  Bullet      Normal  Castle
```
int main() {
    Player *p = ...
    Level *l = ...
    while(notLost) {
        // Generate enemy
        Enemy *e = ??? // key is how to do this?
        // l->createEnemy(); // this will create the proper enemy
        p->strike(e);
        ...
    }
}
```
- Perhaps on easier levels, we want to generate easier tutrles
- Dont want to clutter main gameplay with all the logic
- Want a factory for enemies that will generate enemies based on levels
    - Can start with creating a method in level and overrite it in the specific levels
```
class Level {
    public:
        virtual Enemy* createEnemy() = 0;
};

class Normal : public Level {
    Enemy* createEnemy() { //more turtles }
};

class Castle : public Level {
    Enemy* createEnemy() { //more bullets }
};
```
- `createEnemy()` is the factory method here
- Adding a boss to this game only would require a change to the Castle subclass
    - Good design

## Template Design Pattern
          +----------------------------------------+
          |  AbstractClass                         |
          |     Common Methods - implementation    |
          |     *DiffereingMethods* - pure virtual |
          +----------------------------------------+
                /                       \
               /                         \
        ConcreteClassA                ConcreteClassA
            *differentMethods*            *differentMethods*

```
class Turtle : public Enemy {
    public:
        void draw() {
            drawHead();
            drawShell();
            drawTail();
        }
    private:
        void drawHead() { ... }
        void drawTail() { ... }
        virtual void drawShell = 0 // needs to be different dependent on color
};

class RedTurtle : public Turtle {
    void drawShell() { ... } // draws a red shell
};

// Green turtle is similar
```
- Basic functionality is share between classes
- Specilizations are pure virtual and must be implemented seperately

## Templates - Different than template pattern
```
class Node {
    int data;
    Node* next;
    ...
    ...
```
- Template is a class which is paraterized on one or more types
```
template <typname T> class Node {
    T data;
    Node<T>* next;
    Node(T data, Node<T>* next) : data(data), next(next) {}
    T getData() const { return data; }
    Node<T>* getNext() const { return next; }
}

Node<int>* intList = new Node<int>(2, 
                        new Node<int>(4, NULL));
Node<Node<int> > LlofllInts = new Node(new Node(5, NULL), NULL);
```

## Standard Template Lib (STL)
### Dynamic length arrays
    - Called std::vector in the stl
    - automatic resizing
- ex:
```
#incude <vector>
int main() {
    vector<int> v;
    v.push_back(5); // adds to the end of the array
    v.push_back(10);
    // Not range checked
    for (int i=0; i<v.size()l ++i) {
        cout << v[i] << endl; // operator[] is implemented
        v.at(i); // is range checked
    }
```
- If i is out of range for `v.at(i)` something bad happens
- `v.pop_back();` removes the last element from the vector
- `v.erase(index);` erases an index
### Interators
```
for (vector<int>::iterator i = v.begin(); i != v.end(); ++i) {
    cout << *i << endl;
}
```
