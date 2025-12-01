# 📘 Design Patterns 
---

## #️⃣ 1. Singleton Pattern

### ✅ What it is
Ensures **only one instance** of a class exists and provides a **global access point** to it.

### 📌 Use Cases
- Logger  
- Database Connection Pool  
- Configuration Manager  

### ✔ Example: Logger Singleton (Pseudocode)

```cpp
class Logger {
private:
    static Logger* instance;
    Logger() {}   // private constructor

public:
    static Logger* getInstance() {
        if(instance == nullptr) {
            instance = new Logger();
        }
        return instance;
    }

    void log(string msg) {
        cout << msg << endl;
    }
};
```
## #️⃣ 2. Factory Pattern

### ✅ What it is

The Factory Pattern **creates objects through a dedicated factory class**, hiding the object creation logic from the client.

It ensures that the client never directly uses `new` to create objects.

---

### 📌 Why Used

- Avoid using `new` everywhere  
- Encapsulate and centralize object creation  
- Supports **Open/Closed Principle (OCP)**  
- Easy to extend by adding new object types  

---

### ✔ Example: Shape Factory

```cpp
class Shape {
public:
    virtual void draw() = 0;
};
class Circle : public Shape {
public:
    void draw() { cout << "Circle\n"; }
};
class Rectangle : public Shape {
public:
    void draw() { cout << "Rectangle\n"; }
};
class ShapeFactory {
public:
    static Shape* getShape(string type) {
        if (type == "circle") return new Circle();
        if (type == "rectangle") return new Rectangle();
        return nullptr;
    }
};
```
### Usage

```cpp
Shape* s = ShapeFactory::getShape("circle");
s->draw();
```

## #️⃣ 3. Abstract Factory Pattern

### ✅ What it is

The **Abstract Factory Pattern** is a **factory of factories**.  
It provides an interface to create **families of related objects** without exposing their concrete classes.

You use it when the system needs to be **independent of how objects are created** and when different object families must be supported.

---

### 📌 Example Use Case

**UI Themes**

Each theme produces a **set of related UI components**:

- **Light Theme**
  - Light Button
  - Light Checkbox
- **Dark Theme**
  - Dark Button
  - Dark Checkbox

The client only interacts with the factory — not the specific component classes.

---

### ✔ Example (Conceptual)

```cpp
class UIFactory {
public:
    virtual Button* createButton() = 0;
    virtual Checkbox* createCheckbox() = 0;
};
class DarkFactory : public UIFactory {
public:
    Button* createButton() { return new DarkButton(); }
    Checkbox* createCheckbox() { return new DarkCheckbox(); }
};

```
### 🎯 Why It’s Useful

- Ensures **consistent object families** (e.g., always Dark theme components)
- Promotes **loose coupling**
- Makes adding new themes **easy** without modifying existing code (supports **Open/Closed Principle**)

---

### ✔ Client Example (Conceptual)

```cpp
UIFactory* factory = new DarkFactory();

Button* btn = factory->createButton();
Checkbox* chk = factory->createCheckbox();

```

## #️⃣ 4. Builder Pattern

### ✅ What it is

The **Builder Pattern** helps create **complex objects step-by-step** without writing long and messy constructors.

It is especially useful when objects have **many optional parameters**.

---

### 📌 Why Used

- Avoid long “telescopic constructors”
- Improve readability and maintainability
- Build objects in a **flexible, step-by-step** manner
- Clearly separate object construction from representation

---

### ✔ Example: User Builder

```cpp
class User {
private:
    string name;
    int age;
    string email;

public:
    class Builder {
        string name;
        int age = 0;
        string email;

    public:
        Builder& setName(string n) { 
            name = n; 
            return *this; 
        }

        Builder& setAge(int a) { 
            age = a; 
            return *this; 
        }

        Builder& setEmail(string e) { 
            email = e; 
            return *this; 
        }

        User build() { 
            return User(name, age, email); 
        }
    };
};
```

### ✔ Usage

```cpp
User u = User::Builder()
            .setName("Aishika")
            .setAge(22)
            .build();
```
