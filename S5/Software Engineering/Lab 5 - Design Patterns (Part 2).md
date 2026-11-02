# Problem
**1.** The Test class has exactly the same output as shown.

**2. a.** Adding a new extra sauce requires a change in the code for the method `addDish` of the class `Order` by adding another if statement and another argument `sauce`.
And the adding of the functionality *double cheese* requires the modification of another argument `boolean doublecheese`. 
Both of these changes require the class to modify its code, violating the **Open/Closed principle**.

**b.** Adding a "Family Menu" will have to be accounted for in the `addDish` method, where `if (name == "Family Menu") { /* Add the 2 student menus and the salad rather than the name itself */ }`, and it also needs to account for any discount or choose just the sum.

**c.** To add another status "READY", we'd need to add a whole method `makeReady()` with similar logic than the methods `pay`, `startPreparing`, `deliver`, `cancel`... 

**d.** For a loyalty service to be notified, we would need to create a new class `SmsSenderLoyalty` to take care of this, which will be used right after each change of status. To switch the sms off, we'd need to add an attribute to either the `Order` or the `SmsSender` class, to check whether it is swiched on or off.

**3.** Find four patters: **two structural** and **two behavioral**.
- **Structural Design Patterns**: *Decorator*, *Composite*, ...
- **Behavioral Design Patterns**:...

| Change request      | Code smell | Pattern | Classes of the new design (role) |
| ------------------- | ---------- | ------- | -------------------------------- |
| a. extras           |            |         |                                  |
| b. nested combos    |            |         |                                  |
| c. new status       |            |         |                                  |
| d. new notification |            |         |                                  |


