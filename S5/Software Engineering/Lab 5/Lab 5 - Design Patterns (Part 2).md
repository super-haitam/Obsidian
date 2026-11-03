## Deliverables
You will find the whole project with the `problem1before` and `problem1after` in the github link: https://github.com/super-haitam/lab4, the table is below, as well as the class diagram in PNG format. This file covers the answers to the questions. Thank you!!

---
# Problem
**1.** The Test class has exactly the same output as shown.

**2. a.** Adding a new extra sauce requires a change in the code for the method `addDish` of the class `Order` by adding another if statement and another argument `sauce`.
And the adding of the functionality *double cheese* requires the modification of another argument `boolean doublecheese`. 

**b.** Adding a "Family Menu" will have to be accounted for in the `addDish` method, where `if (name == "Family Menu") { /* Add the 2 student menus and the salad rather than the name itself */ }`, and it also needs to account for any discount or choose just the sum. But this wouldn't work if "Student Menu" followed the same logic. It is then impossible.

**c.** To add another status "READY", we'd need to add a whole method `makeReady()` with similar logic than the methods `pay`, `startPreparing`, `deliver`, `cancel`... 

**d.** For a loyalty service to be notified, we would need to create a new class `SmsSenderLoyalty` to take care of this, which will be used right after each change of status. To switch the SMS off, we'd need to add an attribute to either the `Order` or the `SmsSender` class, to check whether it is switched on or off.

All of the straightforward solutions to these problems violate the *Open/Closed Principle*.

**3.** Find four patters: **two structural** and **two behavioral**.
- **Structural Design Patterns**: *Decorator*, *Composite*, ...
- **Behavioral Design Patterns**:...

| Change request      | Code smell             | Pattern   | Classes of the new design (role)                                                                               |
| ------------------- | ---------------------- | --------- | -------------------------------------------------------------------------------------------------------------- |
| a. extras           | Makes change difficult | Decorator | Dish (Interface), SimpleDish, DishDecorator, CheeseDecorator, SpicyDecorator, SauceDecorator                   |
| b. nested combos    | Makes change difficult | Composite | RestaurantComponent (Interface), Dish, Menu                                                                    |
| c. new status       | Makes change difficult | State     | State (Inteface), Context (here, `Order`), ReadyState, PreaparingState, DeliveredState, ...                    |
| d. new notification | Makes change difficult | Observer  | Subject (Interface), Order will implement Subject, Observer (Interface), ClientObserver, LoyalyServiceObserver |
**a. extras**: If we were to want to divide the doing of the `addDish()` method into classes using OOP, we'd need to make the dishes dynamically wrap-able by the toppings, instead of creating a class for each of the decorators, which would cause *code explosion*. This is exactly the **Decorator** design.
**b. nested combos**: This shows a dish can either be a menu or a plate, thus, making the price calculation messy and hard to get, especially with the discount. The solution comes premade considering the **Composite** pattern, which lets us consider the problem as a tree of components.
**c. new status**: The `Order` class, has a few methods that act differently depending on the internal state of the attribute `status`, making them prone to long `if-else` clauses, and hard to change. This makes them perfect for the **State** design.
**d. new notification**: Additional to the `SmsSender` class, it seems that we need to broadcast changes in `status` to a loyalty service, only, the `Order` class doesn't care about what the SMS classes do. Which simply fits the **Observer** by design.

**4.** I have drawn the 4 class diagrams, each corresponding to a design pattern:
**Decorator:**
![[Decorator.png]]

**Observer**
![[Observer.png]]

**State**
![[State.png]]

**Composite**
![[Composite.png]]

The final class diagram:
![[Class Diagram.png]]

The repeating classes are:
- **MenuItem**: both in Decorator and Composite.
- **Order**: both in State and Observer.

**5.** See the Code part.

**6.** The outermost object is Spicy as it's the last word in the name is "Tacos + cheese + spicy".

**7.** A combo can contain a dish with cheese because it is an instance of `MenuItem` which is what combo allows in its internal structure, wraping it in `Cheese` would add 5 to its price.
Well, technically, as *studentMenu* would be of type Combo (an implementation of MenuItem), then it would be possible, although it doesn't make sense for the restaurant.

**8.** What we'll do:
- create a new class Sauce with name "sauce" and price 3, as for the double cheese, with our current implementation, the user can add even triple cheese and it would be fine, which doesn't seem to be prohibited by the specifications either.
- a family menu can be done by the client without any change of code, just create a combo that contains a combo and a dish, that's all, the code handles the discount.
- for the new READY status, we'd only need to add a method to Order as well as create a new ReadyState as well, i don't think the prohibition of cancelling under ready state makes sense as it is the default, it probably was difficult before the refactoring, also the first change might violate Open/Closed but I don't think there is much we can do as it is core to the logic of Order.
- adding a loyalty service works the same for KitchenScreen and CustomerSms, one line. switching off is just one method away.

**9.**
**Singleton for KitchenScreen: no.** A Singleton means only one object can ever exist. But a restaurant could have more than one kitchen screen, and our Observer pattern already handles that, because any screen can just subscribe. A Singleton would only make the code harder to change and test.

**Builder for Order: no.** A Builder is for objects that need lots of settings when you create them. Our `Order` only needs one thing, the customer name, so a Builder would just add extra classes for nothing. Keeping the design simple is better.