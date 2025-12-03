## 1️⃣ Adapter Pattern

### 📌 Purpose

Convert an **incompatible interface** into one the client expects.  
Adapter acts like a **translator** between two different systems.

---

### 🎯 Real-life Example

A **mobile charger adapter** allows your charger to work with different plug types.

---

### ✔ Code-style Example

Your system expects a `JSONReader`, but a third-party library provides only an `XMLReader`.

```cpp
class XMLReader {
public:
    string readXML();
};
```
```cpp
class JSONAdapter {  // ADAPTER
    XMLReader* xml;
public:
    JSONAdapter(XMLReader* x) : xml(x) {}

    string readJSON() {
        string xmlData = xml->readXML();
        return convertXMLtoJSON(xmlData);  // pretend conversion
    }
};
```
---

## 2️⃣ Bridge Pattern

### 📌 Purpose

Separate **Abstraction** from **Implementation** so both can evolve independently.

This allows changing the abstraction (e.g., Remote) and implementation (e.g., TV brand) **without affecting each other**.

---

### 🎯 Real-life Example

A **remote control** (abstraction) can work with multiple TV brands (implementations):

- Samsung TV  
- Sony TV  

The remote **does not care** which TV model it is controlling.

---

### ✔ Code-style Example

```cpp
class TV {
public:
    virtual void turnOn() = 0;
};
```
```cpp
class SonyTV : public TV {
public:
    void turnOn() { /* Sony logic */ }
};
```
```cpp
class SamsungTV : public TV {
public:
    void turnOn() { /* Samsung logic */ }
};
```
```cpp
class Remote {          // Abstraction
    TV* tv;
public:
    Remote(TV* t) : tv(t) {}

    void on() {
        tv->turnOn();  // delegates to implementation
    }
};
```
---

## 3️⃣ Composite Pattern

### 📌 Purpose

Represent **tree-like structures**, where **individual objects** and **groups of objects** are treated **uniformly**.

This allows the client to interact with both **single items** and **collections** using the same interface.

---

### 🎯 Real-life Example

A **folder** can contain:

- Files  
- Other folders  

Yet you handle both using the same operations (open, delete, rename, etc.).

---

### ✔ Code-style Example

```cpp
class FileSystem {
public:
    virtual void show() = 0;
};
```
```cpp
class File : public FileSystem {
public:
    void show() {
        cout << "File\n";
    }
};
```
```cpp
class Folder : public FileSystem {
    vector<FileSystem*> children;
public:
    void add(FileSystem* f) {
        children.push_back(f);
    }

    void show() {
        for (auto c : children)
            c->show();
    }
};
```
---

## 4️⃣ Decorator Pattern

### 📌 Purpose

Add **extra functionality** to an object **dynamically** without modifying the original class.

This allows flexible and extensible feature addition at **runtime**.

---

### 🎯 Real-life Example

A coffee order where you can add features step-by-step:

- Coffee  
- + Milk  
- + Sugar  
- + Whipped Cream  

Each add-on decorates the original coffee.

---

### ✔ Code-style Example

```cpp
class Coffee {
public:
    virtual int cost() = 0;
};
```
```cpp
class BasicCoffee : public Coffee {
public:
    int cost() {
        return 50;
    }
};
```
```cpp
class MilkDecorator : public Coffee {
    Coffee* coffee;
public:
    MilkDecorator(Coffee* c) : coffee(c) {}

    int cost() {
        return coffee->cost() + 10;  // adds milk cost
    }
};
```
### ✔ Usage
```cpp
Coffee* c = new MilkDecorator(new BasicCoffee());
```
---

## 5️⃣ Facade Pattern

### 📌 Purpose

Provide a **simple, unified interface** to a **complex subsystem**.

The Facade hides multiple steps and exposes **one easy-to-use method** to the client.

---

### 🎯 Real-life Example

When you press **“START”** in a car:

- Fuel pump activates  
- Engine ignites  
- Sensors initialize  
- Electrical systems start  

But you only see **one button** — the Facade.

---

### ✔ Code-style Example

```cpp
class Engine {
public:
    void start();
};
```
```cpp
class FuelPump {
public:
    void pump();
};
```
```cpp
class CarFacade {   // FACADE
    Engine e;
    FuelPump f;
public:
    void startCar() {
        f.pump();
        e.start();
    }
};
```
### ✔ Client Calls

```cpp
CarFacade car;
car.startCar();
```
---

## 6️⃣ Flyweight Pattern

### 📌 Purpose

Reduce memory usage by **sharing common objects** instead of creating new ones each time.

Useful when you have **many repeated, similar objects**.

---

### 🎯 Real-life Example

A **text editor** stores:

- The shape of character `'A'` **once**
- But positions (x, y) separately for each occurrence  

This avoids creating thousands of duplicate `'A'` objects.

---

### ✔ Code-style Example

```cpp
class Character {
    char symbol;
public:
    Character(char c) : symbol(c) {}
};
```
```cpp
class CharacterFactory {
    unordered_map<char, Character*> pool;
public:
    Character* getChar(char c) {
        if (!pool[c])
            pool[c] = new Character(c);  // create once, reuse later
        
        return pool[c];  // shared instance
    }
};
```
---

## 7️⃣ Proxy Pattern

### 📌 Purpose

A **Proxy** acts *on behalf of* another object to **control access**, add extra logic, or improve performance.

Clients interact with the proxy instead of the real object.

---

### 🎯 Real-life Examples

- An **ATM card** acts as a proxy to your bank account  
- **Netflix** uses proxies to allow/block region-specific content  
- **Cache proxies** return data quickly without calling the real server each time  

---

### ✔ Code-style Example

```cpp
class RealImage {
public:
    void display() {
        cout << "Image Loaded\n";
    }
};
```
```cpp
class ImageProxy {
    RealImage* image = nullptr;

public:
    void display() {
        if (image == nullptr)
            image = new RealImage();   // Lazy loading: load only when needed

        image->display();
    }
};
```
---

### Summary

## ⭐ Structural Design Patterns Summary

| Pattern   | Purpose                                   |
|----------|--------------------------------------------|
| Adapter  | Converts interface (translator)             |
| Bridge   | Separates abstraction & implementation      |
| Composite| Tree structure (file/folder hierarchy)      |
| Decorator| Add features dynamically                    |
| Facade   | Simple interface to a complex subsystem     |
| Flyweight| Reduce memory via shared objects            |
| Proxy    | Access control, caching, lazy loading       |

---
