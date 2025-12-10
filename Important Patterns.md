
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

---

## 3️⃣ Abstract Factory Pattern

### **What it is**
A **factory of factories** that creates families of related objects without specifying concrete classes.

### **Why used**
- UI themes (Dark Button + Dark Checkbox)  
- Payment ecosystem object groups  

### **Example**
`UIFactory → DarkFactory or LightFactory`

---

## 4️⃣ Builder Pattern

### **What it is**
Builds **complex objects step-by-step** without long constructors.

### **Why used**
- Many optional fields  
- Avoid telescopic constructors  

### **Example**
```cpp
User u = User::Builder().setName("Aishika").setAge(22).build();
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

---

## 8️⃣ Decorator Pattern

### **What it is**
Adds **new features dynamically** without modifying existing classes.

### **Why used**
- Coffee add-ons  
- Logger enhancements (timestamps, file logging)  

### **Example**
`new MilkDecorator(new BasicCoffee())`

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
Folder.add(File);
Folder.add(AnotherFolder);
```
---
