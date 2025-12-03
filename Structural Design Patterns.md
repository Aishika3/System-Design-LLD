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
