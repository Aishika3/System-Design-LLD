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

