## 1️⃣ Chain of Responsibility Pattern

### ✅ Idea

A request is passed through a **chain of handlers**.  
Each handler decides:

- **Can I handle this?** → Yes → process it  
- **Else** → pass to the **next handler** in the chain  

---

### 🎯 Example: Support Levels

A customer support system where:

- **Level 1 Support**  
- **Level 2 Support**  
- **Level 3 Support**

The request moves through each level until someone can handle it.

---

### 🧩 C++ Example

```cpp
#include <iostream>
using namespace std;

class Handler {
protected:
    Handler* next;
public:
    Handler(Handler* n = nullptr) : next(n) {}
    
    virtual void handle(int level) {
        if (next) next->handle(level);
    }
};

class Level1 : public Handler {
public:
    using Handler::Handler;

    void handle(int level) override {
        if (level == 1)
            cout << "Handled by Level 1\n";
        else
            Handler::handle(level);
    }
};

class Level2 : public Handler {
public:
    using Handler::Handler;

    void handle(int level) override {
        if (level == 2)
            cout << "Handled by Level 2\n";
        else
            Handler::handle(level);
    }
};
```
```cpp
Handler* chain = new Level1(new Level2(nullptr));

chain->handle(2);   // Output: Handled by Level 2

```

---

## 2️⃣ Command Pattern

### ✅ Idea

Wrap a **request as an object**, allowing you to:

- Queue it  
- Log it  
- Undo it  
- Execute it later  

This decouples the **sender** (button press) from the **receiver** (Light).

---

### 🎯 Real-life Example

A **remote control button** sends a command:

➡ “Turn TV On”

The remote doesn't know *how* the TV turns on — it just triggers the command object.

---

### 🧩 C++ Example

```cpp
class Command {
public:
    virtual void execute() = 0;
};
```

```cpp
class Light {
public:
    void on() { 
        cout << "Light ON\n"; 
    }
};
```

```cpp
class LightOnCommand : public Command {
    Light* light;
public:
    LightOnCommand(Light* l) : light(l) {}

    void execute() override { 
        light->on(); 
    }
};
```

```cpp
Light l;
Command* cmd = new LightOnCommand(&l);

cmd->execute();   // Output: Light ON

```

---

## 3️⃣ Interpreter Pattern

### ✅ Idea

Define a **simple language grammar** and create an **interpreter** to evaluate expressions in that language.

Commonly used for:
- Mathematical expression evaluation  
- Parsing rules  
- Simple scripting languages  

---

### 🎯 Example

Evaluate a math expression such as:

1 + 2 + 3


---

### 🧩 C++ Example (Very Simple Interpreter)

```cpp
class Expression {
public:
    virtual int interpret() = 0;
};
```
```cpp
class Number : public Expression {
    int value;
public:
    Number(int v) : value(v) {}

    int interpret() override {
        return value;
    }
};

```

```cpp
class Add : public Expression {
    Expression* left;
    Expression* right;
public:
    Add(Expression* l, Expression* r) : left(l), right(r) {}

    int interpret() override {
        return left->interpret() + right->interpret();
    }
};

```
### ✔ Usage
```cpp
Expression* expr =
    new Add(
        new Number(1),
        new Add(new Number(2), new Number(3))
    );

cout << expr->interpret();   // Output: 6

```
---

# 4️⃣ Mediator Pattern

## ✅ Idea  
A **mediator object** manages and coordinates communication between multiple objects, preventing them from referring to each other directly.  
This reduces coupling and centralizes interaction logic.

---

## 🎯 Example  
A **Chatroom mediator** that handles message passing between users.

---

## 🧩 C++ Example

```cpp
#include <iostream>
#include <string>
using namespace std;

class Mediator;

class User {
    string name;
    Mediator* mediator;
public:
    User(string n, Mediator* m) : name(n), mediator(m) {}
    string getName() const { return name; }
    void send(string msg);
    void receive(string msg) { 
        cout << name << " received: " << msg << endl; 
    }
};

class Mediator {
public:
    virtual void sendMessage(string msg, User* sender) = 0;
};

class ChatRoom : public Mediator {
public:
    void sendMessage(string msg, User* sender) override {
        cout << sender->getName() << " says: " << msg << endl;
    }
};

void User::send(string msg) {
    mediator->sendMessage(msg, this);
}
```
### Usage 

```cpp
ChatRoom room;
User u1("Alice", &room);

u1.send("Hello!");

```
---

# 5️⃣ Memento Pattern

## ✅ Idea  
The **Memento Pattern** allows saving and restoring an object's state — commonly used for **undo/redo** functionality.

---

## 🎯 Example  
A **text editor** that can undo changes by restoring a previous saved state.

---

## 🧩 C++ Example

```cpp
#include <iostream>
#include <string>
using namespace std;

class Memento {
    string state;
public:
    Memento(string s) : state(s) {}
    string getState() { return state; }
};

class Editor {
    string text;
public:
    void type(string t) { text += t; }
    Memento save() { return Memento(text); }
    void restore(Memento m) { text = m.getState(); }
    void show() { cout << text << endl; }
};
```
### ▶️ Usage

```cpp
Editor e;
e.type("Hello ");
auto m = e.save();

e.type("World!");
e.show();       // Hello World!

e.restore(m);
e.show();       // Hello 
```
---

# 6️⃣ Observer Pattern

## ✅ Idea  
A **Subject** maintains a list of **Observers** and notifies them automatically whenever its state changes.  
Useful for building **notification systems**, UI listeners, event systems, etc.

### TO-DO EXAMPLE

---

# 7️⃣ State Pattern

## ✅ Idea  
An object changes its **behavior** when its internal **state** changes.  
Instead of large `if/else` or `switch`, each state is a separate class.

---

## 🎯 Example  
A **Traffic Light** switching between:  
Red ➝ Yellow ➝ Green.

---

## 🧩 C++ Example

```cpp
#include <iostream>
using namespace std;

class State {
public:
    virtual void handle() = 0;
};

class RedState : public State {
public:
    void handle() override { cout << "STOP\n"; }
};

class GreenState : public State {
public:
    void handle() override { cout << "GO\n"; }
};

class TrafficLight {
    State* state;
public:
    TrafficLight(State* s) : state(s) {}
    void setState(State* s) { state = s; }
    void request() { state->handle(); }
};
```
### ▶️ Usage

```cpp
TrafficLight t(new RedState());
t.request();                // STOP

t.setState(new GreenState());
t.request();                // GO

```
---

# 8️⃣ Strategy Pattern

## ✅ Idea  
Encapsulate **different algorithms** (strategies) and choose one at **runtime**.  

Same as previously done example with  **Payment Strategy** (UPI / Card / Cash) ✅  
The main benefit is that you can **swap behavior without changing the client code**.

---

# 9️⃣ Template Method Pattern

## ✅ Idea  
Define the **skeleton of an algorithm** in a base class and let subclasses **override specific steps**.  

---

## 🎯 Example  

Making **Tea vs Coffee**:

- Boil water ✅ (same)
- Brew ☕ (different)
- Add condiments 🍋🥛 (different)

The overall process is fixed, but some steps are customizable.

---

## 🧩 C++ Example

```cpp
#include <iostream>
using namespace std;

class Beverage {
public:
    void prepare() {      // template method
        boilWater();
        brew();
        pourInCup();
        addCondiments();
    }

    void boilWater() { cout << "Boiling water\n"; }
    void pourInCup() { cout << "Pouring into cup\n"; }

    virtual void brew() = 0;
    virtual void addCondiments() = 0;

    virtual ~Beverage() = default;
};

class Tea : public Beverage {
public:
    void brew() override { cout << "Steeping tea\n"; }
    void addCondiments() override { cout << "Adding lemon\n"; }
};
```
### ▶️ Usage

```cpp
Beverage* b = new Tea();
b->prepare();

// Output:
// Boiling water
// Steeping tea
// Pouring into cup
// Adding lemon

delete b;

```

---

# 🔟 Visitor Pattern

## ✅ Idea  
Use the **Visitor Pattern** when you have many object types and want to perform **operations on them without modifying their classes**.  

This is useful when adding new operations frequently (e.g., tax calculation, discounts, analytics, serialization).

---

## 🎯 Example  
Tax calculation for different item categories:

- 🍎 Food  
- 📱 Electronics  
- 👕 Clothes  

Each item stays unchanged, while new operations (like tax rules) are added through Visitors.

---

## 🧩 C++ Example (tiny)

```cpp
#include <iostream>
using namespace std;

class Food;
class Electronics;

class Visitor {
public:
    virtual void visit(Food* f) = 0;
    virtual void visit(Electronics* e) = 0;
};

class Item {
public:
    virtual void accept(Visitor* v) = 0;
    virtual ~Item() = default;
};

class Food : public Item {
public:
    void accept(Visitor* v) override { v->visit(this); }
};

class Electronics : public Item {
public:
    void accept(Visitor* v) override { v->visit(this); }
};

class TaxVisitor : public Visitor {
public:
    void visit(Food* f) override { cout << "Tax on Food\n"; }
    void visit(Electronics* e) override { cout << "Tax on Electronics\n"; }
};
```
### ▶️ Usage

```cpp
Item* i1 = new Food();
Item* i2 = new Electronics();
Visitor* tax = new TaxVisitor();

i1->accept(tax);    // Tax on Food
i2->accept(tax);    // Tax on Electronics

delete i1;
delete i2;
delete tax;

```
---

# 1️⃣1️⃣ Iterator Pattern

## ✅ Idea  
Provide a way to **traverse a collection** without exposing its internal structure.  
The user gets an iterator with methods like `next()` and `hasNext()` (or in C++: `begin()` and `end()`).

This keeps data encapsulated while still allowing sequential access.

---

## 🎯 Example  
Iterating through a list or array one element at a time.

---

## 🧩 C++ Example (simple custom iterator)

```cpp
#include <iostream>
using namespace std;

class MyCollection {
    int arr[5] = {1, 2, 3, 4, 5};
public:
    int* begin() { return &arr[0]; }
    int* end()   { return &arr[5]; }
};
```
### ▶️ Usage

```cpp
MyCollection c;

for (int* it = c.begin(); it != c.end(); ++it) {
    cout << *it << " ";
}

// Output:
// 1 2 3 4 5

```
---
