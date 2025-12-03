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

## 🎯 Concept

Abstract Factory is used when you need to create **families of related objects**.

Each family has its own factory.

Example families:

- **Pet Family** → `Dog`, `DogFood`
- **Wild Family** → `Tiger`, `MeatFood`

The client never interacts with concrete classes directly — only with the factory.

---

# ✔ Easy Example: Animal Factory Family

---

## 1️⃣ Product Interfaces

```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    virtual void speak() = 0;
};

class Food {
public:
    virtual void eat() = 0;
};

```

## 2️⃣ Concrete Products (Two Families)

---

# ➤ 🐶 Family 1: Pet Animals

### **Dog (Animal Product)**

```cpp
class Dog : public Animal {
public:
    void speak() { cout << "Dog: Woof!\n"; }
};

```

### **DogFood (Food Product)**

```cpp
class DogFood : public Food {
public:
    void eat() { cout << "Eating Dog Food\n"; }
};
```
# ➤ 🐯 Family 2: Wild Animals  
Concrete Products for the **Abstract Factory Pattern**

This family represents **wild animals** and their matching **food products**.  
Each product belongs to the Wild Animal family and is used when the `WildFactory` is selected.

---

### Tiger (Animal Product)

```cpp
class Tiger : public Animal {
public:
    void speak() { cout << "Tiger: Roar!\n"; }
};
```

### Meat (Food Product)

```cpp
class Meat : public Food {
public:
    void eat() { cout << "Eating Meat\n"; }
};
```
---

# 🏭 3️⃣ Abstract Factory (Factory of Factories)

The abstract factory declares methods to create **each product type** in a family.

```cpp
class AnimalFactory {
public:
    virtual Animal* createAnimal() = 0;
    virtual Food* createFood() = 0;
};
```
# 🏗️ 4️⃣ Concrete Factories  
Concrete factories create **matching sets of related products** for each family.  
In this Abstract Factory example, we have two product families:

- 🐶 **Pet Family** → Dog + DogFood  
- 🐯 **Wild Family** → Tiger + Meat  

Each factory ensures **family consistency**, meaning all created objects belong to the same group.

---

## 🐶 ➤ Pet Factory

Creates **Dog** (Animal) + **DogFood** (Food):

```cpp
class PetFactory : public AnimalFactory {
public:
    Animal* createAnimal() { return new Dog(); }
    Food* createFood() { return new DogFood(); }
};
```

# 🐯 ➤ Wild Factory  
Concrete Factory for the **Wild Animals Family**  
(Abstract Factory Pattern - C++)

The Wild Factory creates **matching wild-animal products**, ensuring they belong to the same object family.

It produces:

- 🐯 **Tiger** → Animal product  
- 🍖 **Meat** → Food product  

---

## 🏗️ Wild Factory (Concrete Factory)

```cpp
class WildFactory : public AnimalFactory {
public:
    Animal* createAnimal() { return new Tiger(); }
    Food* createFood() { return new Meat(); }
};
```

### ✔ Usage (Very Simple)

```cpp
int main() {
    AnimalFactory* factory;

    factory = new PetFactory();
    Animal* a1 = factory->createAnimal();
    Food* f1 = factory->createFood();
    a1->speak();   // Dog: Woof!
    f1->eat();     // Eating Dog Food

    factory = new WildFactory();
    Animal* a2 = factory->createAnimal();
    Food* f2 = factory->createFood();
    a2->speak();   // Tiger: Roar!
    f2->eat();     // Eating Meat

    return 0;
}
```

---



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
## #️⃣ 5. Prototype Pattern

### ✅ What it is

The **Prototype Pattern** is used to **clone existing objects** instead of creating them from scratch.  
It provides a mechanism to duplicate objects efficiently.

---

### 📌 Good When

- Object creation is **expensive or time-consuming**
- Many similar objects are needed
- Copying is faster and more efficient than constructing new instances

---

### ✔ Example

```cpp
class Shape {
public:
    virtual Shape* clone() = 0;
};
class Circle : public Shape {
public:
    Shape* clone() {
        return new Circle(*this);  // uses copy constructor
    }
};

```
