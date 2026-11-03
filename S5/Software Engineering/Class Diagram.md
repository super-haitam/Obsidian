
![[Pasted image 20261005235004.png]]

# Description
Here there are 3 classes, some attributes have a certain symbol next to them:
- `+`: Means *public*
- `-`: Means *private*
- `#`: Means *protected*

There are also links between the classes:
- **Line with arrow** means inheritance (e.g. the class Student extends Person and inherits all its attributes).
- **Multiplicity**: The * next to the class Student means each Major can be linked to many students, where as each student has exactly one Major. 
# How to read the notation
A class is a box with three compartments: name, attributes, methods.
┌─────────────────┐
│    Order                  │  ← name (italic or «abstract» if abstract)
├─────────────────┤
│ - customer             │  ← attributes   (- private, + public, ~ package)
├─────────────────┤
│ + pay()                   │  ← methods
└─────────────────┘
*Interface*
An interface is the same box with *Interface* above the name. 
### Arrows
Arrows are what matter, there a four:

| Arrow                                         | Meaning                                                 | Reading it                   |
| --------------------------------------------- | ------------------------------------------------------- | ---------------------------- |
| Solid line, **hollow triangle** at the parent | **Inheritance** (`extends`)                             | "Dish *is* a MenuItem"       |
| Dashed line, **hollow triangle**              | **Implements** an interface                             | "Dish *implements* MenuItem" |
| Solid line, **open arrow** (or plain line)    | **Association**: one class holds a reference to another | "Order *has* a State"        |
| Line with a **diamond** at the owner          | **Aggregation**: owner holds a collection of things     | "Combo *contains* items"     |
Add a number at the end of an association to show how many: `1` means exactly one, `*` means many.