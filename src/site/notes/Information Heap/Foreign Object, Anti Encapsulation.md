---
{"dg-publish":true,"permalink":"/information-heap/foreign-object-anti-encapsulation/","noteIcon":"","created":"2025-10-30T15:30:00.984-05:00"}
---

You are making a crucial observation about how **object context and state are handled** in method calls, and you are absolutely right: passing a second object (`other`) into a method on the first object (`client.calc_salted_number(other, ...)`) is a significant step toward spaghetti code. 🍝

The difference lies in **Object Responsibility** and **Encapsulation**.

---

## 🏛️ Two Different Models of Responsibility

|**Pattern**|**client.calc_salted_number(magic_number=0)**|**client.calc_salted_number(other, magic_number=0)**|
|---|---|---|
|**Method Type**|Bound Instance Method (`self` is `client`)|Bound Instance Method (`self` is `client`)|
|**Parameters**|State data only (`magic_number`)|State data AND another object (`other`)|
|**Assignments**|`self.salted_number = ...` (Assigns state to `client`)|`self.magic_number = ...` (Assigns state to `client`) **AND** `other.magic_number = ...` (Assigns state to `other`)|
|**Responsibility**|`client` is responsible for its own state based on input. **Good.**|`client`'s method is responsible for changing state on _both_ `client` and an _external_ object (`other`). **Bad.**|

The first pattern is standard OOP: the object acts on itself using its own data and parameters. The second pattern is a serious violation of good design principles.

---

## 🚫 Why the Second Pattern is "Spaghetti in the Making"

The danger of passing `other` into a method of `client` is that you are breaking fundamental OOP rules:

### 1. ⚠️ Violation of Encapsulation

- **Encapsulation** means an object's internal state (its attributes) should only be managed by its own methods.
    
- In the second pattern, `client`'s method (`calc_salted_number`) is directly reaching into the private state of `other` and changing it (`other.magic_number = ...`).
    
- The `Other` class loses control over when and how its state is changed. This makes debugging a nightmare because you have to look inside the `Client` class to understand why the `Other` object changed.
    

### 2. 🧩 Confused Responsibility

- The method's name, `calc_salted_number`, suggests it should calculate and maybe set a number related to the **client**.
    
- Instead, it's performing a "side effect" on an entirely different object (`other`). A method should have **one clear purpose**. A function that modifies two unrelated objects has confused responsibilities.
    

### 3. 📉 Reduced Testability

- To test `client.calc_salted_number(other, ...)`, you now need to instantiate and pass in a valid `other` object, and you need to check the state of _both_ the `client` _and_ the `other` object after the function runs. This complexity makes unit testing much harder.
    

---

## ✅ The Correct Design (If Two Objects Interact)

If you need two objects to interact, the change in the state of `other` should typically be handled by a method _on_ the `other` object.

|**Action**|**Code**|**Principle**|
|---|---|---|
|**Client is Primary:**|`client.salted_number = client.calc_salted_number(other.data)`|Client uses a piece of `other`'s data to calculate its own state.|
|**Other is Updated:**|`other.update_state(new_value)`|Client calculates the necessary change, but then tells `other` to handle the update itself via its own method.|

Your instinct is excellent: passing a foreign object into a method and changing its attributes inside that method is a **critical code smell** that leads to untraceable dependencies and tangled state management.

Date: [[-Daily Activity Log-/2025 10-October 30\|2025 10-October 30]]

[[AI-generated/Example of Good Foreign Object Handling\|Example of Good Foreign Object Handling]]