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

