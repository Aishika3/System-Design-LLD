
### ⭐ Must-Know Design Patterns (Top 12)

Below are the most important patterns frequently asked in LLD interviews.

---

## 1️⃣ Singleton Pattern

### **What it is**
Ensures **only one instance** of a class exists in the entire application.

### **Why used**
- Only one logger  
- Only one DB connection pool  
- Only one config manager  

### **Key Point**
- Private constructor + static instance
### Example
```cpp
#include <iostream>
using namespace std;

class Logger {
private:
    Logger() {}   // private constructor

public:
    static Logger& getInstance() {
        static Logger instance;
        return instance;
    }

    void log(const string& msg) {
        cout << "Log: " << msg << endl;
    }
};

int main() {
    Logger& logger1 = Logger::getInstance();
    Logger& logger2 = Logger::getInstance();

    logger1.log("Application started");

    if (&logger1 == &logger2) {
        cout << "Same instance" << endl;
    }
}
```

---

## 2️⃣ Factory Pattern

### **What it is**
Provides a **centralized place** to create objects without exposing creation logic.

### **Why used**
- Avoid repeating `new`  
- Choose object type at runtime  
- Supports OCP (add classes without modifying old code)

### **Example**
`ShapeFactory → create(circle/rectangle)`

### Example code
```cpp
#include <iostream>
using namespace std;

class Shape {
public:
    virtual void draw() = 0;
    virtual ~Shape() {}
};

class Circle : public Shape {
public:
    void draw() override {
        cout << "Drawing Circle" << endl;
    }
};

class Rectangle : public Shape {
public:
    void draw() override {
        cout << "Drawing Rectangle" << endl;
    }
};

class ShapeFactory {
public:
    static Shape* createShape(const string& type) {
        if (type == "circle") return new Circle();
        if (type == "rectangle") return new Rectangle();
        return nullptr;
    }
};

int main() {
    Shape* s1 = ShapeFactory::createShape("circle");
    Shape* s2 = ShapeFactory::createShape("rectangle");

    s1->draw();
    s2->draw();

    delete s1;
    delete s2;
}
```

---

## 3️⃣ Abstract Factory Pattern

### **What it is**
A **factory of factories** that creates families of related objects without specifying concrete classes.

### **Why used**
- UI themes (Dark Button + Dark Checkbox)  
- Payment ecosystem object groups  

### **Example**
`UIFactory → DarkFactory or LightFactory`
### Example
```cpp
#include <iostream>
using namespace std;

class Button {
public:
    virtual void render() = 0;
    virtual ~Button() {}
};

class Checkbox {
public:
    virtual void render() = 0;
    virtual ~Checkbox() {}
};

class DarkButton : public Button {
public:
    void render() override { cout << "Dark Button" << endl; }
};

class LightButton : public Button {
public:
    void render() override { cout << "Light Button" << endl; }
};

class DarkCheckbox : public Checkbox {
public:
    void render() override { cout << "Dark Checkbox" << endl; }
};

class LightCheckbox : public Checkbox {
public:
    void render() override { cout << "Light Checkbox" << endl; }
};

class UIFactory {
public:
    virtual Button* createButton() = 0;
    virtual Checkbox* createCheckbox() = 0;
    virtual ~UIFactory() {}
};

class DarkFactory : public UIFactory {
public:
    Button* createButton() override { return new DarkButton(); }
    Checkbox* createCheckbox() override { return new DarkCheckbox(); }
};

class LightFactory : public UIFactory {
public:
    Button* createButton() override { return new LightButton(); }
    Checkbox* createCheckbox() override { return new LightCheckbox(); }
};

int main() {
    UIFactory* factory = new DarkFactory();

    Button* btn = factory->createButton();
    Checkbox* cb = factory->createCheckbox();

    btn->render();
    cb->render();

    delete btn;
    delete cb;
    delete factory;
}
```
---

## 4️⃣ Builder Pattern

### **What it is**
Builds **complex objects step-by-step** without long constructors.

### **Why used**
- Many optional fields  
- Avoid telescopic constructors  

### **Example**
```cpp
#include <iostream>
using namespace std;

class User {
private:
    string name;
    int age;
    string email;

    User(string n, int a, string e) : name(n), age(a), email(e) {}

public:
    void show() {
        cout << "Name: " << name << ", Age: " << age << ", Email: " << email << endl;
    }

    class Builder {
    private:
        string name = "";
        int age = 0;
        string email = "";
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

int main() {
    User u = User::Builder().setName("Aishika").setAge(22).setEmail("a@gmail.com").build();
    u.show();
}
```
---

## 5️⃣ Prototype Pattern

### **What it is**
Creates new objects by **cloning existing ones** instead of building from scratch.

### **Why used**
- Object creation is expensive  
- Avoid repeated initialization/setup  
- Useful for creating many similar objects  

### **Example**
`Shape prototype → clone()`
```cpp
#include <iostream>
using namespace std;

class Shape {
public:
    virtual Shape* clone() = 0;
    virtual void draw() = 0;
    virtual ~Shape() {}
};

class Circle : public Shape {
private:
    int radius;
public:
    Circle(int r) : radius(r) {}

    Shape* clone() override {
        return new Circle(*this);
    }

    void draw() override {
        cout << "Circle with radius " << radius << endl;
    }
};

int main() {
    Shape* original = new Circle(10);
    Shape* copy = original->clone();

    original->draw();
    copy->draw();

    delete original;
    delete copy;
}
```

---

## 6️⃣ Strategy Pattern

### **What it is**
Allows selecting **one of many algorithms at runtime**.

### **Why used**
- Payment strategy (UPI / Card / Cash)  
- Sorting strategy  
- Routing algorithm  

### **Example**
`paymentService.setStrategy(new UpiPayment());`
```cpp
#include <iostream>
using namespace std;

class PaymentStrategy {
public:
    virtual void pay(int amount) = 0;
    virtual ~PaymentStrategy() {}
};

class UpiPayment : public PaymentStrategy {
public:
    void pay(int amount) override {
        cout << "Paid " << amount << " using UPI" << endl;
    }
};

class CardPayment : public PaymentStrategy {
public:
    void pay(int amount) override {
        cout << "Paid " << amount << " using Card" << endl;
    }
};

class PaymentService {
private:
    PaymentStrategy* strategy;
public:
    void setStrategy(PaymentStrategy* s) {
        strategy = s;
    }

    void makePayment(int amount) {
        strategy->pay(amount);
    }
};

int main() {
    PaymentService service;
    UpiPayment upi;
    CardPayment card;

    service.setStrategy(&upi);
    service.makePayment(500);

    service.setStrategy(&card);
    service.makePayment(1000);
}
```

---

## 7️⃣ Observer Pattern

### **What it is**
A **one-to-many relationship**:  
When one object changes, it **notifies all observers**.

### **Why used**
- Notification system  
- Stock price updates  
- UI event listeners  

### **Example**
`NotificationService.notify("Payment Successful!");`
```cpp
#include <iostream>
#include <vector>
using namespace std;

class Observer {
public:
    virtual void update(string message) = 0;
    virtual ~Observer() {}
};

class User : public Observer {
private:
    string name;
public:
    User(string n) : name(n) {}

    void update(string message) override {
        cout << name << " received: " << message << endl;
    }
};

class NotificationService {
private:
    vector<Observer*> observers;
public:
    void subscribe(Observer* obs) {
        observers.push_back(obs);
    }

    void notifyAll(string message) {
        for (auto obs : observers) {
            obs->update(message);
        }
    }
};

int main() {
    NotificationService service;
    User u1("x"), u2("y");

    service.subscribe(&u1);
    service.subscribe(&u2);

    service.notifyAll("Payment Successful!");
}
```

---

## 8️⃣ Decorator Pattern

### **What it is**
Adds **new features dynamically** without modifying existing classes.

### **Why used**
- Coffee add-ons  
- Logger enhancements (timestamps, file logging)  

### **Example**
`new MilkDecorator(new BasicCoffee())`
```cpp
#include <iostream>
using namespace std;

class Coffee {
public:
    virtual string getDescription() = 0;
    virtual int cost() = 0;
    virtual ~Coffee() {}
};

class BasicCoffee : public Coffee {
public:
    string getDescription() override {
        return "Basic Coffee";
    }

    int cost() override {
        return 50;
    }
};

class CoffeeDecorator : public Coffee {
protected:
    Coffee* coffee;
public:
    CoffeeDecorator(Coffee* c) : coffee(c) {}
};

class MilkDecorator : public CoffeeDecorator {
public:
    MilkDecorator(Coffee* c) : CoffeeDecorator(c) {}

    string getDescription() override {
        return coffee->getDescription() + " + Milk";
    }

    int cost() override {
        return coffee->cost() + 20;
    }
};

int main() {
    Coffee* coffee = new MilkDecorator(new BasicCoffee());
    cout << coffee->getDescription() << endl;
    cout << "Cost: " << coffee->cost() << endl;
    delete coffee;
}
```
---

## 9️⃣ Adapter Pattern

### **What it is**
Converts one interface into another that the client expects.

### **Why used**
- Integrate third-party APIs  
- Convert XML → JSON  
- Work with legacy systems  

### **Example**
`JSONAdapter(XMLReader)`
```cpp
#include <iostream>
using namespace std;

class XMLReader {
public:
    string readXML() {
        return "<data>XML Data</data>";
    }
};

class JSONData {
public:
    virtual string getJSON() = 0;
    virtual ~JSONData() {}
};

class JSONAdapter : public JSONData {
private:
    XMLReader* xmlReader;
public:
    JSONAdapter(XMLReader* x) : xmlReader(x) {}

    string getJSON() override {
        string xml = xmlReader->readXML();
        return "{ converted: '" + xml + "' }";
    }
};

int main() {
    XMLReader xml;
    JSONData* adapter = new JSONAdapter(&xml);

    cout << adapter->getJSON() << endl;
    delete adapter;
}
```

---

## 🔟 Proxy Pattern

### **What it is**
A **substitute or wrapper** that controls access to the real object.

### **Why used**
- Caching  
- Security & access control  
- Lazy loading  

### **Example**
`ImageProxy loads RealImage only when needed`
```cpp
#include <iostream>
using namespace std;

class Image {
public:
    virtual void display() = 0;
    virtual ~Image() {}
};

class RealImage : public Image {
private:
    string filename;
public:
    RealImage(string file) : filename(file) {
        cout << "Loading image from disk: " << filename << endl;
    }

    void display() override {
        cout << "Displaying " << filename << endl;
    }
};

class ImageProxy : public Image {
private:
    RealImage* realImage = nullptr;
    string filename;
public:
    ImageProxy(string file) : filename(file) {}

    void display() override {
        if (realImage == nullptr) {
            realImage = new RealImage(filename);
        }
        realImage->display();
    }

    ~ImageProxy() {
        delete realImage;
    }
};

int main() {
    Image* img = new ImageProxy("photo.jpg");
    img->display();
    img->display();
    delete img;
}
```

---

## 1️⃣1️⃣ State Pattern

### **What it is**
Object changes its **behavior** depending on its **internal state**.

### **Why used**
- Traffic lights  
- Vending machines  
- Order lifecycle (Created → Shipped → Delivered)  

### **Example**
`trafficLight.setState(GreenState)`
```cpp
#include <iostream>
using namespace std;

class TrafficLight;

class State {
public:
    virtual void handle(TrafficLight* light) = 0;
    virtual ~State() {}
};

class TrafficLight {
private:
    State* state;
public:
    TrafficLight(State* s) : state(s) {}

    void setState(State* s) {
        state = s;
    }

    void request() {
        state->handle(this);
    }
};

class RedState : public State {
public:
    void handle(TrafficLight* light) override;
};

class GreenState : public State {
public:
    void handle(TrafficLight* light) override;
};

void RedState::handle(TrafficLight* light) {
    cout << "Red -> Green" << endl;
    light->setState(new GreenState());
}

void GreenState::handle(TrafficLight* light) {
    cout << "Green -> Red" << endl;
    light->setState(new RedState());
}

int main() {
    TrafficLight light(new RedState());
    light.request();
    light.request();
}
```

---

## 1️⃣2️⃣ Composite Pattern

### **What it is**
Treat **single objects** and **groups of objects** in a **uniform way**.

### **Why used**
- File system (Folder → Files + Subfolders)  
- Organization hierarchy  
- UI component trees  

### **Example**
```cpp
#include <iostream>
#include <vector>
using namespace std;

class FileSystem {
public:
    virtual void show() = 0;
    virtual ~FileSystem() {}
};

class File : public FileSystem {
private:
    string name;
public:
    File(string n) : name(n) {}

    void show() override {
        cout << "File: " << name << endl;
    }
};

class Folder : public FileSystem {
private:
    string name;
    vector<FileSystem*> items;
public:
    Folder(string n) : name(n) {}

    void add(FileSystem* item) {
        items.push_back(item);
    }

    void show() override {
        cout << "Folder: " << name << endl;
        for (auto item : items) {
            item->show();
        }
    }
};

int main() {
    File* f1 = new File("resume.pdf");
    File* f2 = new File("photo.png");

    Folder* folder1 = new Folder("Documents");
    Folder* folder2 = new Folder("Root");

    folder1->add(f1);
    folder2->add(folder1);
    folder2->add(f2);

    folder2->show();
}
```
---
