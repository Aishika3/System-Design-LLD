# System-Design-LLD

# ⭐ SOLID Principles – Full Notes

Here are **clean, easy, interview-ready notes for SOLID principles** — explained in simple language with examples and real-world use cases.

---

## 📌 What is SOLID?

SOLID = **5 design principles** that make your code:

✔ Maintainable  
✔ Extensible  
✔ Testable  
✔ Readable  
✔ Reusable  

---

# 1️⃣ S — Single Responsibility Principle (SRP)

**A class should have only one reason to change.**  
➡ One class = One job.

### ❌ Bad Example
A `Report` class that:
- creates report  
- prints it  
- sends email  

Too many responsibilities → violates SRP.

### ✔ Good Example
Split into:

- `ReportGenerator`  
- `ReportPrinter`  
- `ReportEmailer`  

Each class has **one responsibility**.

### 💡 Real-life Analogy
A waiter should not cook + clean + take payments.

---

# 2️⃣ O — Open/Closed Principle (OCP)

**A class should be open for extension but closed for modification.**

➡ You can **add new features** *without editing existing code*.

### ❌ Bad Example

```cpp
double area(Shape* s) {
    if(s->type == "circle") { ... }
    else if(s->type == "rectangle") { ... }
}
```
## 🚫 Bad Example (Violates OCP)

Adding a new shape requires **modifying** `area()` logic again and again →  
This **breaks Open/Closed Principle (OCP)**.

---

## ✅ Good Example (Follows OCP Using Polymorphism)

We create a **base class** with a virtual `area()` method.  
Each shape implements its own version.  
Now adding a new shape does **not** modify existing code.

```cpp
class Shape {
public:
    virtual double area() = 0;   // Abstract method
};

class Circle : public Shape {
public:
    double area() override {
        // Circle area logic
    }
};
```
